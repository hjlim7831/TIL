- 자바스크립트는 함수를 다룰 때 탁월한 유연성을 제공함
- 함수는 이곳저곳에 전달될 수 있고, 객체로도 사용될 수 있음
- 이번 챕터에서 함수 간에 호출을 어떻게 포워딩하는지, 함수를 어떻게 데코레이팅하는지에 대해 알아볼 것

### 1. 코드 변경 없이 캐싱 기능 추가하기

- CPU를 많이 잡아먹지만 결과는 안정적인 함수 `slow(x)`가 있다고 가정해보자
- 결과가 안정적이라는 말은 `x`가 같으면 호출 결과도 같다는 것을 의미함
- `slow(x)`가 자주 출몰된다면, 결과를 어딘가에 저장(캐싱)해 재연산에 걸리는 시간을 줄이고 싶을 것
- 아래 예시에선 slow()안에 캐싱 관련 코드를 추가하는 대신, 래퍼 함수를 만들어 캐싱 기능을 추가할 예정
- 이렇게 래퍼 함수를 만들면 여러 가지 이점이 있음
- 예시

  ```javascript
  function slow(x) {
    // CPU 집약적인 작업이 여기에 올 수 있습니다.
    alert(`slow(${x})을/를 호출함`);
    return x;
  }

  function cachingDecorator(func) {
    let cache = new Map();

    return function (x) {
      if (cache.has(x)) {
        // cache에 해당 키가 있으면
        return cache.get(x); // 대응하는 값을 cache에서 읽어옵니다.
      }

      let result = func(x); // 그렇지 않은 경우엔 func를 호출하고,

      cache.set(x, result); // 그 결과를 캐싱(저장)합니다.
      return result;
    };
  }

  slow = cachingDecorator(slow);

  alert(slow(1)); // slow(1)이 저장되었습니다.
  alert("다시 호출: " + slow(1)); // 동일한 결과

  alert(slow(2)); // slow(2)가 저장되었습니다.
  alert("다시 호출: " + slow(2)); // 윗줄과 동일한 결과
  ```

- `cachingDecorator`같이 인수로 받은 함수의 행동을 변경시켜주는 함수를 데코레이터(decorator)라 부름
- 모든 함수를 대상으로 `cachingDecorator`를 호출 할 수 있는데, 이때 반환되는 것이 캐싱 래퍼
- 함수에 `cachingDecorator`를 적용하기만 하면 캐싱이 가능한 함수를 원하는 만큼 구현할 수 있기 때문에 데코레이터 함수는 아주 유용하게 사용됨
- 캐싱 관련된 코드를 함수 코드와 분리할 수 있기 때문에 함수의 코드가 간결해진다는 장점도 있음
- 아래 그림에서 볼 수 있듯이 `cachingDecorator(func)`를 호출하면 '래퍼(wrapper)', `function(x)`이 반환됨
- 래퍼 `function(x)`는 `func(x)`의 호출 결과를 캐싱 로직으로 감쌈(wrapping)

- 바깥 코드에서 봤을 때, 함수 `slow`는 래퍼로 감싼 이전이나 이후나 동일한 일을 수행함
- 행동 양식에 캐싱 기능이 추가된 것 뿐
- `slow` 본문을 수정하는 것보다 독립된 래퍼 함수 `cachingDecorator`를 사용할 때 생기는 이점을 정리하면 다음과 같음
  - `cachingDecorator`를 재사용할 수 있음
    - 원하는 함수 어디에든 `cachingDecorator`를 적용할 수 있음
  - 캐싱 로직이 분리되어 `slow` 자체의 복잡성이 증가하지 않음
  - 필요하다면 여러 개의 데코레이터를 조합해서 사용할 수도 있음 (추가 데코레이터는 `cachingDecorator` 뒤를 따름)

### 2. 'func.call'를 사용해 컨텍스트 지정하기

