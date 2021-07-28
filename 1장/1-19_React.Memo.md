# 1.19 React.memo를 사용한 컴포넌트 리렌더링 방지

컴포넌트의 props가 바뀌지 않았다면 리렌더링을 방지하여 컴포넌트의 리렌더링 성능 최적화를 해줄수 있는 함수이다

### 사용방법

> const Component = React.memo( function )

> export default React.memo(Component)

이런식으로 함수를 선언할때 React.memo로 감싸주거나

컴포넌트를 내보낼때 React.memo로 감싸주면 완료

```jsx
importct from 'react';

const User = React.memo(function User({ user, onRemove, onToggle }) {
//선언시에 react.memo  
return (
    <div>
      <b
        style={{
          cursor: 'pointer',
          color: user.active ? 'green' : 'black'
        }}
        onClick={() => onToggle(user.id)}
      >
        {user.username}
      </b>
      &nbsp;
      <span>({user.email})</span>
      <button onClick={() => onRemove(user.id)}>삭제</button>
    </div>
  );
});

function UserList({ users, onRemove, onToggle }) {
  return (
    <div>
      {users.map(user => (
        <User
          user={user}
          key={user.id}
          onRemove={onRemove}
          onToggle={onToggle}
        />
      ))}
    </div>
  );
}

export default React.memo(UserList);
//export시에 React.memo

```

### 함수에 의존하는 배열이 있을때

이경우 onRemove, onToggle 등에서 users 배열에 의존성을 가지고있으므로, users의 내용이 바뀔때마다 함수가 다시 랜더링되므로, 그 함수가 포함하는 컴포넌트들도 계속해서 랜더링된다.

```jsx
//App.js

const onCreate = useCallback(() => {
  const user = {
    id: nextId.current,
    username,
    email
  };
  setUsers(users.concat(user));

  setInputs({
    username: '',
    email: ''
  });
  nextId.current += 1;
}, [users, username, email]);
//이때 onCreate가 참조하는 deps가 변할때마다 함수가 새로 만들어진다. -> 그 컴포넌트들도 다시랜더링 된다

const onRemove = useCallback(
  id => {
    // user.id 가 파라미터로 일치하지 않는 원소만 추출해서 새로운 배열을 만듬
    // = user.id 가 id 인 것을 제거함
    setUsers(users.filter(user => user.id !== id));
  },
  [users]
);
const onToggle = useCallback(
  id => {
    setUsers(
      users.map(user =>
        user.id === id ? { ...user, active: !user.active } : user
      )
    );
  },
  [users]
);

```

### 함수형 업데이트를 통해 이같은 랜더링을 방지해 줄 수 있다.

바로 deps 에서 users 를 지우고, 함수들에서 현재 useState 로 관리하는 users 를 참조하지 않게 하는것입니다. 

```jsx
const onRemove = useCallback(id => {
    setUsers(users => users.filter(user => user.id !== id));
  }, []);
// 빈 배열을 참조하고 
  const onToggle = useCallback(id => {
    setUsers(users =>
      users.map(user =>
        user.id === id ? { ...user, active: !user.active } : user
      )
    );
  }, []);
```

### 질문

React.memo에서 함수 재선언에 의한 리랜더링을 방지하기 위해 사용하는 방법은?

### 답

함수형 업데이트를 통해 의존하는 배열을 업데이트 해준다.