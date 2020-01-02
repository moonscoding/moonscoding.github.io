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

React Element는 불변객체로 Element를 생성한 이후에는 해당 Element의 자식이나 속성을 변경할 수 없음

Element는 영화에서 하나의 프레임과 같이 특정 시점의 UI 제공

```
ReactDOM.render() 함수는 한 번만 호출하며, 실제 Element 객체가 변하는 것이 아닌 노드의 텍스트 영역만 변동
```



## Components & Props

components

위와 동일

컴포넌트를 쪼개는 것을 꺼리지 말고 재사용이 높다면 컴포넌트를 적극적으로 쪼개세요

React 컴포넌트는 자신의 props를 다룰 때 반드시 순수함수(외부 영향이 없는 단순 함수)처럼 동작해야 합니다.



props 

React가 사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 JSX 어트리부트를 해당 컴포넌트에 단일 객체로 전달하며 이 객체를 props라고 정의



> ES5 function
>
> 컴포넌트로 사용할 함수는 대문자를 쓰나요 ?

```react
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>
}
```



> ES6 class

```react
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```



> component render

```react
const element = <Welcome name="Heo" />;
```





## State & LifeCycle

state

state는 props와 유사하지만 비공개이며 컴포넌트에 의해 완전히 제어



props(properties) vs state

props는 컴포넌트에 전달되는 반면, state는 (함수내 선언된 변수처럼) 컴포넌트 안에서 관리



```react
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  
  render() {
    return (
    	<div>
      	<h1>Hello, World!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```



lifeCycle

1. ReactDOM.render() 전달시 Clock 컴포넌트의 constructor 호출
2. Clock 컴포넌트의 render() 호출
3. componentDidMount() 생명주기 메서드 호출, tick() 호출
4. Clock 컴포넌트는 setState()에 현재 시각을 포함하는 객체를 호출하며 UI 업데이트 진행
   setState() 호출 덕분에 state 변경을 인지하고 화면에 표시될 내용을 알아내기 위해 render() 다시 호출
5. Clock 컴포넌트가 DOM으로 부터 한 번이라도 삭제된 적이 있다면 componentWillUnmount() 생명주기 메서드 호출

```react
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  
  componentDidMount() {
    this.timerID = setInterval(
    	() => this.tick(),
      1000
    );
  }
  
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
  
  tick() {
    this.setState({
      date: new Date()
    });
  }
  
  render() {
    return (
    	<div>
      	<h1>Hello, World!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```



> 직접 state를 수정하지 마세요. setState() 메서드를 사용하세요.
> 렌더링 하지 못합니다.





## Event

- 이름 규칙으로 camelCase 사용

- JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러 전달
- 기본 동작 방지를 위해 preventDefault 명시적으로 호출 (return false; X)



> HTML vs React

```react
<!-- HTML -->
<button onClick="activateLasers()">
  Activate Lasers
</button>

<!-- React -->
<button onClick={activateLasers}>
  Activate Lasers
</button>
```



> preventDefault

```react
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```



> binding (typeA)

```react
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 콜백에서 `this`가 작동하려면 아래와 같이 바인딩 해주어야 합니다.
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <!-- bind 없이 onClick 전달했다면 this는 undefined 처리 -->
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```



> bind (typeB)

LoggingButton가 렌더링 될 때마다 다른 콜백이 생성

콜백이 하위 컴포넌트에 props로 전달 된다면 그 컴포넌트들은 추가로 다시 렌더링을 수행할 수도 있음

따라서 typeA 방식을 권장

```react
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 이 문법은 `this`가 handleClick 내에서 바인딩되도록 합니다.
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```



> parameter

```react
<!-- Lambda --> 
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>

<!-- prototype bind -->
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```





## Conditional Rendering



> 논리연산자 & 엘리먼트

```react
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```



> IF-ELSE (typeA)

```react
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```



> IF-ELSE (typeB)

```react
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```



## List & Key



> 엘리먼트 배열

```react
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```



> 엘리먼트 key

- key는 React가 어떤 항목을 변경, 추가 또는 삭제할지 식별할 때 사용

