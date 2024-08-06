### 1. 'while' 반복문
- `while` 반복문의 문법
    ``` javascript
    while (condition) {
        // 코드
        // '반복문 본문(body)'이라 불림
    }
    ```
    - `condition`(조건)이 truthy이면 반복문 본문의 `코드`가 실행됨
- 아래 반복문은 조건 `i < 3`을 만족할 동안 `i`를 출력해줌
    ``` javascript
    let i = 0;
    while (i < 3) {
        alert( i );
        i++;
    }
    ```

- 반복문 본문이 한 번 실행되는 것 : 반복(iteration, 이터레이션)이라 부름
- 위 예시에선 반복문이 세 번의 이터레이션을 만듦
- `i++`가 없었다면, 이론적으로 반복문이 영원히 반복되었을 것
- 브라우저는 이런 무한 반복을 멈추게 해주는 실질적인 수단을 제공
- 서버 사이드 자바 스크립트도 이런 수단을 제공해주므로, 무한으로 반복되는 프로세스를 종료할 수 있음
- 반복문 조건엔 비교뿐만 아니라 모든 종류의 표현식, 변수가 올 수 있음
- 조건은 `while`에 의해 평가되고, 평가 후엔 불린값으로 변경됨
- 아래 예시에선 `while (i != 0)`을 짧게 줄여 `while(i)`로 만들어 봄
    ``` javascript
    let i = 3;
    while (i) {
        alert( i );
        i--;
    }
    ```
### 2. 'do...while' 반복문
- `do..while` 문법을 사용하면 `condition`을 반복문 본문 아래로 옮길 수 있음
    ``` javascript
    do {
        // 반복문 본문
    } while (condition);
    ```
    - 본문이 먼저 실행되고, 조건을 확인한 후 조건이 truthy인 동안엔 본문이 계속 실행됨
- 예시
    ``` javascript
    let i = 0;
    do {
        alert( i );
        i++;
    } while (i < 3);
    ```
- `do..while` 문법은 조건이 truthy인지 아닌지에 상관 없이, 본문을 **최소한 한 번**이라도 실행하고 싶을 때만 사용해야 함
- 대다수의 상황에선 `do..while`보다 `while(...) {...}`이 적합함

### 3. 'for' 반복문
- `for` 반복문은 `while` 반복문보다는 복잡하지만, 가장 많이 쓰이는 반복문
    ``` javascript
    for (begin; condition; step) {
        // ... 반복문 본문 ...
    }
    ```
- `for`문을 구성하는 각 요소가 무엇을 의미하는지 알아보자
- 아래 반복문을 실행하면 i가 0부터 3이 될 때까지(단, `3`은 포함하지 않음) `alert(i)`가 호출됨
    ``` javascript
    for (let i = 0; i < 3; i++) {
        alert(i);
    }
    ```
- `for` 문의 구성 요소

| 구성 | 예시 | 설명 |
| --- | --- | --- |
| begin | i = 0 | 반복문에 진입할 때 단 한 번 실행됨 |
| condition | i < 3 | 반복마다 해당 조건이 확인됨. false이면 반복문을 멈춤 |
| body | alert(i) | condition이 truthy일 동안 계속해서 실행됨 |
| step | i++ | 각 반복의 body가 실행된 이후에 실행됨 |

- 일반적인 반복문 알고리즘
    ```
    begin을 실행함
    -> (condition이 truthy이면 -> body를 실행한 후 -> step을 실행함)
    -> (condition이 truthy이면 -> body를 실행한 후 -> step을 실행함)
    -> (condition이 truthy이면 -> body를 실행한 후 -> step을 실행함)
    -> ...
    ```

- 거치는 과정
    ``` javascript
    // for (let i = 0; i < 3; i++) alert(i)

    // begin을 실행함
    let i = 0
    // condition이 truthy이면 → body를 실행한 후, step을 실행함
    if (i < 3) { alert(i); i++ }
    // condition이 truthy이면 → body를 실행한 후, step을 실행함
    if (i < 3) { alert(i); i++ }
    // condition이 truthy이면 → body를 실행한 후, step을 실행함
    if (i < 3) { alert(i); i++ }
    // i == 3이므로 반복문 종료
    ```

#### 3.1. 구성 요소 생략하기
- `for`문의 구성 요소를 생략하는 것도 가능
- begin, step, 모든 구성 요소 생략 가능
    ``` javascript
    let i = 0;

    for (; i < 3; i++) {
        alert( i ); // 0, 1, 2
    }

    i = 0;
    for (; i < 3;) {
        alert( i++ );
    }

    for (;;) {
        // 끊임 없이 본문이 실행됨
    }
    ```
    - 생략 시, 두 개의 세미콜론을 꼭 넣어주기

### 4. 반복문 빠져나오기
- 대개는 반복문의 조건이 falsy가 되면 반복문이 종료됨
- 특별한 지시자인 `break`를 사용하면 언제든 원하는 때에 반복문을 빠져나올 수 있음
- 아래 예시의 반복문은 사용자에게 일련의 숫자를 입력하도록 안내하고, 사용자가 아무런 값도 입력하지 않으면 반복문을 '종료'함
    ``` javascript
    let sum = 0;

    while (true) {
        let value = +prompt("숫자를 입력하세요.", "")
        if (!value) break; // (*) : 사용자가 아무것도 입력하지 않거나, Cancel 버튼을 눌렀을 때 활성화됨

        sum += value;
    }
    alert( "합계: " + sum);
    ```

### 5. 다음 반복으로 넘어가기
- `continue` : 현재 시행중인 이터레이션을 멈추고, 반복문이 다음 이터레이션을 강제로 실행시키도록 함
- 아래 반복문은 `continue`를 사용해 홀수만 출력
    ``` javascript
    for (let i = 0; i < 10; i++) {
        if (i % 2 == 0) continue;

        alert(i); // 1, 3, 5, 7, 9
    }
    ```

- '?' 오른쪽엔 break나 continue가 올 수 없음

### 6. break/continue와 레이블
- 레이블(label) : 반복문 앞에 콜론과 함께 쓰이는 식별자
    ``` javascript
    labelName: for (...) {
        ...
    }
    ```
    - 여러 개의 중첩 반복문을 한 번에 빠져나와야 하는 경우 사용 가능

- 반복문 안에서 `break <labelName>` 문을 사용하면 레이블에 해당하는 반복문을 빠져나올 수 있음
    ``` javascript
    outer: for (let i = 0; i < 3; i++) {

        for (let j = 0; j < 3; j++) {
            let input = prompt(`(${i}, ${j})의 값`, "");

            if (!input) break outer;
        }
    }
    alert("완료!")
    ```
    -  continue 지시자와 함께 사용하는 것도 가능

- 레이블은 마음대로 '점프'할 수 있게 해주진 않음
    - break, continue는 반복문 안에서만 사용할 수 있고, 레이블은 반드시 break이나 continue 지시자 위에 있어야 함