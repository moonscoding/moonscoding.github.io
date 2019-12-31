# React 가이드

자습서

https://ko.reactjs.org/tutorial/tutorial.html



개념서

https://ko.reactjs.org/docs/hello-world.html



## JSX

별도의 파일에 마크업(HTML)과 로직(JavaScript)를 넣어 기술을 인위적으로 분리

둘 다 포함하는 `컴포넌트` 라고 부르는 느슨하게 연결된 유닛으로 관심사를 분리

필수요소는 아니지만 로직(JavaScript) 안에서 UI관련 작업을 할 떄 시각적으로 더 도움이 된다고 생각



```react
const name = 'Heo';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
	element,
  docuemnt.getElementById('root');
);
```



### JSX 객체 & 로직

JSX도 하나의 표현식

컴파일이 끝나면 JSX 표현식이 정규 JavaScript 함수 호출이 되고 JavaScript 객체로 인식

따라서 if, for와 같은 로직을 사용할 수 있음



객체기 때문에 다음과 같은 속성을 가질 수 있음

```react
const element = <img src={user.name}></img>;
```



아래 두개의 객체는 동일 ( React.createELement() 함수 사용 )

```react
const element = (
	<h1 className="greeting">
  	hello, world!
  </h1>
);

const elementB = React.createElement(
	'h1',
  { className : 'greating' },
	'hello, world!'
);
```



## Element Rendering







## Components and Props



## State & LifeCycle



## Event



## Conditional Rendering



## List & Key



## Form



## State 끌올



## 합성 vs 상속



## React식으로 생각하기



