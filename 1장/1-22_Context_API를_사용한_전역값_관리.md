## 22. Context API를 사용한 전역 값 관리

### ConText API 사용하는 이유?

→ 자식컴포넌트들로 값을

프로젝트 안에서 전역적으로 사용할 수 있는 값을 관리할 수 있다!!

이 값이라는 것은 값 뿐만 아니라 상태, 함수 모두 가능하다!

외부 라이브러리, 인스턴스 등 상관 없음

## 사용방법!!

1. createContext로 새로운 context 만들어주기
1. Provider 컴포넌트로 감싸주기
1. context 값을 사용하는 곳에서 사용하지

새로운 Context 만들기 

1. createContext로 context 내보내주기

    상위 컴포넌트에 createContext로 context를 생성한다.

    ```jsx
    //App.js
    import React, {createContext} from 'react';

    const reducer = (state, action) => {
    	//dispatch를 컨텍스트로 관리할것이다. reducer 생성}

    export default UserDispatch = createContext(null)
    // UserDispatch라는 이름으로 context 생성
    ```

    createContext의 파라미터에는 Context의 기본값을 설정할 수 있다. 

    기본값이란 Context를 쓸 때 값을 따로 지정하지 않을 경우에 사용되는 값! 위에서는 null 로 생성

2. 만든 Context값을 받을 컴포넌트들감싸기

```jsx
//App.js
import React, {createContext} from 'react';

export const UserDispatch = creactContext(null);

const App() {} /////

<UserDispatch.Provider value={dispatch}>
	<CreateUser />
	<UserList />
</UserDispatch.Provider>

```

createContext를 하게되면

UserDispatch 안의 Provider라는 컴포넌트가 생긴다

따라서 UserDispatch.Provider 컴포넌트로 감싸주기

value 값에 넣어준다

3. 사용 할 장소에서 context 사용하기

```jsx
import React, {useContext} from 'react';
import {UserDispatch} from './App';
//useContext Hooks 임포트, UserDispatch Context 임포트

//UserList.js

const UserList = () => {

	const dispatch = useContext(UserDispatch);
	// useContext라는 Hooks를 사용해서 컨텍스트 값 사용 가능

	const onRemove  = (id) => {
		dispatch({
			type:"REMOVE",
			id:id})
	}//App.js안에 있는 reducer에 해당하는 dispatch를 사용할 수 있다.

}

```

## 여러개의 contextAPI 사용하기

[https://ridicorp.com/story/how-to-use-redux-in-ridi/](https://ridicorp.com/story/how-to-use-redux-in-ridi/)

여러개의 context를 사용하면 컴포넌트를 계속 감싸야 하기때문에 복잡하기도 하고

userupdate의 경우 리랜더링이 여러번 일어나기 때문에. 감싸줄수 있다

```jsx
import React, { createContext, useState, useContext } from "react";

const UserContext = createContext(null);
const UserUpdateContext = createContext(null);

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  return (
    <UserContext.Provider value={user}>
      <UserUpdateContext.Provider value={setUser}>
        {children}
      </UserUpdateContext.Provider>
    </UserContext.Provider>
  );
}

function useUser() {
  return useContext(UserContext);
}

function useUserUpdate() {
  return useContext(UserUpdateContext);
}

function UserInfo() {
  const user = useUser();
  if (!user) return <div>사용자 정보가 없습니다.</div>;
  return <div>{user.username}</div>;
}

function Authenticate() {
  const setUser = useUserUpdate();
  const onClick = () => {
    setUser({ username: "velopert" });
  };
  return <button onClick={onClick}>사용자 인증</button>;
}

export default function App() {
  return (
    <UserProvider>
      <UserInfo />
      <Authenticate />
    </UserProvider>
  );
}
```