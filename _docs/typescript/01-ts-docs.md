

# TypeScript

docs

- 자료형 및 강력한 객체지향 아키텍처를 지원



> tsconfig.json

compile config 내용을 작성하는 json 파일



## Type Declaration

변수 타입선언

```typescript
let foo: string = 'hello';
let bar: number = true; // error
```



매개변수 타입선언

```typescript
function multiplyA(x: number, y: number): number {
  return x * y;
}
```

```typescript
const multiplyB = (x: number, y: number): number => x * y;
```



### Type

| Type      |  JS  |  TS  | Description                                                  |
| :-------- | :--: | :--: | :----------------------------------------------------------- |
| boolean   |  ◯   |  ◯   | true와 false                                                 |
| null      |  ◯   |  ◯   | 값이 없다는 것을 명시                                        |
| undefined |  ◯   |  ◯   | 값을 할당하지 않은 변수의 초기값                             |
| number    |  ◯   |  ◯   | 숫자(정수와 실수, Infinity, NaN)                             |
| string    |  ◯   |  ◯   | 문자열                                                       |
| symbol    |  ◯   |  ◯   | 고유하고 수정 불가능한 데이터 타입이며 주로 객체 프로퍼티들의 식별자로 사용 <br>(ES6에서 추가) |
| object    |  ◯   |  ◯   | 객체형(참조형)                                               |
| array     |      |  ◯   | 배열                                                         |
| tuple     |      |  ◯   | 고정된 요소수 만큼의 타입을 미리 선언후 배열을 표현          |
| enum      |      |  ◯   | 열거형. 숫자값 집합에 이름을 지정한 것이다.                  |
| any       |      |  ◯   | 타입 추론(type inference)할 수 없거나 타입 체크가 필요없는 변수에 사용. <br> var 키워드로 선언한 변수와 같이 어떤 타입의 값이라도 할당 가능. |
| void      |      |  ◯   | 일반적으로 함수에서 반환값이 없을 경우 사용한다.             |
| never     |      |  ◯   | 결코 발생하지 않는 값                                        |



#### string

```typescript
let color: string = "blue";
color = 'red';
let myName: string = `Lee`; // ES6 템플릿 문자열
let greeting: string = `Hello, my name is ${ myName }.`; // ES6 템플릿 대입문
```



>  타입은 소문자(원시타입) & 대문자(객체타입)를 구분하기 때문에 주위가 필요 

```typescript
let primiteveStr: string = 'hello'; // OK
primiteveStr = new String('hello'); // Error - 원시 타입 문자열 타입에 객체를 할당하였다.

let objectStr: String = 'hello'; // OK
objectStr = new String('hello'); // OK
```



#### object

```typescript
const obj: object = {};

// Date 타입
const today: Date = new Date();

// HTMLElement 타입
const elem: HTMLElement = document.getElementById('myId');

// Person 타입
class Person { }
const person: Person = new Person();
```



#### array

```typescript
let list1: any[] = [1, 'two', true];
let list2: number[] = [1, 2, 3];
let list3: Array<number> = [1, 2, 3]; // 제네릭 배열 타입
```



#### tuple

배열을 자리를 고정시켜 안정성을 강화한 배열

```typescript
let tuple: [string, number];
tuple = ['hello', 10]; // OK
tuple = [10, 'hello']; // Error
tuple = ['hello', 10, 'world', 100]; // Error
tuple.push(true); // Error
```



#### enum

인덱스 체계를 알고 있어야 할 듯

```typescript
enum Color1 {Red, Green, Blue};
let c1: Color1 = Color1.Green;
console.log(c1); // 1

enum Color2 {Red = 1, Green, Blue};
let c2: Color2 = Color2.Green;
console.log(c2); // 2

enum Color3 {Red = 1, Green = 2, Blue = 4};
let c3: Color3 = Color3.Blue;
console.log(c3); // 4
```



#### any

```typescript
let notSure: any = 4;
notSure = 'maybe a string instead';
notSure = false; // okay, definitely a boolean
```



#### void

```typescript
function warnUser(): void {
  console.log("This is my warning message");
}
```



#### never

```typescript
function infiniteLoop(): never {
  while (true) {}
}

function error(message: string): never {
  throw new Error(message);
}
```



### Static Typing (vs Dynamic Typing)

정적 타이핑(Static Typing)은 한번 명시적으로 선언한 타입을 바꿀 수 없는 방식

동적 타이핑(Dynamic Typing)은 느슨한 타입으로 타입 선언 없이 값이 할당되어 동적으로 타입을 추론(Type Inference)

js는 동적 타이핑 언어이나 ts는 정적 타이핑 언어입니다.



- javascript

```javascript
var foo;

console.log(typeof foo);  // undefined

foo = null;
console.log(typeof foo);  // object
```



