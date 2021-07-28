# 20.useReducer를 사용하여 상태 업데이트 로직 분리하기

### useReducer 이해하기

useState에서는 결과를 setter함수에 넣어서 상태를 바꾼다면

useReducer는 여러개의 동작을 구분하고 그 동작에 따라 상태를 바꿀 수 있는것

### useReducer에 필요한 요소

**액션** 

액션에는 업데이트를 위한 정보를 가지고 있습니다. 주로 type 값을 지닌 객체 형태로 사용하지만, 꼭 따라야 할 규칙은 따로 없습니다.

```jsx
// 카운터에 1을 더하는 액션
{
  type: 'INCREMENT'
}
// 카운터에 1을 빼는 액션
{
  type: 'DECREMENT'
}
// input 값을 바꾸는 액션
{
  type: 'CHANGE_INPUT',
  key: 'email',
  value: 'tester@react.com'
}
// 새 할 일을 등록하는 액션
{
  type: 'ADD_TODO',
  todo: {
    id: 1,
    text: 'useReducer 배우기',
    done: false,
  }
```

type 값에서 어떤 동작을 취할 것인지를 적고, 그 동작에 필요한 정보들을 추가하여 액션객체를 만들게 됩니다!

 

**리듀서**

리듀서는 는 현재 상태와 액션 객체를 파라미터로 받아와서 새로운 상태를 반환해주는 함수입니다.

reducer는 새로운 상태를 만드는 로직을 작성해야 합니다.

switch문을 사용해서 자신이 원하는 동작에서 맞는 상태 값을 리턴해줄수 있습니다

```jsx

function reducer(state, action) {
  switch (action.type) {
//스위치 구문에서는 액션 타입값을 확인하여 동작을 구분한다
    case 'INCREMENT':
      return state + 1;
 //구분된 동작에서는 상태를 바꾸는 로직을 통해 새로운 상태를 리턴해준다. 
//실제 여기서 상태값이 변화함
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```

**디스패치**

useState의 setter 함수처럼 상태를 바꾸기 위해 dispatch를 사용합니다

파라미터로 액션을 받고 이 액션값을 리듀서에 전달해주는 역할을 합니다

리듀서에 적어놓은 상태를 바꾸는 로직을 실행시키려면

dispatch({action})을 사용해야 합니다

### 예시 코드

useReducer를 사용하려면

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

[상태, 디스패치] = useReducer(리듀서, 처음 상태)

로 선언을 먼저 해주어야 합니다

**상태 로직 리듀서에 작성**

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```

**핸들링**

```jsx
function Counter() {
  const [number, dispatch] = useReducer(reducer, 0);

  const onIncrease = () => {
    dispatch({ type: 'INCREMENT' });
  };

  const onDecrease = () => {
    dispatch({ type: 'DECREMENT' });
  };
```




### Counter.js

```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}

function Counter() {
  const [number, dispatch] = useReducer(reducer, 0);

  const onIncrease = () => {
    dispatch({ type: 'INCREMENT' });
  };

  const onDecrease = () => {
    dispatch({ type: 'DECREMENT' });
  };

  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}

export default Counter;
```

1. 리듀서로 상태 업데이트 로직을 만들고, 분리할 수 있습니다
2. useReducer 사용 시 [상태, 디스패치] = useReducer(리듀서, 처음 상태)로 선언해줍니다
3. dispatch를 사용한 핸들링 함수를 만들어줍니다, dispatch는 액션을 인자로 받습니다
4. 랜더링 시 원하는곳에 이 핸들링 함수(onDecrease, onIncrease)를 넣어줍니다 






### 질문

useReducer에서 state를 변경하려면  사용해야 하는것은?

### 답

dispatch를 사용한다