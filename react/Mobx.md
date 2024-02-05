
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

		




##### 참고자료
- [React에서 Mobx 경험기(Redux와 비교기)](https://techblog.woowahan.com/2599/)
- [MobX의 장점과 기본 원칙](https://medium.com/hcleedev/web-mobx의-장점과-기본-원칙-40a36c1cf634)