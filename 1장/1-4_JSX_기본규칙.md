## 4. JSX의 기본 규칙 알아보기

JSX는 리액트에서 생김새를 정의할 때 사용하는 문법, HTML같이 생겼지만 실제로는 javascript이다.

### 변환 예시

```html
<div>
	<b> HEllo </b> World!
</div>
```

바벨에서 변환을 시켜준다면

```jsx
"use strict"

React.createElemnet("div",null,
	React.createElement("b",null,"Hello"),"World");

```

위와 같이 XML을 javascript로 변환시켜준다.

## JSX 규칙

### 태그는 닫혀있어야 한다 , <div></div>

- 태그를 열었다면 반드시 닫아주어야 한다.
- Self closing 태그 사용가능, 태그와 태그 사이에 내용을 넣지 않을때 사용한다

    → <div /> : 그냥 열고닫는 태그

### 두개 이상의 태그는 무조건 하나의 태그로 감싸져있어야 한다.

div로 묶기 애매한 상황이라면 Fragment 사용, <> </> (빈 태그)

### JSX 안에서 자바스크립트 값을 사용하려면

- css 인라인형태인 경우는 카멜케이스로작성하기.
- class 설정은 className으로.
- JSX 내부에 변수를 보여주려고 하면 {} 중괄호사용.

```jsx
const style = { fontsize : 10px ; background-color : gray;}
const name = "name"
const id = "id"

function a()
{

return(
	<div style={style}> </div>
	<div name={name} id={id}></div>
	<div className={class}> </div>
);

}

export default a;
```

- 주석을 사용하려면 {/* 이안에 주석 작성 */}

    열리는 태그 안에서는 // 형태로도 주석 작성이 가능하다.

### 질문

- JSX 내에서 변수를 사용하려면 어떻게 해야하는가?

    답 : {}로 변수를 감싸서 사용한다.