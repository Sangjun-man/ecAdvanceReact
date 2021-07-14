## 1.9 여러개의 Input 상태 관리하기

```jsx
import React, {useState} from 'react'

function InputSample (){
	const [inputs, setInput] = useState({name: '', nickName:''}); // useState 사용 왜 배열?
	const {name, nickName} = inputs; //객체 비구조화 할당, 이거 안하면 inputs.name inputs.nickName으로 접근해야함. 
    const onChange =(e) =>{ // e를 받는다, e는 이벤트 모음집
	    const {value, name} = e.target
	    setInput({
	        ...inputs, //spread 문법, inputs를 전체 복사해온다.
		    [name]: value // name 키에 해당하는 값을 (e.target.)value 로 설정
		    });
        console.log(e)
}
    const onMouseMove = ()=>{
        console.log('마우스move');
    }
    const style = { width : 100, height : 1000} ;
	return (
	<div>
		<input name='name' placeholder='name' onChange={onChange}></input>
		<input name='nickName' placeholder='nickName' onChange={onChange}></input>
        {/*인풋 안에는 값을 넣으면 안된다*/} 
	    <b>값:{name}({nickName})</b>
        <div style={style} onMouseMoveCapture={onMouseMove}>여기보세요</div>
	</div>
	);
}

export default InputSample;
```

### ...(spread 문법) 사용하는 이유

랜더링에서 변화를 관찰하기 위해선

객체값을 바로 바꿔주는게 아니라, 복사해와서 그 복사한 값에서 바꿔주기 

원래값을 바꾸면 바뀐 값을 비교할수 없어서 변화를 감지하지 못한다.

### 질문

두 input 태그의 value를 구분하기 위해 사용한 방법은?

- 답

    name 프로퍼티를 설정하고 이벤트가 발생했을때 참조한다.