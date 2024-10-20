- 반복 가능한(iterable, 이터러블) 객체 : 배열을 일반화한 객체
- 이터러블이라는 개념을 사용하면 어떤 객체에든 `for..of` 반복문을 적용할 수 있음
- 예시 : 배열, 문자열
- 배열이 아닌 객체 중 어떤 것들의 컬렉션(목록, 집합 등)을 나타내고 있는 경우에도 `for..of` 문법을 사용할 수 있다면 순회하는 데 유용할 것 -> 가능하도록 해보자

### 1. Symbol.iterator

- `for..of`를 적용하기에 적합해 보이는 배열이 아닌 객체 예시

  ```javascript
  let range = {
    from: 1,
    to: 5,
  };

  // for (let num of range) ... num=1,2,3,4,5
  ```

- `range`를 iterable로 만들려면 (`for..of`가 동작하도록 하려면) 객체에 `Symbol.iterator` (특수 내장 심볼)라는 메서드를 추가해 아래와 같은 일이 벌어지도록 해야 함

  1. `for..of`가 시작되자 마자 `for..of`는 `Symbol.iterator`를 호출 (없으면 에러 발생)

  - `Symbol.iterator`는 반드시 이터레이터(iterator, 메서드 next가 있는 객체)를 반환해야 함

  2. 이후 `for..of`는 반환된 객체(이터레이터)만을 대상으로 동작
  3. `for..of`에 다음 값이 필요하면, `for..of`는 이터레이터의 `next()` 메서드를 호출함
  4. `next()`의 반환 값은 `{done: Boolean, value: any}`와 같은 형태여야 함

  - `done=true` : 반복이 종료되었음을 의미
  - `done=false` : value에 다음 값이 저장됨

- 위의 range를 반복 가능한 객체로 만들어주는 코드

  ```javascript
  let range = {
    from: 1,
    to: 5,
  };

  // 1. for..of 최초 호출 시, Symbol.iterator가 호출됩니다.
  range[Symbol.iterator] = function () {
    // Symbol.iterator는 이터레이터 객체를 반환합니다.
    // 2. 이후 for..of는 반환된 이터레이터 객체만을 대상으로 동작하는데, 이때 다음 값도 정해집니다.
    return {
      current: this.from,
      last: this.to,

      // 3. for..of 반복문에 의해 반복마다 next()가 호출됩니다.
      next() {
        // 4. next()는 값을 객체 {done:.., value :...}형태로 반환해야 합니다.
        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      },
    };
  };

  // 이제 의도한 대로 동작합니다!
  for (let num of range) {
    alert(num); // 1, then 2, 3, 4, 5
  }
  ```

- 이터러블 객체의 핵심 : 관심사의 분리(Separation of concern, SoC)

  - `range`엔 메서드 `next()`가 없음
  - 대신 `range[Symbol.iterator]()`를 호출해서 만든 '이터레이터' 객체와 이 객체의 메서드 `next()`에서 반복에 사용될 값을 만들어냄

- 이렇게 하면 이터레이터 객체와 반복 대상인 객체를 분리할 수 있음
- range 자체를 이터레이터로 만들면, 아래처럼 간단해짐

  ```javascript
  let range = {
    from: 1,
    to: 5,

    [Symbol.iterator]() {
      this.current = this.from;
      return this;
    },

    next() {
      if (this.current <= this.to) {
        return { done: false, value: this.current++ };
      } else {
        return { done: true };
      }
    },
  };

  for (let num of range) {
    alert(num); // 1, then 2, 3, 4, 5
  }
  ```

  - 단점 : 두 개의 `for..of` 반복문을 하나의 객체에 동시에 사용할 수 없다는 점
  - 이터레이터(객체 자신)가 하나뿐이어서 두 반복문이 반복 상태를 공유하기 때문
  - 하지만 동시에 두 개의 `for..of`를 사용하는 것은 비동기 처리에서도 흔한 케이스는 아님
