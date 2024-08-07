- 복수의 `if` 조건문은 `switch`문으로 바꿀 수 있음
- `switch`문을 사용한 비교법은 특정 변수를 다양한 상황에서 비교할 수 있게 해줌
- 코드 자체가 비교 상황을 잘 설명함

### 1. 문법
- switch문은 하나 이상의 case문으로 구성됨
- 대개 default문도 있지만, 필수는 아님
``` javascript
switch(x) {
    case "value1": // if (x === "value1")
        ...
        [break]
    case "value2": // if (x === "value2")
        ...
        [break]
    default:
        ...
        [break]
}
```
- `case` 문에서 변수 `x`의 값과 일치하는 값을 찾으면, 해당 `case`문의 아래의 코드가 실행됨
- 이때, `break`문을 만나거나 `switch`문이 끝나면 코드의 실행은 멈춤
- 값과 일치하는 `case`문이 없다면, `default`문 아래의 코드가 실행됨

### 2. 예시
- 실제 실행 가능한 `switch`문 예시를 살펴보자
- 아래 예시에선 강조된 코드가 실행됨
    ``` javascript
    let a = 2 + 2;

    switch (a) {
        case 3:
            alert("비교하려는 값보다 작음.")
            break;
        case 4:
            alert("비교하려는 값과 일치함.")
            break;
        case 5:
            alert("비교하려는 값보다 큼.")
            break;
        default:
            alert("어떤 값인지 파악이 되지 않음")
    }
    ```
- case문 안에 break문이 없으면 조건에 부합하는지 여부를 따지지 않고 이어지는 case문을 실행함
- break문이 없는 경우, 어떤 일이 일어나는지 예시를 통해 살펴봄
    ``` javascript
    let a = 2 + 2;

    switch(a) {
        case 3:
            alert("비교하려는 값보다 작음")
        case 4:
            alert("비교하려는 값과 일치함")
        case 5:
            alert("비교하려는 값보다 큼")
        default:
            alert("어떤 값인지 파악이 되지 않음")
    }
    ```
    - 위 예시를 실행하면 case 4, 5, default의 alert문이 실행됨

- switch/case 문의 인수엔 어떤 표현식이든 올 수 있음
    ``` javascript
    let a = "1";
    let b = 0;

    switch (+a) {
        case b + 1;
            alert("표현식 +a는 1,  표현식 b + 1은 1이므로 이 코드가 실행됨")
            break;
        default:
            alert("이 코드는 실행되지 않음")
    }
    ```

### 3. 여러 개의 "case"문 묶기
- 코드가 같은 case문은 한데 묶을 수 있음
- case 3과 case 5에서 실행하려는 코드가 같은 경우에 대한 예시를 살펴보자
    ``` javascript
    let a = 3;

    switch (a) {
        case 4:
            alert("계산이 맞습니다!")
            break;
        case 3: // 두 case문을 묶음
        case 5:
            alert("계산이 틀립니다!")
            alert("수학 수업을 다시 들어보는 걸 추천해요")
            break;
        default:
            alert("계산 결과가 이상하네요")
    }
    ```

### 4. 자료형의 중요성
- switch문은 일치 비교로 조건을 확인함
- 비교하려는 값과 case문의 값의 **형과 값**이 같아야 해당 case문이 실행됨
    ``` javascript
    let arg = prompt("값을 입력해주세요")
    switch (arg) {
        case "0":
        case "1":
            alert("0이나 1을 입력하셨습니다.")
            break;
        case "2":
            alert("2를 입력하셨습니다.")
            break;
        case 3:
            alert("이 코드는 절대 실행되지 않습니다.")
            break;
        default:
            alert("알 수 없는 값을 입력하셨습니다.")
    }
    ```