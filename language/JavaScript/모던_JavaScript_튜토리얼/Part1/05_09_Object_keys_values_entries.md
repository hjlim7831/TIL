- `keys()`, `values()`, `entries()`를 사용할 수 있는 자료구조

  - Map
  - Set
  - Array

- 일반 객체에도 순회 관련 메서드가 있긴 하지만, 위 메서드들과는 문법에 차이가 있음

### 1. Object.keys, values, entries

- 일반 객체엔 다음과 같은 메서드를 사용할 수 있음

1. `Object.keys(obj)` : 객체의 키만 담은 배열을 반환
2. `Object.values(obj)` : 객체의 값만 담은 배열을 반환
3. `Object.entries(obj)` : `[키, 값]` 쌍을 담은 배열을 반환

#### 1.1. Map, Set, Array 전용 메서드와 일반 객체용 메서드의 차이 (맵 기준)

1. `obj.keys()`가 아닌 `Object.keys(obj)`를 호출한다는 점

- 문법이 다른 이유 : 유연성 때문
- 객체 자체에 keys, values, entries라는 메서드를 구현해 사용하는 경우가 있을 수 있음
- 하지만 `Object.values(data)`와 같은 형태로 메서드를 호출할 수 있으면, 커스텀 메서드와 내장 메서드를 둘 다 사용할 수 있음

2. 메서드 `Object.*`를 호출하면 iterable 객체가 아닌 객체의 한 종류인 배열을 반환함

- '진짜' 배열을 반환하는 이유 : 하위 호환성 때문
- 예시

  ```javascript
  let user = {
    name: "John",
    age: 30,
  };
  ```

  - `Object.keys(user) = ["name", "age"]`
  - `Object.values(user) = ["John", 30]`
  - `Object.entries(user) = [["name", "john"], ["age", 30]]`

- 아래 예시처럼 `Object.values`를 사용하면 프로퍼티 값을 대상으로 원하는 작업을 할 수 있음

  ```javascript
  let user = {
    name: "Violet",
    age: 30,
  };

  // 값을 순회함
  for (let value of Object.values(user)) {
    alert(value);
  }
  ```

- Object.keys, values, entries는 심볼형 프로퍼티를 무시함
  - 심볼형 키가 필요한 경우엔
  1. 심볼형 키만 배열 형태로 반환해주는 메서드인 `Object.getOwnPropertySymbols`를 사용하기
  2. 키 전체를 배열 형태로 반환하는 메서드인 `Reflect.ownKeys(obj)` 사용하기
