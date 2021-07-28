## 23. Immer를 사용한 더 쉬운 불변성 관리

- 리액트에서 배열이나 객체를 업데이트 할때는 직접수정해서는 안되고 불변성을 지켜주어야 한다!!

    → splice, push, n번째 항목 수정 x

    ⇒ concat, filter, map 등으로 새 객체를 만들어주어야한다

- immer를 사용하면 immer가 알아서 불변성 관리를 해줌!!

- produce(state, draft⇒{}) 두개의 파라미터를 넣게되면 새로운 상태를 반환하고

    produce(draft⇒{})한개의 파라미터만 넣는다면, 상태 업데이트 함수를 반환해준다!

### 사용법

1. 라이브러리 설치

    ```jsx
    $ yarn add immer
    ```

2. Immer 불러오기

    ```jsx
    import produce from 'immer';

    //produce를 불러온다
    ```

3. produce 사용법

    > return produce(state, (draft)⇒{ 업데이트를 정의하는 함수 })

    ```jsx

    //리듀서 안에서 사용

    case first:
    return produce(state, (draft) => {
            const index = draft.users.findIndex((user) => user.id === action.id);
            draft.users.splice(index, 1);
          });

    //원래는 이런식으로 했다
    //return {
    //...state,
    //users: state.users.filter((user) => user.id !== action.id),
    //};

    ```

4. useState에서 사용

    ```jsx
    const todo = {
      text: 'Hello',
      done: false
    };

    const updater = produce(draft => {
      draft.done = !draft.done;
    });

    const nextTodo = updater(todo);

    console.log(nextTodo);
    // { text: 'Hello', done: true }
    // immer를 사용해서 함수를 정의하고, 처음 상태를 받아서 새로운 상태를 반환받았다

    const [todo, setTodo] = useState({
      text: 'Hello',
      done: false
    });

    const onClick = useCallback(() => {
      setTodo(
        produce(draft => {
          draft.done = !draft.done;
        })
      );
    }, []);

    //produce(draft =>{}) 이걸로 상태 변화를 정의하는 함수를 만들어서 사용.
    ```

### 주의할점

1. 큰 차이가 없을 수 있지만 일반 JS 문법보다는 immer를 쓸때 더 느리다
2. React-native 에서는 ES5문법을 사용하기때문에 속도가 느리다,

### 질문

state와 draft는 어떤 차이가 있는가?

**답**

state는 불변성을 지켜주면서 업데이트 해야 하는 상태,

draft는 업데이트 함수에서 사용하는 파라미터 이다

produce(state, draft ⇒{}) : state가 뒤의 업데이트 함수의 인자가 됨

produce(draft ⇒ {}) : 이것은 불변성을 지켜주는 업데이트 함수를 의미하고. 실제 사용시에 넣어주는 값이 업데이트 함수의 인자가 됨

```jsx
const todo = {
  text: 'Hello',
  done: false
};

const updater = produce(draft => {
  draft.done = !draft.done;
});

const nextTodo = updater(todo);

console.log(nextTodo);
// { text: 'Hello', done: true }
```