- 위에서 구현한 캐싱 데코레이터는 객체 메서드에 사용하기엔 적합하지 않음
- 객체 메서드 `worker.slow()`는 데코레이터 적용 후 제대로 동작하지 않음

  ```javascript
  // worker.slow에 캐싱 기능을 추가해봅시다.
  let worker = {
    someMethod() {
      return 1;
    },

    slow(x) {
      // CPU 집약적인 작업이라 가정
      alert(`slow(${x})을/를 호출함`);
      return x * this.someMethod(); // (*)
    },
  };

  // 이전과 동일한 코드
  function cachingDecorator(func) {
    let cache = new Map();
    return function (x) {
      if (cache.has(x)) {
        return cache.get(x);
      }
      let result = func(x); // (**)
      cache.set(x, result);
      return result;
    };
  }

  alert(worker.slow(1)); // 기존 메서드는 잘 동작합니다.

  worker.slow = cachingDecorator(worker.slow); // 캐싱 데코레이터 적용

  alert(worker.slow(2)); // 에러 발생!, Error: Cannot read property 'someMethod' of undefined
  ```

  - 원인 : `(**)`로 표시한 줄에서 래퍼가 기존 함수 `func(x)`를 호출하면 `this`가 `undefined`가 되기 때문
  - 아래 코드를 실행해도 비슷한 증상이 나타남
    ```javascript
    let func = worker.slow;
    func(2);
    ```

- 래퍼가 기존 메서드 호출 결과를 전달하려 했지만 `this`의 컨텍스트가 사라졌기 때문에 에러가 발생하는 것
- 에러가 발생하지 않게 코드를 수정해보자
- 먼저, `this`를 명시적으로 고정해 함수를 호출할 수 있게 해주는 특별한 내장 함수 메서드 `func.call(context, ...args)`에 대해 알아보자
- 문법

  ```javascript
  func.call(context, arg1, arg2, ...)
  ```

- 메서드를 호출하면 메서드의 첫 번째 인수가 `this`, 이어지는 인수가 `func`의 인수가 된 후, `func`가 호출됨
- 아래 함수와 메서드를 호출하면 거의 동일한 일이 발생함

  ```javascript
  func(1, 2, 3);
  func.call(obj, 1, 2, 3);
  ```

- 둘 다 인수로 `1`, `2`, `3`을 받음

  - 유일한 차이점 : `func.call`에선 `this`가 `obj`로 고정된다는 점
  - 다른 컨텍스트(다른 객체) 하에 `sayHi`를 호출하는 예시를 살펴보자
  - `sayHi.call(user)`를 호출하면 `sayHi`의 컨텍스트가 `this=user`로, `sayHi.call(admin)`을 호출하면 `sayHi`의 컨텍스트가 `this=admin`로 설정됨

    ```javascript
    function sayHi() {
      alert(this.name);
    }

    let user = { name: "John" };
    let admin = { name: "Admin" };

    sayHi.call(user); // this = John
    sayHi.call(admin); // this = Admin
    ```

- 래퍼 안에서 `call`을 사용해 컨텍스트를 원본 함수로 전달하면 에러가 발생하지 않음

  ```javascript
  let worker = {
    someMethod() {
      return 1;
    },

    slow(x) {
      alert(`slow(${x}을/를 호출함)`);
      return x * this.someMethod(); // (*)
    },
  };

  function cachingDecorator(func) {
    let cache = new Map();
    return function (x) {
      if (cache.has(x)) {
        return cache.get(x);
      }
      let result = func.call(this, x);
      cache.set(x, result);
      return result;
    };
  }

  worker.slow = cachingDecorator(worker.slow); // 캐싱 데코레이터 적용

  alert(worker.slow(2));
  alert(worker.slow(2));
  ```

- 이제 에러 없이 모든 게 정상적으로 동작함
- 명확한 이해를 위해 this가 어떤 과정을 거쳐 전달되는지 자세히 살펴볼 것
  1. 데코레이터를 적용한 후에 `worker.slow`는 래퍼 `function(x) {...}`가 됨
  2. `worker.slow(2)`를 실행하면 래퍼는 2를 인수로 받고, `this=worker`가 됨 (점 앞의 객체)
  3. 결과가 캐시되지 않은 상황이라면 `func.call(this, x)`에서 현재 `this(=worker)`와 인수(`=2`)를 원본 메서드에 전달함

### 3. 여러 인수 전달하기

- `cachingDecorator`를 좀 더 다채롭게 해보자
- 지금 상태론 인수가 하나뿐인 함수에만 `cachingDecorator`를 적용할 수 있음
