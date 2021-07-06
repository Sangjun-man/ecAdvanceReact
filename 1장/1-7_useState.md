### 7. useState를 통해 컴포넌트에서 바뀌는 값 관리하기

### 이벤트 설정

```jsx
import React from 'react'

function Counter(){
	const number = 1;
	const onIncrease = () =>{
	console.log(+1);
}
// 이벤트설정 -> 이벤트 발생하게 하려면 함수형태로 작성, 이번엔 화살표 함수로 작성됨.
// 화살표 좌측에 파라미터, 오른쪽에 코드 구현 
	const onDecrease = function () {
        console.log(-1);
    } // 화살표함수와 같은 식.
	const abc = () =>{
	console.log("abc");
		// 으로만 처리할때는, on 붙이지 않아도 됨.
}

return(
<div> {number} </div> // const 선언된 1이 등장.
<button onClick={onIncrese}> +1</button> //버튼 클릭시, onIncrease 함수 실행.
<button onClick={onDecrease}> -1</button>  
<button onClick={abc}> abc </button> // 콘솔에 abc 출력.

)

```

동적인 요소를 넣기 위해 onClick 속성으로 이벤트를 발생시킬 수 있게 만들고, 이벤트 발생시 특정 함수를 호출하게 코드를 짰다

이때 이벤트를 설정할 때 함수를 실행시키면 안되고 함수 형태를 넣어주어야 한다.

ex) onClick={onIncrease() } X ⇒ 랜더링 시 함수가 호출되어버린다.

onClick={onIncrease} O

### 동적인 값 끼얹기

useState를 활용해서 본격적으로 상태변화시켜보자

```jsx
import React, {useState} from 'react'

function Counter(){
    const consoleLog = (a) => {console.log(a)}
    const [number, NumNum] = useState(1); //useState( )안에 넣어준 값이 number 초기값으로 설정됨.
	
	
	/*const [number,setNumber] = useState(0); 는
		const numberState = useState(0);
		const number = numberState[0];
		const setNumber = numberState[1]; 를 비구조화 할당 한것,*/

    const onIncrease = () => {
          NumNum(number + 1); // useState의 기능을 사용한거 같다.
					//예제에서는 NumNum 대신 setNumber 사용, 이름을 NumNum으로 바꿔도 작동한다.
	        consoleLog(number);// onClick이벤트 활성화 시 이 함수도 같이작동한다.   
        }
    const onDecrease = function () {
	        NumNum(number -1);
				//setNumber(number -1);  예제에는 이렇게.
    } 
    const abc = () => {console.log("abc")}
return (
    
<div>
    <h1> {number} </h1>
    <button onClick={onIncrease}>+1</button>
    <button onClick={onDecrease}>-1</button>
    <button onClick={abc}> abc </button>
</div>

);

} export default Counter;
```

useState로 다룰상태와 변화시킬 함수 setNumber 를 선언해준다

setNumber({변화시킬 상태값}); 같은 형식으로 상태값을 변화시킬 수 있다.

### 함수형 업데이트

```jsx
import React, {useState} from 'react'

function Counter(){
    const concon = (a) => {console.log(a)}
    const [number, setNumber] = useState(1); //number의 초기값 1
    const onIncrease = () => {
          setNumber(prevNumber =>
									  prevNumber +=1 }
    const onDecrease = function () {
          setNumber(number => number -1);
    } 
    const abc = () => {console.log("abc")}
return (
    
<div>
    <h1> {number} </h1>
    <button onClick={onIncrease}>+1</button>
    <button onClick={onDecrease}>-1</button>
    <button onClick={abc}> abc </button>
</div>

);

} export default Counter;
```

setNumber에 상태값이 아닌 함수를 넣어주면, 함수 실행 이전 상태값을 받아와 함수를 실행시키고 그 결과값을 상태로 다시 반환한다

참고 : [https://ko.reactjs.org/docs/hooks-reference.html#usestate](https://ko.reactjs.org/docs/hooks-reference.html#usestate)

### 질문

useState를 비구조화 할당을 사용하지 않고 만든다면 어떻게 할당해야 하는가?

답

```jsx
const [number, setNumber] = useState(0);

const numberState = useState(0);
const number = numberState[0];
const setNumber = numberState[1];
```