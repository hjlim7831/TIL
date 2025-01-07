- 함수를 만들 수 있는 방법
- 잘 사용하는 방법은 아니지만, 이 방법 외에는 대안이 없을 경우 사용

### 1. 문법

- 문법
  ```javascript
  let func = new Function([arg1, arg2, ...argN], functionBody);
  ```
- 새로 만들어지는 함수는 인수 `arg1...argN`과 함수 본문 `functionBody`로 구성됨
- 인수 두 개가 있는 함수를 직접 만들어보면서 `new Function` 문법에 대해 이해해 보자

  ```javascript
  let sum = new Function("a", "b", "return a + b");

  alert(sum(1, 2)); // 3
  ```

- 인수가 없고 함수 본문만 있는 함수를 만들어보자

  ```javascript
  let sayHi = new Function("alert('Hello')");

  sayHi(); // Hello
  ```

- 기존 사용 방법과 가장 큰 차이점 : 런타임에 받은 문자열을 사용해 함수를 만들 수 있음
- 어떤 문자열도 함수로 바꿀 수 있음
- 서버에서 전달받은 문자열을 이용해 새로운 함수를 만들고, 이를 실행하는 것도 가능
- 서버에서 코드를 받거나 템플릿을 사용해 함수를 동적으로 컴파일해야 하는 경우, 복잡한 웹 애플리케이션을 구현할 때와 같이 아주 특별한 경우에 사용 가능
