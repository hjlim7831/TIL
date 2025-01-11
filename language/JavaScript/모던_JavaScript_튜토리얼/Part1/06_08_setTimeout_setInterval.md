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

### 2. setInterval

- `setTimeout`과 동일한 문법을 사용
- 문법
  ```javascript
  let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...)
  ```
- 인수 역시 동일
- `setTimeout`과 달리, 함수를 주기적으로 실행함
- 함수 호출을 중단하려면 `clearInterval(timerId)`를 사용하면 됨
- 다음 예시를 실행하면 메시지가 2초 간격으로 보이다가 5초 이후에는 더 이상 메시지가 보이지 않음

  ```javascript
  // 2초 간격으로 메시지를 보여줌
  let timerId = setInterval(() => alert("째깍"), 2000);

  // 5초 후에 정지
  setTimeout(() => {
    clearInterval(timerId);
    alert("정지");
  }, 5000);
  ```

- NOTE : `alert` 창이 떠 있더라도 타이머는 멈추지 않음
  - 위 예시를 실행한 후 첫 번째 alert 창이 떴을 때 몇 초간 기다렸다가 창을 닫으면, 두 번째 alert 창이 바로 나타나는 것을 보고 이를 확인할 수 있음

### 3. 중첩 setTimeout

- 예시

  ```javascript
  /** setInterval을 이용하지 않고 아래와 같이 중첩 setTimeout을 사용함
  let timerId = setInterval(() => alert('째깍'), 2000);
  */

  let timerId = setTimeout(function tick() {
    alert("째깍");
    timerId = setTimeout(tick, 2000); // (*)
  }, 2000);
  ```

  - (\*) 줄의 실행이 종료되면, 다음 호출을 스케줄링함

- 중첩 setTimeout을 이용한 방법은 setInterval을 사용하는 방법보다 유연함
- 호출 결과에 따라 다음 호출을 원하는 방식으로 조정해 스케줄링 할 수 있기 때문
- 5초 간격으로 서버에 요청을 보내 데이터를 얻는다고 가정해보자
- 서버가 과부하 상태라면 요청 간격을 10초, 20초, 40초 등으로 증가시켜주는 게 좋을 것
- 예시 의사코드

  ```javascript
  let delay = 5000;

  let timerId = setTimeout(function request() {
    ...요청 보내기...

    if (서버 과부하로 인한 요청 실패) {
      // 요청 간격을 늘립니다.
      delay *= 2;
    }

    timerId = setTimeout(request, delay);

  }, delay);
  ```

- CPU 소모가 많은 작업을 주기적으로 실행하는 경우에도 `setTimeout`을 재귀 실행하는 방법이 유용함
- 작업에 걸리는 시간에 따라 다음 작업을 유동적으로 계획할 수 있기 때문
- 중첩 `setTimeout`을 이용하는 방법은 지연 간격을 보장하지만, `setInterval`은 이를 보장하지 않음

  - setInterval : func를 실행하는 데 '소모되는'시간도 지연 간격에 포함시킴
  - func를 실행하는 데 걸리는 시간이 명시한 지연 간격보다 길 경우라면?  
    -> 엔진이 func의 실행이 종료될 때까지 기다려줌
  - func의 실행이 종료되면 엔진은 스케줄러를 확인하고, 지연 시간이 지났으면 다음 호출을 바로 시작함
  - 따라서 함수 호출에 걸리는 시간이 매번 delay 밀리초보다 길면, 모든 함수가 쉼 없이 계속 연속 호출됨

- NOTE : 가비지 컬렉션과 setInterval/setTimeout
  - setInterval이나 setTimeout에 함수를 넘기면, 함수에 대한 내부 참조가 새롭게 만들어지고 이 참조 정보는 스케줄러에 저장됨
  - 해당 함수를 참조하는 것이 없어도 setInterval과 setTimeout에 넘긴 함수는 가비지 컬렉션의 대상이 되지 않음
  ```javascript
  // 스케줄러가 함수를 호출할 때까지 함수는 메모리에 유지됨
  setTimeout(function() {...}, 100);
  ```
  - `setInterval`의 경우, `clearInterval`이 호출되기 전까지 함수에 대한 참조가 메모리에 유지됨
  - 그런데 이런 동작 방식에는 부작용이 하나 있음
  - 외부 렉시컬 환경을 참조하는 함수가 있다고 가정해 보자
  - 이 함수가 메모리에 남아있는 동안엔 외부 변수 역시 메모리에 남아있기 마련임
  - 그런데 이렇게 되면 실제 함수가 차지했어야 하는 공간보다 더 많은 메모리 공간이 사용됨
  - 이런 부작용을 방지하고 싶다면, 스케줄링할 필요가 없어진 함수는 아무리 작더라도 취소하자
