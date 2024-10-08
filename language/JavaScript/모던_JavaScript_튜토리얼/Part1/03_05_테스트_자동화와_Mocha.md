### 1. 테스트는 왜 해야 하는가?
- 개발 중엔 콘솔 창 등을 이용해 실제 실행 결과가 기대했던 결과와 같은지 계속 비교하면서, 원하는 기능이 잘 구현되고 있는지 확인할 것
- 실제 실행 결과가 기대했던 결과와 다를 땐, 코드를 수정하고 다시 실행해 그 결과를 기대했던 결과와 다시 비교해 볼 것
- 원하는 기능을 완성할 때까지 이 과정을 계속 반복
- 이렇게 수동으로 코드를 '재실행'하는 건 상당히 불완전함

- **코드를 수동으로 '재실행'하면서 테스트를 하면 무언가를 놓치기 쉬움**
- 개발자는 무언가를 만들 때 머릿속에 수많은 유스 케이스를 생각하며 코드를 작성하는데, 코드를 변경해야 할 때마다 모든 유스케이스를 상기하면서 코드를 수정하는 것은 거의 불가능
- 하나를 고치면 또 다른 문제가 튀어나오는 이유가 바로 이 때문
- **테스팅 자동화는 테스트 코드가 실제 동작에 관여하는 코드와 별개로 작성되었을 때 가능**
- 테스트 코드를 이용하면 함수를 다양한 조건에서 실행해 볼 수 있는데, 이때 실행 결과와 기대 결과를 비교해볼 수 있음

### 2. Behavior Driven Development (BDD)
- 테스트(test), 문서(documentation), 예시(example)을 한데 모아놓은 개념

### 3. 거듭제곱 함수와 명세서
- x를 n번 곱해주는 함수, `pow(x, n)`를 구현하고 있다고 가정해보자 (단, `n`은 자연수이고, 조건 `n >= 0`을 만족해야 함)
- 코드를 작성하기 전, 코드가 무슨 일을 하는지 상상한 후 이를 자연어로 표현해야 함
- 이때 만들어진 산출물을 BDD에선 명세서(specification) 또는 짧게 줄여 스펙(spec)이라 부름
- 명세서엔 아래와 같이 유스 케이스에 대한 자세한 설명과 테스트가 담겨있음
``` javascript
describe("pow", function() {

    it("주어진 숫자의 n 제곱", function() {
        assert.equal(pow(2, 3), 8);
    });
});
```
- 스펙은 세 가지 주요 구성 요소로 이루어짐
1. `describe ("title", function() { ... })`
- 구현하고자 하는 기능에 대한 설명이 들어감
- 우리 예시에선 함수 pow가 어떤 동작을 하는지에 대한 설명이 들어갈 것
- it 블록을 한데 모아주는 역할도 함

2. `it("유스케이스 설명", function() { ... })`
- `it`의 첫 번째 인수엔 특정 유스 케이스에 대한 설명이 들어감
- 이 설명은 누구나 읽을 수 있고 이해할 수 있는 자연어로 적어줌
- 두 번째 인수엔 유스케이스 테스트 함수가 들어감

3. `assert.equal(value1, value2)`
- 기능을 제대로 구현했다면, it 블록 내의 코드 `assert.equal(value1, value2)`이 에러 없이 실행됨

- 함수 `assert.*`는 pow가 예상대로 동작하는지 확인해줌

### 4. 개발 순서
1. 명세서 초안 작성. 초안엔 기본적인 테스트도 들어감
2. 명세서 초안을 보고 코드 작성
3. 코드가 작동하는지 확인하기 위해, Mocha라 불리는 테스트 프레임워크를 사용해 명세서를 실행
- 이 때, 코드가 잘못 작성되었다면 에러가 출력됨
- 개발자는 테스트를 모두 통과해 에러가 더는 출력되지 않을 때까지 코드를 수정
4. 모든 테스트를 통과하는 코드 초안이 완성됨
5. 명세서에 지금까진 고려하지 않았던 유스케이스 몇 가지를 추가
-테스트가 실패하기 시작할 것
6. 3번으로 돌아가 테스트를 모두 통과할 때까지 코드를 수정
7. 기능이 완성될 때까지 3 ~ 6단계를 모두 반복

