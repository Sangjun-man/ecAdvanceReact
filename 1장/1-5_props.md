# 5.props를 통해 컴포넌트에게 값 전달하기

## props란?

props란, properties의 줄임말로 컴포넌트에 어떠한 값을 전달해주어야 할때 사용.

### App.js(상위 컴포넌트)에서 props 전달.

App.js에서 컴포넌트들로 props를 전달해 줄수 있다.

```jsx
//App.js
import React from 'react';
import Hello from './Hello';

function App(){
	return (
		<Hello name="Name" color="Color" asdfasdf="props" />
	);	
}

export default App;
```

```jsx
//Hello.js
import React from 'react';

function Hello(props){
	return <>
			<div>안녕하세요 {props.name}</div>
			<div>Props = {props.asdfasdf}</div>			
			</>
}

export default Hello;
```

App.js 에서 Hello 컴포넌트로 props 전달을 하고있다

### 객체 비구조화 할당으로 값 전달하기

- 원래대로 props를 사용한다면.

    ```jsx
    //App.js

    import React from 'react';
    import Hello from './Hello';

    function App(){

    	return (
    		<Hello name="react" color="red"/>
    	);
    }

    export default App;
    ```

    ```jsx
    // 원래대로 하면
    //Hello.js
    function Hello(props){
    	return(<div name="props.Name" color="props.Color"> </div>);
    } 
    export default Hello;
    // props 전달 시에 props = {name= "Name", color="color"} 라는 객체로 전달.
    // const {name , color} = props → 객체 비구조화 할당,
    ```

    props.{NAME} 이런식으로 props 안에서 꺼내써야함.

- 객체 비구조화 할당을 사용하면

    ```jsx

    //객체 비구조화 할당 사용
    //Hello.js
    import React from 'react';

    function Hello({name,color}){
    return(<div name={name} color={color}> </div> );
    }
    export default Hello;
    ```

    변수명인 name, color 으로  props를 전달받아 사용할 수 있다.

### defaultProps 설정

```jsx
//App.js에서
import React from 'react';

function App(){
	return (<> 
		<Hello name="name" color="red" />
		<Hello color="pink" />
	</>
}
export default App;
```

```jsx
//Hello.js에서

import React from 'react';
function Hello({name, color}){
	return(
		<div name={name} style={{color : color}}> 
			안녕하세요 {name}
		</div>
	 )
;}

Hello.defaultProps(
	name = "디폴트값임". 
); export default Hello;   

//Component.defaultProps 으로 설정.
```

실제 출력해보면 이런식으로 디폴트값이 설정 된 것을 알 수 있다!

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/027273d9-36ec-4f0d-af25-d78734ebf825/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/027273d9-36ec-4f0d-af25-d78734ebf825/Untitled.png)

### props.Children (컴포넌트 태그 안의 값 조회)

```jsx
//App.js에서
import React from 'react';
import Hello from './hello';
import Wrapper from './Wrapper';

function App(){

return(
	<Wrapper> 
		<Hello name="name" color="pink" />
		<Hello color="red" /> {/* Hello 다른 프롭스의 컴포넌트 두개를 Wrapper에 삽입 */}
	</Wrapper>
	);
}

export default App;
```

```jsx
//Wrapper.js 에서
import React from 'react';

function Wrapper({children}){
	const style = {
		border : '2px solid black',
		padding : '16px',
	};

	return(
		<div style={style}>
			{children}
		</div>
	)
}

export default Wrapper;

```

Wrapper의 div 태그 안에 {children}으로 값을 넣어주지 않으면 Hello 컴포넌트에 들어가는 name, color 등의 프롭스값을 읽어오지 못한다!

### 질문

```jsx
import React from 'react';
function Hello({name, color}){
	return(
		<div name={name} style={{color : color}}> 
			안녕하세요 {name}
		</div>
	 )
;}
export default Hello;
```

위의 코드에서 style 함수에서 {{}} 이중으로 중괄호 해준 이유는??

답 :객체 리터럴

{"color" : color} 앞의 color는 키값으로 들어가고 뒤의 color는 부모컴포넌트에서 보내준 props 값을 가져온다. css 표현을 하기위한 객체가 만들어졌다고 생각하면 될듯.

 {{color : color}} 이중 중괄호 중 바깥쪽 중괄호는 jsx에서 사용되는 문법. jsx는 자바스크립트 표현을 {}안에 넣어서 사용할 수 있다
