### MobX란?
- Redux와 또 다른 상태 관리 라이브러리
- Component와 State를 연결하는 보일러플레이트 코드들을 데코레이터 제공으로 깔끔히 해결

<br>

### Observable
- '추적한다'는 뜻을 가짐
- 속성 (property), 모든 객체, 배열, map과 set은 observable로 설정될 수 있음
- 관찰 대상 (observable value) : Mobx에서 Rendering 대상이 되는 state(상태, 값)
- @observable 데코레이터로 지정한 State : 관찰 대상으로 지정되고 그 State는 변경될 때마다 렌더링됨
- 객체를 observable로 만드는 가장 기본적인 방법 : makeObservable을 사용해, 속성마다 주석 지정하기
    - observable : state를 저장하는 추적 가능한 필드를 정의
    - action : state를 수정하는 메서드를 표시
    - computed : state로부터 새로운 사실을 도출하고, 그 결과값을 캐시하는 getter

<br>

### MobX를 사용하는 이유

1) 왜 상태관리 라이브러리를 사용하는가?
- SPA 개발 시, 화면에 보이는 상태들이나 보이지 않는 상태들도 실시간이나 비동기로 변화함 <br>
-> 상태가 언제 어떻게 변화하였는지 알 수 없어져 컨트롤하기 힘든 상황에 놓임
- 프로젝트의 규모가 커지고 복잡해지면서, 방대한 데이터들을 관리하기 어려워짐
- Component 간의 정보 공유의 어려움

Why?
- 자식 컴포넌트 간의 다이렉트 데이터 전달이 불가능
- 자식 컴포넌트들 간의 데이터를 주고 받을 때, 상태를 관리하는 부모 컴포넌트를 통해 데이터 전달
- but 자식 컴포넌트가 많아지면, 상태 관리가 매우 복잡해짐<br>
-> 상태를 관리하는 상위 컴포넌트에서 계속 내려 받아야 함 (Props drilling)

<br>
2) 해결방안 : 전역 상태 관리 스토어 제공
- 전역 상태 관리 스토어를 제공해, 어디에서나 상태들을 관리할 수 있게 함<br>
-> 다중 계층 컴포넌트에서 데이터와 메소드 접근의 복잡성을 해결
컴포넌트에 집중된 비즈니스 로직을 분리 가능

<br>

### MobX의 장단점
1) 간단함
- Reactive Programming으로부터 영감을 받음
    - Reactive Programming : 데이터의 흐름과 전파에 초점을 맞춘 프로그래밍 패러다임
    - 대표적인 구조 : 어떤 값이 변경이 되면 이 값이 변경되었는지 쳐다보고(Subscribing) 있는 Observer에게 알림을 줘, 리랜더링 하도록 알려주는 흐름
- Redux와의 차이 : Action이 직접 State를 업데이트함

2) 객체 지향적
- 주로 Class를 사용. class를 이용함으로써 얻을 수 있는 구조적 장점이 있음
- Store를 클래스로 관리하면, 상태와 그 업데이트와 관련된 로직은 Store 내에 위치하게 할 수 있음 <br>
-> 상태와 관련된 역할, 책임을 Store에 부여해 상태를 직접 건드리는 로직을 컴포넌트로부터 분리
- 직접 상태에 접근해 업데이트하는 것은 클래스 내부에 있는 메서드(action)만 가능함
-> 캡슐화도 잘 적용 가능
3) 불변성 신경 쓰지 않아도 됨
- State의 불변성을 유지하기 위해 번잡스러운 코드나 ImmutableJs와 같은 라이브러리를 따로 사용할 필요 X
- 불변성을 유지하면서 State를 변경하는 코드의 단점
    - Object의 Depth가 깊어지면 코드의 가독성이 매우 떨어짐 <br>
    -> ImmutableJs 라이브러리를 사용하게 됨
    - Redux와 같이 사용하게 될 경우, 여러가지 설정이 필요함
    + 추가적인 라이브러리도 필요함
(MobX는 신경 쓰지 않아도 됨)

<br>

### MobX 기본 원칙
- action이 state를 변경하는 단방향 데이터 흐름을 사용 <br>
-> 영향을 받는 모든 view를 업데이트함

1) state가 변경되면 모든 derivation이 자동으로, 원자 단위로 업데이트 됨 <br>
-> 중간 값을 관찰 불가
2) 모든 derivation은 동기식으로 업데이트됨
    - action이 state를 변경한 직후 computed 값을 안전하게 검사 가능
3) computed 값은 느리게 업데이트 됨
    - 자주 사용되지 않는 computed 값은 부수효과(I/O)에 필요할 때까지 업데이트 되지 않음
    - 만약 view가 계산된 값을 사용하지 않으면, 가비지가 자동으로 수집됨
    - 모든 computed 값은 순수해야 하며, state를 바꾸면 안됨

<br>

### MobX Core 개념
- Observable : State의 변화를 감시하는 역할
    - state를 저장하는 추적 가능한 필드임을 의미
- Action : State를 수정하는 메서드 <br>
ex) 카운터 예제에서 숫자를 증가/감소시키는 등의 액션이 존재
- Computed : state의 변화로 인해 계산된 값
    - 일종의 캐싱
    - computed 내부에서 사용하는 state가 변경되었을 때만 새로 계산해 계산 값을 저장해 놓고 사용
    - 만약 computed 내부 state가 변경되지 않았다면, 기존에 계산해 놨던 캐싱 값을 그대로 다시 사용
