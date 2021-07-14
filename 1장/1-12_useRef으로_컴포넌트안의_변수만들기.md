## 12. useRef로 컴포넌트 안의 변수 만들기

- useRef 관리되는 변수는 값을 바꾼다고해서 리렌더링 되지 않는다,
- 컴포넌트 안의 상태값을 조회하려면 상태를 바꾸는 함수호출 → 랜더링 → 조회가 가능하지만

    useRef는 설정후 바로 호출이 가능하다

### useRef를 사용하여 관리할 수 있는 값

- `setTimeout`, `setInterval` 을 통해서 만들어진 `id`
- 외부 라이브러리를 사용하여 생성된 인스턴스
- scroll 위치

등등의 값을 변경할 수 있다.

```jsx
import React from 'react'
import UserList from './Userlist.js'

function App(){
	const users = [{
      id: 1,
      username: 'velopert',
      email: 'public.velopert@gmail.com'
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com'
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com'
    }];

	const nextId = useRef(4) //여기서 4가 파라미터. 파라미터 넣으면 이게 current 기본값이됌
	const onCreate = () => {
		//create 구현

		nextId.current += 1;

		//파라미터 넣은 값인 4가 추가되면 입력될 id값임.
		// 
		

}
 return (
		<UserList users={users}/>
);
	
}export default App;
```

### 질문

useRef의 두가지 용도는?, useRef 변수가 담겨있는 위치는?