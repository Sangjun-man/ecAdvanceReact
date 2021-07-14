## 1.10 useRef로 특정 DOM 선택하기

ref = reference. 참조 . current.focus()등으로 접근하려고 사용,

const ref = useRef(); 으로 Ref 만들고 태그에 ref 설정 해두면, ref 변수 따라서 div등 변수에 접근 가능한듯 하다. focus같은 함수는 input 에 포커스 가능, id , text 등등 값에 접근할 수 있다.

```jsx
import React, {useState , useRef} from 'react'

function InputSample (){
	const [inputs, setInput] = useState({name: '', nickName:''});
	
    const nameInput = useRef(); // useRef 사용. reference

    const {name, nickName} = inputs; // 여러개의 입력 관리할것임.
    const onChange =(e) =>{
	    const {value, name} = e.target // 객체 비구조화 할당
	    setInput({
	        ...inputs, //spread 문법.
		    [name]: value  //입력시 {e.target[name]} : {e.target.value}
		    });            //
}   
    const onReset = () => {
        setInput({
            name: '', //입력값 초기화 해주는 setInput.
            nickName:''
        })
        nameInput.current.focus(); // 위에서 초기값 세팅 후에, focus를 nameInput으로.
        console.log(nameInput);
				//여기서 nameInput은 무엇이냐.?>
    }
    const onMouseMove = ()=>{
        console.log('마우스move'); //이건 그냥 마우스 무브
    }
    const style = { width : 100, height : 1000} ;
	return (
	<div>
		<input name='name'
               placeholder='name'
               onChange={onChange}
               value={name}
               ref={nameInput}></input> {/*여기 ref를 설정해줘야, current.focus로 접근가능. ref를 만들어 두어야 */}
	

		<input name='nickName'
               placeholder='nickName'
               onChange={onChange}
               value={nickName}
               ref={nameInput}></input>
        <div>
	    <b>값:{name}({nickName})</b>
        <button onClick={onReset}>초기화</button>
        </div>
        <div style={style} onMouseMoveCapture={onMouseMove}>여기보세요</div>
	</div>
	);
}

export default InputSample;

```