- typescript

```typescript
let foo: string,   // 문자열 타입
    bar: number,   // 숫자 타입
    baz: boolean;  // 불리언 타입

foo = 'Hello';
bar = 123;
baz = 'true'; // error: Type '"true"' is not assignable to type 'boolean'.
```



> 정적 타이팅의 장점은 **코드 가독성, 예측성, 안정성의 향상**이라고 볼 수 있는데 이는 대규모 프로젝트에 매우 적합하다.



### Type Inference 

타입 선언을 생략하면 값이 할당되는 과정에서 동적으로 타입이 결정되며 이를 타입 추론(Type Inference)이라 함

typescript는 동적 타이핑 언어이기 때문에 타입 추론을 하지 않고 타입을 명시적으로 나타냄

만약 타입을 생략하면 `any` 타입으로 설정되며 이는 typescript에서 권장하진 않음



## Class



### Properties

- ES6 Class

ES6 클래스는 몸체에 메소드만을 담을 수 있음

클래스 몸체에 프로퍼티를 선언할 수 없고 반드시 생성자 내부에서 클래스 프로퍼티를 선언하고 초기화

```javascript
class Person {
  constructor(name) {
    // 클래스 프로퍼티의 선언과 초기화
    this.name = name;
  }

  walk() {
    console.log(`${this.name} is walking.`);
  }
}
```



- typescript

```typescript
class Person {
  // 클래스 프로퍼티를 사전 선언하여야 한다
  name: string;

  constructor(name: string) {
    // 클래스 프로퍼티수에 값을 할당
    this.name = name;
  }

  walk() {
    console.log(`${this.name} is walking.`);
  }
}

const person = new Person('Lee');
person.walk(); // Lee is walking
```



### Access Modifier

Typescript 클래스는 클래스 기반 객체 지향 언어가 지원하는 접근 제한자(Access Modifier) 

public, private, protected를 지원하며 의미 또한 기본적으로 동일

| 접근 가능성      | public | protected | private |
| :--------------- | :----: | :-------: | :-----: |
| 클래스 내부      |   ◯    |     ◯     |    ◯    |
| 자식 클래스 내부 |   ◯    |     ◯     |    ✕    |
| 클래스 인스턴스  |   ◯    |     ✕     |    ✕    |



### Constructor Access Modifier

접근 제한자는 생성자 파라미터에도 선언이 가능



>  생성자 파라미터에 접근 제한자 있을 때

 이때 접근 제한자가 사용된 생성자 파라미터는 암묵적으로 클래스 프로퍼티로 선언되고 

생성자 내부에서 별도의 초기화 없이 암묵적으로 초기화가 수행

```typescript
class Foo {
  /*
  접근 제한자가 선언된 생성자 파라미터 x는 클래스 프로퍼티로 선언되고 지동으로 초기화된다.
  public이 선언되었으므로 x는 클래스 외부에서도 참조가 가능하다.
  */
  constructor(public x: string, private y: string) { }
}

const foo = new Foo('Hello', 'World');
console.log(foo);   // Foo { x: 'Hello', y: 'World' }
console.log(foo.x); // Hello
console.log(foo.y); // private이 선언된 bar.x는 클래스 내부에서만 참조 가능하다
```



>  생성자 파라미터에 접근 제한자 없을 때

만일 생성자 파라미터에 접근 제한자를 선언하지 않으면 

생성자 파라미터는 생성자 내부에서만 유효한 지역 변수가 되어 생성자 외부에서 참조가 불가능하게 된다.

```typescript
class Foo {
  // x는 생성자 내부에서만 유효한 지역 변수이다.
  constructor(x: string) {
    console.log(x);
  }
}

const foo = new Foo('Hello');
console.log(foo); // Foo {}
```



### readonly

readonly가 선언된 클래스 `프로퍼티는 선언 시 또는 생성자 내부에서만 값을 할당`

그 외의 경우에는 값을 할당할 수 없고 오직 읽기만 가능한 상태가 된다. 

이를 이용하여 상수의 선언에 사용한다. (전역적인 상수라 할 순 없고 클래스 내부의 상수)



```typescript
class Foo {
  private readonly MAX_LEN: number = 5;
  private readonly MSG: string;

  constructor() {
    this.MSG = 'hello';
  }

  log() {
    // readonly가 선언된 프로퍼티는 재할당이 금지된다.
    this.MAX_LEN = 10; // Cannot assign to 'MAX_LEN' because it is a constant or a read-only property.
    this.MSG = 'Hi'; // Cannot assign to 'MSG' because it is a constant or a read-only property.

    console.log(`MAX_LEN: ${this.MAX_LEN}`); // MAX_LEN: 5
    console.log(`MSG: ${this.MSG}`); // MSG: hello
  }
}

new Foo().log();

```