- 위와 같은 방법은 반복적인(iterative) 성격을 지님
- 명세서를 작성하고 실행한 후 테스트를 모두 통과할 때까지 코드를 작성하고, 또 다른 테스트를 추가해 앞의 과정을 반복함
- 이렇게 하다 보면 종래에는 완전히 동작하는 코드와 테스트 둘 다를 확보하게 됨
- 실제 사례에 위 개발 프로세스를 적용해보자
- 함수 pow의 스펙 초안은 이미 위에서 작성했으므로, 첫 번째 단계는 이미 끝난 상황
- 코드를 본격적으로 작성하기 전에, 잠시 자바스크립트 라이브러리 몇 가지를 사용해 테스트를 실행해 볼 것
- 지금 상태에선 테스트 모두가 실패할 텐데, 그런데도 실행해 보는 이유는 테스트가 실제로 돌아가는지 확인하기 위해서임

### 5. 스펙 실행하기
- 본 튜토리얼에선 총 3개의 라이브러리를 사용해 테스트를 진행해볼 것
- 각 라이브러리에 대한 설명은 아래와 같음
    1. Mocha : 핵심 테스트 프레임워크. describe, it과 같은 테스트 함수와 테스트 실행 관련 주요 함수를 제공
    2. Chai : 다양한 assertion을 제공해 주는 라이브러리. 우리 예시에선 assert.equal 정도만 사용해 볼 예정
    3. Sinon : 함수의 정보를 캐내는 데 사용되는 라이브러리. 내장 함수 등을 모방함. 본 챕터에선 사용하지 않고, 다른 챕터에서 실제로 사용해 볼 예정

- 세 라이브러리 모두 브라우저, 서버 사이드 환경을 가리지 않고 사용 가능
- 아래 HTML 페이지엔 pow의 스펙, 라이브러리 모두가 들어있음
    ``` html
    <!DOCTYPE html>
    <html>
    <head>
    <!-- 결과 출력에 사용되는 mocha css를 불러옵니다. -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.2.0/mocha.css">
    <!-- Mocha 프레임워크 코드를 불러옵니다. -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.2.0/mocha.js"></script>
    <script>
        mocha.setup('bdd'); // 기본 셋업
    </script>
    <!-- chai를 불러옵니다 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/chai/3.5.0/chai.js"></script>
    <script>
        // chai의 다양한 기능 중, assert를 전역에 선언합니다.
        let assert = chai.assert;
    </script>
    </head>

    <body>
    <script>
        function pow(x, n) {
        /* 코드를 여기에 작성합니다. 지금은 빈칸으로 남겨두었습니다. */
        }
    </script>

    <!-- 테스트(describe, it...)가 있는 스크립트를 불러옵니다. -->
    <script src="test.js"></script>

    <!-- 테스트 결과를 id가 "mocha"인 요소에 출력하도록 합니다.-->
    <div id="mocha"></div>

    <!-- 테스트를 실행합니다! -->
    <script>
        mocha.run();
    </script>
    </body>

    </html>
    ```
- 위 페이지는 다섯 부분으로 나눌 수 있음
1. `<head>` : 테스트에 필요한 서드파티 라이브러리와 스타일을 불러옴
2. `<script>` : 테스트할 함수(`pow`)의 코드가 들어감
3. 테스트 : `describe("pow", ... )` 를 외부 스크립트 (`test.js`)에서 불러옴
4. HTML 요소 `<div id="mocha">` : Mocha 실행 결과가 출력됨
5. `mocha.run()` : 테스트를 실행시켜주는 명렁어

- 지금은 함수 pow 본문에 아무런 코드도 없으므로, 테스트가 실패할 수밖에 없음
- karma 같은 고수준의 테스트 러너(test-runner)를 사용하면 다양한 종류의 테스트를 자동으로 실행할 수 있음

