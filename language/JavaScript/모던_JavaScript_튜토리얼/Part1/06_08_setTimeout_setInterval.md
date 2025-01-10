- 호출 스케줄링(scheduling a call) : 일정 시간이 지난 후에 원하는 함수를 예약 실행(호출)할 수 있게 하는 것

- 호출 스케줄링을 구현하는 방법

  - setTimeout -> 일정 시간이 지난 후에 함수를 실행하는 방법
  - setInterval -> 일정 시간 간격을 두고 함수를 실행하는 방법

- 자바스크립트 명세서엔 setTimeout, setInterval이 명시되어있지 않음
- 시중에 나와 있는 모든 브라우저, Node.js를 포함한 자바스크립트 호스트 환경 대부분이 이와 유사한 메서드와 내부 스케줄러를 지원함

### 1. setTimeout

- 문법

  ```javascript
  let timeId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
  ```

  - `func|code` : 실행하고자 하는 코드
    - 함수 또는 문자열 형태
    - 대개는 이 자리에 함수가 들어감
    - 하위 호환성을 위해 문자열도 받을 수 있게 해놓았지만, 추천하진 않음
  - `delay` : 실행 전 대기 시간
    - 단위 : 밀리초
    - 기본값 : 0
  - `arg1`, `arg2`, ...
    - 함수에 전달할 인수
    - IE9 이하에선 지원하지 않음

- 예시

  ```javascript
  function sayHi() {
    alert("안녕하세요.");
  }

  setTimeout(sayHi, 1000);
  ```

- 아래와 같이 함수에 인수를 넘겨줄 수도 있음

  ```javascript
  function sayHi(who, phrase) {
    alert(who + " 님, " + phrase);
  }

  setTimeout(sayHi, 1000, "홍길동", "안녕하세요.");
  ```

- setTimeout의 첫 번째 인수가 문자열이면 자바스크립트는 이 문자열을 이용해 함수를 만듦
- 따라서 아래 예시도 정상적으로 동작함

  ```javascript
  setTimeout("alert('안녕하세요. ')", 1000);
  ```

  - 하지만 이렇게 문자열을 사용하는 방식은 추천하지 않음

- 되도록 다음 예시와 같이 익명 화살표 사용하기

  ```javascript
  setTimeout(() => alert("안녕하세요. "), 1000);
  ```

- NOTE : 함수를 실행하지 말고 넘기기!
  ```javascript
  // 잘못된 코드
  setTimeout(sayHi(), 1000);
  ```
  - 이렇게 전달하면 함수 실행 결과가 전달되어버림

#### 1.1. clearTimeout으로 스케줄링 취소하기

- `setTimeout`을 호출하면 '타이머 식별자(timer identifier)'가 반환됨
- 스케줄링을 취소하고 싶을 땐 이 식별자(아래 예시에서 `timerId`)를 사용하면 됨

  ```javascript
  let timerId = setTimeout(...);
  clearTimeout(timerId);
  ```

- 아래 예시는 함수 실행을 계획해 놓았다가 중간에 마음이 바뀌어 계획해 놓았던 것을 취소한 상황을 코드로 표현하고 있음
- 예시를 실행해도 스케줄링이 취소되었기 때문에 아무런 변화가 없는 것을 확인할 수 있음

  ```javascript
  let timerId = setTimeout(() => alert("아무런 일도 일어나지 않습니다."), 1000);
  alert(timerId); // 타이머 식별자

  clearTimeout(timerId);
  alert(timerId); // 위 타이머 식별자와 동일함 (취소 후에도 식별자의 값은 null이 되지 않음)
  ```

- 예시를 실행하면 `alert` 창이 2개가 뜨는데, 이 얼럿 창을 통해 브라우저 환경에선 타이머 식별자가 숫자라는 걸 알 수 있음
- 다른 호스트 환경에선 타이머 식별자가 숫자형 이외의 자료형일 수 있음
- 참고로 Node.js에서 `setTimeout`을 실행하면 타이머 객체가 반환함

- 스케줄링에 관한 명세는 따로 없어서 호스트 환경마다 약간의 차이가 있을 수밖에 없음
- 브라우저는 HTML5의 timers section을 준수하고 있음
