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