### static

ES6 클래스에서 static 키워드는 클래스의 정적 메소드를 정의

정적 메소드는 클래스의 인스턴스가 아닌 클래스 이름으로 호출



>  typescript에선 클래스 프로퍼티에서 static 사용이 가능

```typescript
class Foo {
  // 생성된 인스턴스의 갯수
  static instanceCounter = 0;
  constructor() {
    // 생성자가 호출될 때마다 카운터를 1씩 증가시킨다.
    Foo.instanceCounter++;
  }
}

var foo1 = new Foo();
var foo2 = new Foo();

console.log(Foo.instanceCounter);  // 2
console.log(foo2.instanceCounter);
```



### abstract

추상 클래스(abstract class)는 하나 이상의 추상 메소드를 포함하며 일반 메소드도 포함이 가능

추상 메소드는 내용이 없이 메소드 이름과 타입만이 선언된 메소드를 말하고 선언할 때 abstract 키워드 사용



```typescript
abstract class Animal {
  // 추상 메소드
  abstract makeSound(): void;
  // 일반 메소드
  move(): void {
    console.log('roaming the earth...');
  }
}

// 직접 인스턴스를 생성할 수 없다.
// new Animal();
// error TS2511: Cannot create an instance of the abstract class 'Animal'.

class Dog extends Animal {
  // 추상 클래스를 상속한 클래스는 추상 클래스의 추상 메소드를 반드시 구현하여야 한다
  makeSound() {
    console.log('bowwow~~');
  }
}

const myDog = new Dog();
myDog.makeSound();
myDog.move();
```





## Interface

인터페이스는 개발 단계에서 도움을 주기 위해 제공되는 기능으로 자바스크립트의 표준이 아니다. 

따라서TypeScript 파일을 자바스크립트 파일로 트랜스파일링하면 인터페이스가 삭제된다.



### Object & Interface 

```typescript
// 인터페이스의 정의
interface Todo {
  id: number;
  content: string;
  completed: boolean;
}

// 변수 todo의 타입으로 Todo 인터페이스를 선언하였다.
let todo: Todo;

// 변수 todo는 Todo 인터페이스를 준수하여야 한다.
todo = { id: 1, content: 'typescript', completed: false };
```



### Array & Interface

```typescript
// 인터페이스의 정의
interface Todo {
  id: number;
  content: string;
  completed: boolean;
}

let todos: Todo[] = [];

// 파라미터 todo의 타입으로 Todo 인터페이스를 선언하였다.
function addTodo(todo: Todo) {
  todos = [...todos, todo];
}

// 파라미터 todo는 Todo 인터페이스를 준수하여야 한다.
const newTodo: Todo = { id: 1, content: 'typescript', completed: false };
addTodo(newTodo);
console.log(todos)
// [ { id: 1, content: 'typescript', completed: false } ]
```



### Function & Interface

인터페이스를 함수의 타입으로 사용 가능

```typescript
// 함수 인터페이스의 정의
interface SquareFunc {
  (num: number): number;
}

// 함수 인테페이스를 구현하는 함수는 인터페이스를 준수하여야 한다.
const squareFunc: SquareFunc = function (num: number) {
  return num * num;
}

console.log(squareFunc(10)); // 100
```



### Class & Interface



> typeA (properties)

```typescript
// 인터페이스의 정의
interface ITodo {
  id: number;
  content: string;
  completed: boolean;
}

// Todo 클래스는 ITodo 인터페이스를 구현하여야 한다.
class Todo implements ITodo {
  constructor (
    public id: number,
    public content: string,
    public completed: boolean
  ) {}
}

const todo = new Todo(1, 'Typescript', false);

console.log(todo);
```



> typeB (properties + method)

```typescript
// 인터페이스의 정의
interface IPerson {
  name: string;
  sayHello(): void;
}

/*
인터페이스를 구현하는 클래스는 인터페이스에서 정의한 프로퍼티와 추상 메소드를 반드시 구현하여야 한다.
*/
class Person implements IPerson {
  // 인터페이스에서 정의한 프로퍼티의 구현
  constructor(public name: string) {}

  // 인터페이스에서 정의한 추상 메소드의 구현
  sayHello() {
    console.log(`Hello ${this.name}`);
  }
}

function greeter(person: IPerson): void {
  person.sayHello();
}

const me = new Person('Lee');
greeter(me); // Hello Lee
```



### DuckTyping

주의할 것은 인터페이스 구현이 타입 체크 통과의 유일한 방법은 아니라는 점

타입 체크에서 중요한 것은 값을 실제로 가지고 있다는 것