### 6. 코드 초안
- 오로지 테스트 통과만을 목적으로 코드를 간단하게 작성해보자
    ``` javascript
    function pow(x, n) {
        return 8;
    }
    ```
- 이젠 스펙을 실행해도 에러 발생 X

### 7. 스펙 개선하기
- 더 많은 유스 케이스 추가하기
- 스펙에 테스트를 추가하는 방법
    1. 기존 it 블록에 assert 하나 더 추가하기
    ``` javascript
    describe("pow", function() {

        it("주어진 숫자의 n 제곱", function() {
            assert.equal(pow(2, 3), 8);
            assert.equal(pow(3, 4), 81);
        });

    });
    ```
    - 이렇게 추가하면 첫 번째 assert에서 에러 발생 시 두 번째 assert의 결과를 알 수 없음
    2. 테스트를 하나 더 추가하기 (it 블록 하나 더 추가하기)
    ``` javascript
    describe("pow", function() {

        it("2를 세 번 곱하면 8입니다.", function() {
            assert.equal(pow(2, 3), 8);
        });

        it("3을 네 번 곱하면 81입니다.", function() {
            assert.equal(pow(3, 4), 81);
        });

    });
    ```
    - 이렇게 짜면 더 많은 정보를 얻을 수 있으므로, 이쪽을 추천

- 추천 규칙 : **테스트 하나에선 한 가지만 확인하기**
    - 테스트 하나에서 연관이 없는 사항 두 개를 점검하고 있다면, 이 둘을 분리하는 게 좋음


### 8. 코드 개선하기
- 함수 개선
    ``` javascript
    function pow(x, n) {
        let result = 1;

        for (let i = 0; i < n; i++) {
            result *= x;
        }

        return result;
    }
    ```
- 함수가 제대로 작동하는지 확인하기 위해, 더 많은 값들을 테스트해보자.
- 수동으로 여러 개의 it 블록을 만드는 대신, for문을 사용해 자동으로 it 블록 만들기
    ``` javascript
    describe("pow", function() {

        function makeTest(x) {
            let expected = x * x * x;
            it(`${x}을/를 세 번 곱하면 ${expected}입니다.`, function() {
                assert.equal(pow(x, 3), expected);
            })
        }

        for (let x = 1; x <= 5; x++) {
            makeTest(x);
        }
    })
    ```

### 9. 중첩 describe
``` javascript
describe("pow", function() {

    // 새로운 테스트 '하위 그룹(subgroup)'을 정의할 때 사용
    describe("x를 세 번 곱합니다.", function() {
        function makeTest(x) {
            let expected = x * x * x;
            it(`${x}을/를 세 번 곱하면 ${expected}입니다.`, function() {
                assert.equal(pow(x, 3), expected);
            })
        }

        for (let x = 1; x <= 5; x++) {
            makeTest(x);
        }
    })
})
```
- before : (전체) 테스트가 실행되기 전에 실행됨
- after : (전체) 테스트가 실행된 후에 실행됨
- beforeEach : 매 it이 실행되기 전에 실행됨
- afterEach : 매 it이 실행된 후에 실행됨

### 10. 스펙 확장하기
- n이 조건에 맞지 않을 때 함수가 NaN을 반환하는지에 대한 테스트 추가
    ``` javascript
    describe("pow", function() {

        // ...
    })

    it("n이 음수일 때 결과는 NaN입니다.", function() {
        assert.isNaN(pow(2, -1));
    });

    it("n이 정수가 아닐 때 결과는 NaN입니다.", function() {
        assert.isNaN(pow(2, 1.5));
    })

    ```
- pow 코드 개선하기
    ``` javascript
    function pow(x, n) {
        if (n < 0) return NaN;
        if (Math.round(n) != n) return NaN;
        let result = 1;

        for (let i = 0; i < n; i++) {
            result *= x;
        }

        return result;
    }
    ```
