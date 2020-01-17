## Hook

v16.8



- https://ko.reactjs.org/docs/hooks-overview.html

- https://velog.io/@velopert/react-hooks



> Hook은 `함수 컴포넌트에서 React state와 lifecycle 기능을 연동(hook into)` 할 수 있게 해주는 함수
>
> Hook은 class 안에서 동작하지 않고, class 없이 React를 사용할수 있게 해줌
>
> (새로 작성하는 컴포넌트 부터 Hook을 이용하는 것을 권장)




### 특징

- 구현방식
  - Hook을 이용하여 Class 작성 없이 상태값과 여러 React의 기능을 사용
- 선택적사용
  - 기존 코드를 다시 작없 없이 Hook이 필요하다면 사용가능
  - 이것은 독립적인 테스트와 재사용이 가능하며 Hook은 계층 변화 없이 상태 관련 로직을 재사용할 수 있음
- 로직추상화
  - Hook을 사용하면 컴포넌트로부터 상태 관련 로직을 추상화
- 호환성
  - Hook 호환성을 깨뜨리는 변화가 없음



### State Hook



버튼을 클릭하면 값이 증가하는 간단한 카운터 예시

useState는 인자로 초기 state값을 하나 받습니다. (카운터는 0부터 시작함으로 위 예시에서는 초기값으로 0 설정)

setState Hook의 state는 객체일 필요가 없습니다. (초기값은 첫 번째 렌더링에서만 딱 한번 사용)



#### useState

useState Hook을 이해해보자면, 이 함수의 파라미터에는 초기값을 넣어 주며 배열을 반환

반환하는 배열을 첫 번째 인자는 상태 값이며, 두 번째 인자는 상태를 설정하는 함수

```react
import React, { useState } from 'react';

function Example() {
  // "count"라는 새로운 상태 값을 정의합니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```



이것을 `클래스` 로 어떻게 구현했을까?

```react
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```



#### useState (Multiple)

하나의 컴포넌트 내에서 State Hook을 여러 개 사용

`배열 구조 분해 (destructuring)` 문법은 useState로 호출된 state 변수들을 다른 변수명으로 할당

```react
function ExampleWithManyStates() {
  // 상태 변수를 여러 개 선언했습니다!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```





### Effect Hook

React 컴포넌트 안에서 데이터를 가져오거나 구독하고, DOM을 직접 조작하는 작업을 Side Effect (effect)라고 합니다.

이 부분은 다른 컴포넌트에 영향을 줄 수 있고, 렌더링 과정에서 구현할 수 없는 작업입니다.



#### useEffect

`useEffect`를 사용하면 React는 DOM을 바꾼 뒤에 `effect` 함수를 실행할 것입니다.

(componentDidMount + componentDidUpdate)

Effects는 컴포넌트 안에 선언되어 props와 state에 접근할 수 있음

```react
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // componentDidMount, componentDidUpdate와 비슷합니다
  useEffect(() => {
    // 브라우저 API를 이용해 문서의 타이틀을 업데이트합니다
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```



#### useEffect (Once)

두 번째 인자로 빈 배열을 삽입

```react
useEffect(() => {
  console.log('마운트 될 때만 실행됩니다.');
}, []);
```



#### useEffect (validate)

두 번째 인자로 배열을 전달하고 배열 안에 검사하고 싶은 값을 넣어줌

```react
useEffect(() => {
	console.log(name);
}, [name]);
```



#### useEffect (unmount & update)

만약, unmount 되기 전이나 update 되기 직전에 어떠한 작업을 수행하고 싶다면

뒷정리 (cleanup) 함수를 반환

```react
useEffect(() => {
  console.log('effect');
  console.log(name);
  return () => {
    console.log('cleanup');
    console.log(name);
  };
});
```



#### useEffect (unmount)

```react
useEffect(() => {
  console.log('effect');
  console.log(name);
  return () => {
    console.log('cleanup');
    console.log(name);
  };
}, []);
```



> 기본적으로 React는 매 렌더링 이후에 effect를 실행 **(생명주기함수와 다른점은?)**



### Context Hook

Hook을 사용하면 함수형 컴포넌트에서 Context를 보다 쉽게 사용 가능



#### useContext

```react
import React, { createContext, useContext } from 'react';

const ThemeContext = createContext('black');
const ContextSample = () => {
  const theme = useContext(ThemeContext);
  const style = {
    width: '24px',
    height: '24px',
    background: theme
  };
  return <div style={style} />;
};

export default ContextSample;
```



### Reducer Hook

useState보다 컴포넌트에서 더 다양한 상황에 따라 다양한 상태를 다른 값으로 업데이트 해주고 싶을 때 사용하는 Hook



Reducer라는 개념은 Redux에서 학습

Reducer는 현재 상태와 업데이트를 위해 필요한 정보를 담은 액션값을 전달 받아 새로운 상태를 반환하는 함수

Reducer 함수에서 새로운 상태를 만들 때는 꼭 불편성을 지켜주어야..



#### useReducer





### 규칙

Hook은 단지 JavaScript 함수지만 다음 규칙을 따라야 합니다.

- 최상위에서만 Hook을 호출해야 합니다. 반복, 조건, 중첩된 함수내에서 Hook을 실행하지 마세요.
- React 함수 컴포넌트 내에서 Hook을 호출. 일반 JavaScript 함수에서는 호출하지 마세요.
  - (단, 직접 작성한 Custom Hook 내에서는 호출이 가능)



### 사용자정의 Hook (Custom Hook)

기존에는 컴포넌트 재사용을 위해 `HOC, render props를 대게 사용

Custom Hook은 이들 둘과는 달리 컴포넌트 트리에 새 컴포넌트를 추가하지 않고도 이것을 가능하게 처리



```react 
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

