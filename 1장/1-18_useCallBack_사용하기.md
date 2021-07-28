## 1.18 useCallback을 사용하여 함수 재사용하기

useCallback이란 useMemo와 비슷한 Hook이다.

useMemo는 값을 저장하는거, useCallback은 함수를 저장해두는것 특정 함수를 새로 만들지 않고 재사용 하고싶을때 사용한다.

useCallback을 사용하지 않으면 함수는 리렌더링 될때마다 계속 불러온다.

의존하는 배열의 변화가 없다면, useCallback 에 있는 함수를 그대로 사용하게 된다.

.

```jsx
//App.js

const onCreate = () => {
    const user = {
      id: nextId.current,
      username,
      email,
    };
    setUsers(users.concat(user));

    setInputs({
      username: "",
      email: "",
    });
    nextId.current += 1;
  };
// CreateUser의 "등록" 버튼에 사용. 등록 버튼을 누루면
// CreateUser가 App.js 의 user 스테이트를 바꾸고
// App.js의 state가 바뀌면서 App.js 전체가 다시 리렌더링 되는데
// 그때 이 함수도 다시 렌더링 된다. 

  const { username, email } = inputs; //변수 비구조화 할당?
  const onChange = (e) => {
    const { name, value } = e.target;
    console.log(e);
    console.log(this);
    setInputs({
      ...inputs,
      [name]: value,
    });

    //input 값이 변할때마다 e값 콘솔에 띄움,
  };

  const onRemove = (id) => {
    // user.id 가 파라미터로 일치하지 않는 원소만 추출해서 새로운 배열을 만듬
    // = user.id 가 id 인 것을 제거함
    setUsers(users.filter((user) => user.id !== id));
  };

  const onToggle = (id) => {
    setUsers(
      users.map((user) =>
        user.id === id ? { ...user, active: !user.active } : user
      )
    );
  };
```

여기서 App.js 안에 작성된 함수들은 App컴포넌트가 리랜더링 될때마다 새로 만들어진다

- 사용방법

    ```jsx
    const onChange = useCallback(
    (e)=> {
    	const {name, value} = e.target;
    	setInputs({
    		...inputs,
    		[name] : value,
    		});  //setInputs
    	};  //e => {}
    	,[inputs]
    ); //useCallback.

    const onCCC = useCallback(() => {}, [deps])

    //요런 형식, useCallback((param) => {함수}, [의존하는 배열])
    //랜더링 할때 의존하는 배열의변화가 없다면, 그대로 저장해놓고 사용한다.

    const {username, email} = inputs;
    const onCreate = useCallback(
    	()=> {
    	const user = {
    		id:nextId.current,
    		username:username,  //username: 안써도 위에 비구조화할당 조져서, username만 써도됨
    		email:email, //마찬가지로 email만 써도됌,
    	}
    	setUsers(users.concat(user));

    ```

### 질문 :

useMemo를 사용해서 useCallback을 구현할 수 있는데 그 방법은?

### 답

```jsx
const onToggle = useMemo(
  () => () => {
    /* ... */
  },
  [users]
);
```