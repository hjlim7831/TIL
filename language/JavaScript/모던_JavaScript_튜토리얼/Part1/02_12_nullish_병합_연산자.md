- nullish 병합 연산자(nullish coalescing operator) `??`를 사용하면 짧은 문법으로 여러 피연산자 중 그 값이 '확정 되어있는' 변수를 찾을 수 있음
- `a ?? b`의 평가 결과
    - `a`가 `null`도 아니고 `undefined`도 아니면 `a`
    - 그 외의 경우에는 `b`
- nullish 병합 연산자 `??`없이 `x = a ?? b`와 동일한 동작을 하는 코드를 작성하면
    ``` javascript
    x = (a !== null && a !== undefined) ? a : b;
    ```
    - 비교 연산자와 논리 연산자만으로 nullish 병합 연산자와 같은 기능을 하는 코드를 작성하니, 코드 길이가 길어짐

- 또 다른 예시
    ``` javascript
    let firstName = null;
    let lastName = null;
    let nickName = "바이올렛";

    // null이나 undefined가 아닌 첫 번째 피연산자
    alert(firstName ?? lastName ?? nickName ?? "익명의 사용자");
    ```

### 1. '??'' 와 '||'의 차이
- nullish 병합 연산자는 OR 연산자 `||`와 상당히 유사해보임
- 실제로 위 예시에서 `??`를 `||`로 바꿔도 그 결과는 동일하기까지 함
- `??` 와 `||`의 차이점
    - `||`는 첫 번째 truthy 값을 반환
    - `??`는 첫 번쨰 정의된(defined) 값을 반환
- `null`과 `undefined`, 숫자 `0`을 구분 지어 다뤄야 할 때 이 차이점은 매우 중요한 역할을 함
- 예시
    ``` javascript
    height = height ?? 100;
    ```
    - `height`에 값이 정의되지 않은 경우, `height`엔 `100`이 할당됨

- `??`와 `||` 비교
    ``` javascript
    let height = 0;

    alert(height || 100); // 100
    alert(height ?? 100); // 0
    ```

### 2. 연산자 우선순위
- `??`의 연산자 우선순위는 `5`로 꽤 낮음
- `??`는 `=`와 `?`보다는 먼저, 대부분의 연산자보단 나중에 평가됨
- 복잡한 표현식 안에서 `??`를 사용해 값을 하나 선택할 땐 괄호를 추가하는 게 좋음
    ``` javascript
    let height = null;
    let width = null;

    let area = (height ?? 100) * (width ?? 50);
    alert(area); // 5000
    ```

- 그렇지 않으면 `*`가 `??`보다 우선순위가 높기 때문에, `*`가 먼저 실행됨
    ``` javascript
    let area = height ?? (100 * width) ?? 50;
    ```

- 안전성 관련 이슈로 인해, `??`는 `&&`나 `||`와 함께 사용하지 못함
    ``` javascript
    let x = 1 && 2 ?? 3; // SyntaxError: Unexpected token '??'
    ```
    - 제약을 피하려면, 괄호 사용하기!