- Reaction : observable state를 변경하면 그에 따른 파생 값(computed)이 계산됨
    - 만약 이 파생 값들을 사용하고 있는 곳이 있다면, 거기도 자동 변경되어야 함
    - 파생 값을 사용하는 컴포넌트를 다시 렌더링하거나, 파생 값을 로그로 남기거나 하는 작업들이 Reaction

<br>

### MobX에서 구분되는 3가지 개념
1) 상태 (state) : 앱을 구동하는 데이터
- 값을 보유하고 있는 스프레드시트 셀과 같음
- 시간이 지남에 따라 변경하려는 모든 속성을 MobX가 추적할 수 있도록 observable로 표시해야 함
- observable을 사용하는 것은 객체의 속성을 스프레드시트 셀로 바꾸는 것과 같음

2) 동작 (action) : state를 변경하는 코드 조각 <br>
ex) 사용자 이벤트, 백엔드 데이터 푸시, 예약된 이벤트 등
- 스프레드시트 셀에 새 값을 입력하는 사용자와 같음
- observable을 변경하는 코드는 action으로 표시하는 것이 좋음
- action을 사용하면 코드를 구조화하는 데 도움을 줄 수 있음
- 의도하지 않은 state 변경도 방지

3) 파생 (derivation) : state에서 더 이상의 상호작용 없이 파생될 수 있는 모든 것 <br>
ex) 사용자 인터페이스, 남은 todos 수와 같은 파생 데이터, 백엔드 api 요청
- 두 종류의 파생 값이 존재 <br>
    i. computed : 현재의 observable state에서 순수 함수를 사용해 파생될 수 있는 값 <br>
        - js getter 함수를 사용하면 됨 <br>
        - 스프레드시트 자동 저장처럼, 자동으로 업데이트됨 <br>
        - 필요할 때 (무언가 결과에 영향을 미칠 수 있을 때)만 업데이트 됨 <br>
    ii. reaction : state가 변경될 때 자동으로 발생해야 하는 부수효과 (side effect) <br>
        - 명령형 프로그래밍과 반응형 프로그래밍 사이를 연결해주는 다리 역할 <br>
        - state의 변화나 computed 값의 변화를 볼 수 있으려면, GUI의 일부를 다시 그리는 reaction이 필요 <br>
        - computed 값과 유사하지만, 정보 생성 대신 콘솔 출력, 네트워크 요청, DOM 패치 적용을 위해 React 컴포넌트 트리를 점진적으로 업데이트하는 등의 부수효과를 생성

<br>

### 리액트에 observer 적용하기 : observer HOC 사용
- React를 사용하는 경우 설치 중에 선택한 바인딩 패키지에서 observer 함수를 이용해 컴포넌트를 감싸서 반응형으로 만들 수 있음 (ex. mobx-react의 observer 혹은 Observer)
    - HOC : 리액트 컴포넌트를 argument로 받아, 새로운 리액트 컴포넌트를 리턴하는 함수

<br>

- observer는 리액트 컴포넌트를 렌더링하는 데이터의 derivation으로 변환함
- MobX에서는 필요할 때마다 컴포넌트가 다시 렌더링 되며, 그 이상은 렌더링 되지 않음
- 위 예시에서 onClick 핸들러로 toggle 액션을 사용하면, 필요한 TodoView 컴포넌트를 강제로 리렌더링함
- TodoListView 컴포넌트의 경우, 완료되지 않은 작업의 수 (unfinishedTodoCount)가 변경된 경우에만 다시 렌더링됨
- Observer를 통해 감싸줄 수도 있음

<br>

### 리액트와 MobX 같이 사용하기
- MobX는 React와 독립적으로 작동하지만, 일반적으로 React와 함께 사용됨
- 위에서 설명한 React 컴포넌트를 감싸는 observer HOC 참고하기
- Observer HOC는 렌더링 중에 사용되는 모든 observable에 React 컴포넌트들을 자동으로 구독 <br>
-> 관련 observable이 변경되면 컴포넌트들이 자동으로 다시 렌더링 됨
- 또한, 관련된 변경사항이 없을 때는 컴포넌트가 다시 렌더링 되지 않음 <br>
-> 컴포넌트로부터 접근할 수는 있지만, 실제로 읽지 않는 observable은 다시 렌더링 되지 않음 <br>
-> 따라서 위 로직에 따라 MobX는 리액트 앱을 최적화 시키고, 과도한 렌더링을 방지 가능
- React Context를 사용해 외부 state 사용 가능
    - React Context : context는 컴포넌트 안에서 전역적으로 데이터를 공유하도록 나온 개념 <br>
    그런 데이터는 로그인 데이터, 웹 내 사용자가 쓰는 설정파일, 테마, 언어 등등 다양하게 컴포넌트 간 공유되어야 할 데이터로 사용되면 좋음

- mobx-react-lite : react 컴포넌트에서 사용하는 라이브러리
    - 함수형 컴포넌트만 지원

- mobx-store-provider : useStore 함수 지원



<br>

##### 참고자료
- [React에서 Mobx 경험기(Redux와 비교기)](https://techblog.woowahan.com/2599/)
- [MobX의 장점과 기본 원칙](https://medium.com/hcleedev/web-mobx의-장점과-기본-원칙-40a36c1cf634)
