### 1. 'if' 문
- `if(...)` 문은 괄호 안에 들어가는 조건을 평가함
- 그 결과가 `true`이면 코드 블록이 실행됨
    ``` javascript
    let year = prompt('ECMAScript-2015 명세는 몇 년도에 출판되었을까요?', '');

    if (year == 2015) alert("정답입니다!.");
    ```
- 조건이 true일 때 복수의 문을 실행하고 싶다면, 중괄호로 코드 블록 감싸기
    ``` javascript
    if (year == 2015) {
        alert( "정답입니다!" );
        alert( "아주 똑똑하시네요!" );
    }
    ```
- `if`문을 쓸 때는 조건이 참인 경우 실행되는 구문이 단 한 줄이더라도, 중괄호 `{}`를 사용해 코드를 블록으로 감싸는 것을 추천
- 이렇게 하면 코드 가독성이 증가함

### 2. 불린형으로의 변환
- `if (...)` 문은 괄호 안의 표현식을 평가하고 그 결과를 불린값으로 변환함
    ``` javascript
    if (0) { // 0은 falsy. 절대 실행되지 않음
        ...
    }

    if (1) { // 1은 truthy. 항상 실행됨
        ...
    }

    let cond = (year == 2015); // 동등 비교를 통해 true/false 여부를 결정함

    if (cond) {
        ...
    }
    ```

### 3. 'else' 절
- `if` 문엔 `else` 절을 붙일 수 있음
- `else` 뒤에 이어지는 코드 블록은 조건이 거짓일 때 실행됨
    ``` javascript
    let year = prompt('ECMAScript-2015 명세는 몇 년도에 출판되었을까요?', '');

    if (year == 2015) {
    alert( '정답입니다!' );
    } else {
    alert( '오답입니다!' ); // 2015 이외의 값을 입력한 경우
    }
    ```

### 4. 'else if'로 복수 조건 처리하기
``` javascript
let year = prompt('ECMAScript-2015 명세는 몇 년도에 출판되었을까요?', '');

if (year < 2015) {
  alert( '숫자를 좀 더 올려보세요.' );
} else if (year > 2015) {
  alert( '숫자를 좀 더 내려보세요.' );
} else {
  alert( '정답입니다!' );
}
```

### 5. 조건부 연산자 '?'
``` javascript
let accessAllowed;
let age = prompt('나이를 입력해 주세요.', '');

if (age > 18) {
  accessAllowed = true;
} else {
  accessAllowed = false;
}

alert(accessAllowed);
```
- `물음표(question mark) 연산자`라고도 불리는 `조건부 (conditional) 연산자`를 사용하면 위 예시를 더 짧고 간결하게 변형할 수 있음
- 조건부 연산자는 물음표`?`로 표시
- 피연산자가 세 개이기 때문에 조건부 연산자를 `삼항(ternary) 연산자`라고 부르는 사람도 있음
- 문법
    ``` javascript
    // condition이 truthy라면 : value1
    // condition이 falsy라면 : value2
    let result = condition ? value1 : value2;
    ```
- 예시
    ``` javascript
    let accessAllowed = (age > 18) ? true : false;
    ```
    - 괄호가 있으나 없으나 차이는 없지만, 코드의 가독성 향상을 위해 괄호를 사용할 것을 권장

### 6. 다중 '?'
- 물음표 연산자 `?`를 여러 개 연결하면 복수의 조건을 처리할 수 있음
- 예시
    ``` javascript
    let age = prompt("나이를 입력해주세요.", 18);

    let message = (age < 3) ? "아기야 안녕?" :
        (age < 18) ? "안녕!" :
        (age < 100) ? "환영합니다!" :
        "나이가 아주 많으시거나, 나이가 아닌 값을 입력 하셨군요!";
    
    alert( message );
    ```
- 위 예시는 아래와 의미가 같음
    ``` javascript
    if (age < 3) {
        message = "아기야 안녕?";
    } else if (age < 18) {
        message = "안녕!";
    } else if (age < 100) {
        message = "환영합니다!";
    } else {
        message = "나이가 아주 많으시거나, 나이가 아닌 값을 입력 하셨군요!";
    }
    ```

### 7. 부적절한 '?'
- 물음표 `?`를 `if` 대용으로 쓰는 경우가 종종 있음
    ``` javascript
    let company = prompt('자바스크립트는 어떤 회사가 만들었을까요?', '');

    (company == 'Netscape') ?
    alert('정답입니다!') : alert('오답입니다!');
    ```
    - 이렇게 코드를 작성하면 가독성이 떨어짐
- 아래는 if를 사용해 변형한 코드
    ``` javascript
    let company = prompt('자바스크립트는 어떤 회사가 만들었을까요?', '');

    if (company == 'Netscape') {
    alert('정답입니다!');
    } else {
    alert('오답입니다!');
    }
    ```