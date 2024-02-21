
### MST (MobX-State-Tree) 란?

- MobX 위에서 만든 상태 관리 시스템
- functional reactive state library
- MobX : 상태 관리 엔진
- MobX-State-Tree : MobX 구조화, 앱에 필요한 도구들을 제공

### MST 기본 개념
- MST는 트리 구조로 되어 있음
- 각 트리는 nodes(model)와 leaves(properties)로 구성되어 있음
- 불변성을 위해 Snapshot을 사용
- Views : memorization(기억)을 위해 사용
- 비동기적 효과 지원
- Lifecycle Method 지원

- Tree, Nodes, Leaves, Actions, Views, Snapshots



##### Snapshot vs Patch
- state로 관리되는 상태를 사진찍듯이 저장해 놓는 개념
- Snapshot : state를 그대로 저장
- Patch : 어떤 값이 변경되었는지를 저장


##### 참고 자료
- Mobx-state-tree 학습하기 #1 : Mobx-state-tree를 사용해 React state 관리하기 https://steemit.com/zzan/@anpigon/react-native-manage-application-state-with-mobx-state-tree-1
- https://velog.io/@djaxornwkd12/Mobx-State-Tree에-대해-알아보자-1
- Mobx-State-Tree에 대해 알아보자 (2) TodoList 앱 만들기 https://velog.io/@djaxornwkd12/Mobx-State-Tree에-대해-알아보자2
- https://devstarsj.github.io/development/2019/05/19/mobx-state-tree.usage/
- MST 학습하기 #12 : Flow를 사용해 비동기 프로세스 정의하기 https://steemit.com/zzan/@anpigon/react-mobx-state-tree-12-flow
- flow 사용 예시들 : https://snyk.io/advisor/npm-package/mobx-state-tree/functions/mobx-state-tree.flow
- mobx-react와 React Hooks API 함께 사용하기 : https://blog.rhostem.com/posts/2019-07-22-mobx-v6-and-react-v16-8
- Manage your state application with Mobx State Tree : https://blog.nimbleways.com/mobx-state-tree/
https://velog.io/@hojkim77/MSTMobx-State-Tree란