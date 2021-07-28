# 16. useEffect를  사용하여 마운트 / 언마운트 /  업데이트시 할 작업 설정하기

## 어디에 사용되는가?

> **컴포넌트가 마운트 됐을때 , 언마운트 됐을때, 업데이트 될때 side effect를 사용하고 싶을 때 처리!**

side effect란 ?

함수 절차의 실행에 의해서 발생되는 외부적인 작용이고, 함수의 결과 값을 발생시키는 작용과는 다른 것을 말한다.

데이터 가져오기, 구독(subscription) 설정하기, 수동으로 리액트 컴포넌트의 DOM을 수정하는 것까지 이 모든 것이 side effects입니다. 이런 기능들(operations)을 side effect(혹은 effect)라 부르는 것이 익숙하지 않을 수도 있지만, 아마도 이전에 만들었던 컴포넌트에서 위의 기능들을 구현해보았을 것입니다

함수형 컴포넌트는 인풋이 들어오면 결과값은 return으로 내보내 주는 jsx이고, 데이터 가져오기, 구독 등등의 것들은 그 결과값을 발생시키는 작용과는 다른 것이므로, side effect 이다.

동기화를 구현하는것에 가깝다

> 요약하면 처음 나타날때, 사라질때, 업데이트 될때 함수형 컴포넌트 내에서 특정 작업을 처리한다.

```jsx
useEffect(()=> {} , [])
```

첫번째 파라미터로 함수를 받고
두번째 파라미터로 의존값이 들어있는 배열을 넣는다. 이 배열안에 있는 

 이 배열이 비어있다면 컴포넌트가 처음 나타날때만 useEffect에 등록된 함수가 호출됨.

```jsx

useEffect(() => {
    console.log('컴포넌트가 화면에 나타남');
    return () => {
      console.log('컴포넌트가 화면에서 사라짐');
    };
  }, []);
```

useEffect에서는 함수를 반환해 줄 수 있늗네 이것을 cleanup 함수라고 부름,

deps가 비어있는 경우 컴포넌트가 사라질때 cleanup 함수가 호출된다.

cleanup 함수란 useEffect의 뒷정리를 해주는 함수?

### 마운트 / 언마운트시에 사용하기

주로, 마운트 시에 하는 작업들은 다음과 같은 사항들이 있습니다.

- `props` 로 받은 값을 컴포넌트의 로컬 상태로 설정
- 외부 API 요청 (REST API 등)
- 라이브러리 사용 (D3, Video.js 등...)
- setInterval 을 통한 반복작업 혹은 setTimeout 을 통한 작업 예약

그리고 언마운트 시에 하는 작업들은 다음과 같은 사항이 있습니다.

- setInterval, setTimeout 을 사용하여 등록한 작업들 clear 하기 (clearInterval, clearTimeout)
- 라이브러리 인스턴스 제거

### deps가 있을때

```jsx
//UserList.js

function User({ user, onRemove, onToggle }) {
  useEffect(() => {
    console.log('user 값이 설정됨');
    console.log(user);
    return () => {
      console.log('user 가 바뀌기 전..');
      console.log(user);
    };
  }, [user]); 

}
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/64517f97-4200-43df-8753-8131bca5e260/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/64517f97-4200-43df-8753-8131bca5e260/Untitled.png)

onToggle 실행하면 위와같이 나옴,

(cleanUp이라고 함)return의 console.log 실행 후에 다시 useEffect의 console.log 실행,

deps가 있으면 (cleanUp)return 실행 후 useEffect의 console.log 실행.

useEffect 안에서 사용하는 상태나 props를 deps에 넣지 않게 된다면 useEffect에 등록한 함수가 실행 될때 최신 props, 상태를 가르키지 않게 된다.

## deps 파라미터를 생략하면

```jsx
function User({ user, onRemove, onToggle }) {
  useEffect(() => {
    console.log(user);
  });
//return도 없고 []도 없다

```

이경우 컴포넌트가 리랜더링 될 때마다 함수를 호출한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2d73cd3-6f93-4629-b356-8eee2f084ac1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2d73cd3-6f93-4629-b356-8eee2f084ac1/Untitled.png)

컴포넌트 하나하나마다 console이 찍히는걸 볼수 잇다.

### 질문

 useEffect는 어떤 상황에서 effect를 일으키고 싶을때 사용하는가? (3가지)

답 : 

컴포넌트가 마운트 될때, 언마운트 될때, 업데이트 될때