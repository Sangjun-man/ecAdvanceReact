## 17. useMemo를 사용하여 연산한 값 재사용하기

성능 최적화를 위하여 useMemo라는 Hook을 사용하여 재사용 한다!

```jsx
//App.js
import React, { useRef, useState } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

function App(){

const [users, setUsers] = useState([
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
  ]);

...

const onToggle = (id) => {
	setUsers(users.map(user =>
        user.id === id ? { ...user, active: !user.active } : user
			)
		)	
};

const count = counterActiveUsers(users);

return <></>

}

export default App
//users가 정리 된 이후에 count를 써야함. 저 아래로 내려야함

```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56c52e4f-62d6-42ca-a3e2-b42bed4c445d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56c52e4f-62d6-42ca-a3e2-b42bed4c445d/Untitled.png)

인풋에 값을 입력할때마다 콘솔창에는 .. → onChange 될때마다 리렌더링 되고있다는 소리

onChange ⇒ App.js의 상태값 변화 시킴. setInput으로 App.js 컴포넌트 안의 state값을 변화시키고 있다.

```jsx
//App.js안에서

const [inputs, setInputs] = useState({
    username: "",
    email: "",
  });
// 인풋값의 상태변화를 사용하겠다.

const onChange = (e) => {
    const { name, value } = e.target;
    console.log(e);
    setInputs({
      ...inputs,
      [name]: value,
    });

    //input 값이 변할때마다 e값 콘솔에 띄움,
  };
//Input값 밸류를 얻기 위해 setInputs으로 value값 변화를 계속 확인하겠다.

const counterActiveUsers = (users) => {
    console.log("활성 유저를 세는중");
    return users.filter((user) => user.active).length;
  };
  const count = counterActiveUsers(users);
  // const count = useMemo(() => counterActiveUsers(users), [users]);

return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} onToggle={onToggle} />
      <div>활성사용자 수 : {count}</div>
    </>
  );

//App.js 안의 usestate
```

### 그래서 이걸 막으려면 useMemo 사용

```jsx
const count = useMemo(() => 	countActiveUsers(users),  [users])
```

첫번째 파라미터에 오는 함수를 통해 어떻게 연산할지 정의
위 함수는 users.filter(user => user.active).length 가 나옴. length값 리턴 */
두번째 파라미터에는 deps 배열을넣어준다.   뒤의 []에 담긴 users가 변해야 유즈메모안에 등록된 함수를 작동시킨다.

### 질문

useMemo를 사용하는 이유는??

답 : 

이미 연산된 값을 리랜더링 하지 않음으로써 성능을 최적화 하기 위해 사용