> Typescript는 해당 인터페이스에서 정의한 프로퍼티나 메소드를 가지고 있다면, 그 인터페이스를 구현한 것으로 인정
>
> 이것을 덕 타이핑(Duck Typing) 또는 구조적 타이핑(Structural Typing)이라 명명

```typescript
interface IDuck { // 1
  quack(): void;
}

class MallardDuck implements IDuck { // 3
  quack() {
    console.log('Quack!');
  }
}

class RedheadDuck { // 4
  quack() {
    console.log('q~uack!');
  }
}

function makeNoise(duck: IDuck): void { // 2
  duck.quack();
}

makeNoise(new MallardDuck()); // Quack!
makeNoise(new RedheadDuck()); // q~uack! // 5
```



### 선택적 프로퍼티

인터페이스 프로퍼티는 반드시 구현되어야 하나 인터페이스의 프로퍼티가 선택적으로 필요한 경우가 있을 수 있음

이 때 선택적 프로퍼티(Optional Property)를는 프로퍼티명 뒤에 `?`를 붙여 생략하여도 에러가 발생하지 않음



```typescript
interface UserInfo {
  username: string;
  password: string;
  age?    : number;
  address?: string;
}

const userInfo: UserInfo = {
  username: 'ungmo2@gmail.com',
  password: '123456'
}

console.log(userInfo);
```



## Generic

제네릭은 선언 시점이 아닌 생성 시점에 타입을 명시하여 하나의 타입만이 아닌 다양한 타입을 사용할 수 있도록 하는 기법

한 번은 선언으로 다양한 타입 재사용이 가능하다는 장점

(T는 제네릭을 선언할 때 관용적으로 사용되는 식별자로 타입 파라미터)



### Class & Generic

```typescript
class Queue<T> {
  protected data: Array<T> = [];
  push(item: T) {
    this.data.push(item);
  }
  pop(): T {
    return this.data.shift();
  }
}
```



### Function & Generic

함수에 전달되는 타입에 따라 매개변수 타입이 달라집니다.

만약 {name: string} 타입의 요소를 갖는 배열을 전달받으면 타입 매개변수는 {name: string}가 된다.

```typescript
function reverse<T>(items: T[]): T[] {
  return items.reverse();
}
```



## Mapped types

https://www.typescriptlang.org/docs/handbook/advanced-types.html

```typescript
interface PersonPartial {
    name?: string;
    age?: number;
}

interface PersonReadonly {
    readonly name: string;
    readonly age: number;
}
```



This happens often enough in JavaScript that `TypeScript provides a way to create new types based on old types` — **mapped types**. In a mapped type, the new type transforms each property in the old type in the same way. For example, you can make all properties of a type `readonly` or `optional`. Here are a couple of examples:

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}
type ReadonlyPerson = Readonly<Person>;

type Partial<T> = {
    [P in keyof T]?: T[P];
}
type PersonPartial = Partial<Person>;
```





## Utility Type

https://www.typescriptlang.org/docs/handbook/utility-types.html

https://rinae.dev/posts/helper-types-in-typescript (helper type)



### Pick<T, K>

Constructs a type by picking the set of properties `K` from `T`.

T의 모든 속성중 K에 포함된 것만 모아 구성합니다.

```typescript
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Pick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
```



### Omit <T, K>

Constructs a type by picking all properties from `T` and then removing `K`.

T에서 모든 속성을 선택한 다음 K를 제거하여 형식을 구성합니다.

```typescript
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Omit<Todo, 'description'>;

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
```



### Exclude<T, U>

Constructs a type by excluding from `T` all properties that are assignable to `U`.

- `(() => void)` == `Function`

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">;  // "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;  // "c"
type T2 = Exclude<string | number | (() => void), Function>;  // string | number
```





## 기타 API



### type





### keyof

keyof and Lookup Types

JavaScript에서는 속성 이름을 매개 변수로 사용하는 API를 사용하는 것이 일반적이지만 

지금까지 이러한 API에서 발생하는 유형 관계를 표현할 수 없었습니다.

색인 유형 조회 또는 keyof를 입력하십시오. 

T의 색인 유형 쿼리 키는 T에 허용되는 특성 이름 유형을 생성합니다. T 유형의 키 유형은 문자열의 하위 유형으로 간주됩니다.

```typescript
interface Person {
    name: string;
    age: number;
    location: string;
}

type K1 = keyof Person; // "name" | "age" | "location"
type K2 = keyof Person[];  // "length" | "push" | "pop" | "concat" | ...
type K3 = keyof { [x: string]: Person };  // string
```





### declare global

모듈 내부에서 전역 스코프로 선언할 때 사용합니다. 

```typescript
declare global {
    interface Array<T> {
        toObservable(): Observable<T>;
    }
}
```

