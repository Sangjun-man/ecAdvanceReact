## 21. 커스텀 Hooks 만들기

### 커스텀 hooks를 사용하는이유

**반복되는 로직을 재사용 하기 위해 만든다**

input값을 받아오는 로직의 경우, 로그인 할때 회원가입 할때 등등 여러곳에 사용될 수 있는데 그럴때마다 바녹되는 코드들이 있으니 그것을 customHooks로 만들어서 사용할 수 있다

사용방법:

use 라는 키워드로 시작하는 파일을 만든후, useState, useReducer, useCallback 등 원하는 기능을 사용하고, 컴포넌트에서 사용할 값들을 반환해주기

> input값, hooks 내의 함수, output값을 생각해서 만들면 된다!!

실제 만들어보기

1. useState 사용

```jsx
//useInputs.js -> 파일 만들기

import useState, useCallback from 'react';

const useInputs = (initialInputs) =>{
	const [inputs, setInputs] = useState({initialInputs});
	
	const onChange = (e) => {
		const {name, value} = e.target
		console.log(name, value);//
		setInputs({
			...inputs, //불변성 지켜주기
			[name]:value}) //e.target.name을 ["key"] 형식으로 해서 값을 불러온것
	}
	const reset = () =>{
		setInputs(initialInputs); //불변성 지켜주기
	}
	return [inputs, onChange , reset];
}
export default useInputs;

//useInputs 이라는 hooks를 사용할때는 initialForm을 인자로 넣어주어야 하고
//꺼내 쓸때는 form, onChange, reset 꺼내 쓰면 된다
//함수형 프로그래밍이 이렇게 진행되는것, 들어가는것 함수처리 나오는것 확인.
 
```

2. useReducer 사용

```jsx
import useReducer, useState from 'react';

const reducer = (state, action)=> {
	const {name, value} = action
	switch(action.type){
		case "CHANGE_INPUT":
			return (
				...state, [name]:value);
		case "RESET_INPUT":
			return Object.keys(state).reduce((acc, current)=> {
				acc[current} = ''; //object reduce 메서드 같은데 잘 몰겠당
				return acc;},{});
		default:
			return state;
	}
const useInputs = (inintialForm) => {
		const [inputs, dispatch] = useReducer(initialForm);

		const onChange = (e)=>{
				const {name, value} = e.target
					dispatch({
					type:"CHANGE_INPUT"
					name,
					value });
		}
	;
		const reset = ()=>{
				dispatch({
					type:"RESET_INPUT"})};
		
return [inputs, onChange, reset]
}

export default useInputs;
```

```jsx
import React, { useRef, useReducer, useMemo, useCallback } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';
import useInputs from './hooks/useInputs';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

const initialState = {
  users: [
    {
      id: 1,
      username: 'velopert',
      email: 'public.velopert@gmail.com',
      active: true
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
      active: false
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
      active: false
    }
  ]
};

function reducer(state, action) {
  switch (action.type) {
    case 'CREATE_USER':
      return {
        users: state.users.concat(action.user)
      };
    case 'TOGGLE_USER':
      return {
        users: state.users.map(user =>
          user.id === action.id ? { ...user, active: !user.active } : user
        )
      };
    case 'REMOVE_USER':
      return {
        users: state.users.filter(user => user.id !== action.id)
      };
    default:
      return state;
  }
}

**function App() {
  const [{ username, email }, onChange, reset] = useInputs({
    username: '',
    email: ''
  });
//custom Hooks를 사용해서. 인풋값을 받기 위해 useState 만들고 setter함수로 인풋값 설정해주고 리셋해주고 하는 코드를 APP.js안에 작성하지 않아도 된다!!**
  const [state, dispatch] = useReducer(reducer, initialState);
  const nextId = useRef(4);

  const { users } = state;

  const onCreate = useCallback(() => {
    dispatch({
      type: 'CREATE_USER',
      user: {
        id: nextId.current,
        username,
        email
      }
    });
    reset();
    nextId.current += 1;
  }, [username, email, reset]);

  const onToggle = useCallback(id => {
    dispatch({
      type: 'TOGGLE_USER',
      id
    });
  }, []);

  const onRemove = useCallback(id => {
    dispatch({
      type: 'REMOVE_USER',
      id
    });
  }, []);

  const count = useMemo(() => countActiveUsers(users), [users]);
  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onToggle={onToggle} onRemove={onRemove} />
      <div>활성사용자 수 : {count}</div>
    </>
  );
}

export default App;
```