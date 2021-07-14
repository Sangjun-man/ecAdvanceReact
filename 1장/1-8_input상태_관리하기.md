## 8. input 상태 관리하기

```jsx
import React, {useState} from 'react'

//text 에서도 useState 사용.

function InputSample(){
    const [text, setText] = useState(''); //변수와 함수 설정.
																					//useState에서 변수할당 따로 안하면 앞에 적어둔 변수로 알아서 입력되는듯 하다.
    const onChange = (e) =>{ //JS에서는 e로 인풋값을받는다.
        setText(e.target.value); //e에 엄청난 정보들이 숨어있다.
    }
    const onReset = () => {
        setText(''); //변수를 ''로 만들어주기
      };
    const onAlert = () => {
        alert(text); // 그냥 함수, alert와 console
        console.log(text);

    }
    
    return(
        <> 
        <div>
            <input onChange={onChange}></input> 
						{/* input에 변화가 있을때마다 onChange가 알아채림 */}
            <button onClick={onReset}>초기화</button>
            <button onClick={onAlert}>출력</button>
            </div> 
            <b>값 : {text} </b>

        </>
    );
}

export default InputSample;
```

### 질문

onChange 안의 e는 어떤 것을 의미하는가?

- 답

    DOM과 관련된 모든 이벤트가 발생하면 관련 정보가 모두 담기는 객체.