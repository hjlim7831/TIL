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

##### 참고자료
- [React에서 Mobx 경험기(Redux와 비교기)](https://techblog.woowahan.com/2599/)
- [MobX의 장점과 기본 원칙](https://medium.com/hcleedev/web-mobx의-장점과-기본-원칙-40a36c1cf634)
