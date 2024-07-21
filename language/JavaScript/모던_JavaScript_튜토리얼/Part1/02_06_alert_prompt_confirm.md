### 1. alert
- 이 함수가 실행되면 사용자가 '확인(OK)'버튼을 누를 때까지 메시지를 보여주는 창이 계속 떠있게 됨

``` javascript
alert("Hello");
```

### 2. prompt
- 함수가 실행되면 텍스트 메시지와 입력 필드(input field), 확인(OK) 및 취소(Cancel) 버튼이 있는 모달 창을 띄워줌
``` javascript
// title : 사용자에게 보여줄 문자열
// default : 입력 필드의 초깃값(선택값)
result = prompt(title, [default]);
```

``` javascript
let age = prompt("나이를 입력해주세요.", 100);

alert(`당신의 나이는 ${age}살 입니다.`);
```

### 3. confirm
``` javascript
result = confirm(question);
```
- 매개변수로 받은 question(질문)과 확인 및 취소 버튼이 있는 모달 창을 보여줌
- 사용자가 확인 버튼을 누르면 true, 그 외에는 false를 반환

``` javascript
let isBoss = confirm("당신이 주인인가요?");

alert( isBoss ); // 확인 버튼을 눌렀다면 true가 출력됨
```