- key를 선택하는 가장 좋은 방법은 리스트를 다른 항목들 사이에서 해당 항목을 고유하게 식별할 수 있는 문자열 사용 
  (데이터 ID)
  (데이터 ID가 없다면 최후의 수단으로 INDEX를 사용합니다, 하지만 항목 순서가 바뀔 수 있는 경우 권장 X)
- key 주변 배열 context에서만 의미가 있습니다.
- key는 형제 사이에서만 고유한 값이여야 합니다.
- 컴포넌트에서 key를 사용하진 않습니다. 만약 key와 같은 value가 필요하다면 prop에 재정의합니다.



다음 코드에서 <li> 태크에 key가 없다면 key를 넣어야 한다는 경고를 발생

key는 엘리먼트 리스트르 만들 때 포함해야 하는 특수한 문자열 어트리뷰트

```react
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```



> Key로 컴포넌트 추출

키 주변 배열 context에서만 의미가 있습니다.

```react
function ListItem(props) {
  const value = props.value;
  return (
    // 틀렸습니다! 여기에는 key를 지정할 필요가 없습니다.
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 틀렸습니다! 여기에 key를 지정해야 합니다.
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```



## Form

https://ko.reactjs.org/docs/forms.html



> 제어컴포넌트(controlled components)

```react

```



## State 끌어올리기

동일한 데이터의 변경사항을 여러 컴포넌트에 반영



> State 끌어올리기 예제

예제가 너무 어려운데 ..



즉, 하위에 같은 state(진리의원천) 를 사용해야 하면 state 값을 부모로 올려서 받아 사용하는 개념

하지만 자식 컴포넌트는 부모 컴포넌트의 state를 받을 수 없기 때문에 이벤트에 따라 부모의 state를 함수를 통해 호출

(자식 컴포넌트가 state를 호출할 수 있도록 prop에 호출 함수를 미리 가지고 있음)

```react
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}

class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />

        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />

        <BoilingVerdict
          celsius={parseFloat(celsius)} />

      </div>
    );
  }
}
```





## 합성 (vs 상속)

- React는 강력한 합성 모델을 가지고 있으며, 상속 대신 합성을 사용하여 컴포넌트 간에 코드를 재사용하는 것이 좋습니다.
- props.children (의미를 가진 변수)



> 자식을 중첩하여 전달하기

props로 JSX를 전달하며, 자식이 한 개일 경우에는 태크 사이에 입력하는 것(props.children)으로 전달이 가능

```react
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <!-- children Start -->
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
      <!-- children End -->
    </FancyBorder>
  );
}
```



> 자식을 2개 이상 전달하기

props로 JSX를 전달

```react
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane left={<Contacts />} right={<Chat />} />
  );
}
```



> 특수화 (state를 전달하는 합성)

```react
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        
        <!-- children - state 전달가능 -->
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
        <!-- children - state 전달가능 -->
        
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```





## React식으로 생각하기

https://ko.reactjs.org/docs/thinking-in-react.html



1. Component 구성도 그리기

2. 데이터의 역할 구분하기 State vs Props (Context)

   State 찾기 (진리의 원천)

   1. 부모의 prop에서 가져올 수 없고
   2. 시간이 지남에도 변하지 않고
   3. 다른 데이터를 가지고 파생할 수 없고

3. State 위치 찾기

4. 역방향 데이터 흐름 추가하기 (State 끌어올리기)



## Context

- Context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공
- Context를 사용하면 컴포넌트 재사용이 어려워 집니다.
- Context보다 컴포넌트 합성이 더 간단한 경우가 있음



> Context 활용

```react
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Provider를 이용해 하위 트리에 테마 값을 보내줍니다.
    // 아무리 깊숙히 있어도, 모든 컴포넌트가 이 값을 읽을 수 있습니다.
    // 아래 예시에서는 dark를 현재 선택된 테마 값으로 보내고 있습니다.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // 현재 선택된 테마 값을 읽기 위해 contextType을 지정합니다.
  // React는 가장 가까이 있는 테마 Provider를 찾아 그 값을 사용할 것입니다.
  // 이 예시에서 현재 선택된 테마는 dark입니다.
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

