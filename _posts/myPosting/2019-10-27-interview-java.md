---
typora-root-url: ../../../moonscoding.github.io
---

## Interview For Java



## JVM

Java Virtual Machine의 약자로 자바로 구현된 바이트 코드를 멀티 플랫폼위에서 동작하게 해주는 가상 머신



- JRE
  - JVM
  - Java API



![image-20191027233916990](/assets/images/image-20191027233916990.png)



### 1. ClassLoader

JVM의 한 부분으로 동적 로드를 담당

자바는 동적 로드로 동작하는데 컴파일 타임이 아닌 런타임중 클래스를 처음으로 참조할 때 해당 클래스를 로드하고 링크



### 2. RunTime Memory

JVM 프로그램이 운영체제에게 할당받은 메모리 영역



- 스레드별 독립 공간
  - `스택영역`
  - 레지스터 ( 이중 PC 레지스터 영역 )
  - 네이티브 메서드 스택영역 
- 스레드간 공유 공간
  - `힙영역`
  - 메서드영역 ( 런타임 상수풀 + a )



#### Stack Area

스레드마다 독립적인 공간으로 생성

스택 프레임 + 지역 변수 배열 + 피연산자 스택



#### Heap Area

인스턴스를 저장하는 공간이며 스레드간 공유



### 3. Execution Engine

자바로 구현된 바이트코드가 실행 엔진에 의해서 실행 (ByteCode -> NativeCode)



#### 인터프리터 방식

바이트코드 명령어를 하나씩 읽어서 해석하고 실행 (default)



#### JIT(Just-In-Time) 방식

JIT Compiler가 적절한 시점에 바이트 코드 전체를 컴파일하여 네이티브 코드로 변경



### 4. Garbage Collection

힙 영역의 메모리 수거 프로그램

1. Mark and Sweep
2. 스케줄링 (Young Generation / Old Generation)



## OOP

Object Oriented Programming의 약자로서 기존의 절차 지향 언어에서 객체라는 필드와 메소드가 묶인 패러다임으로 프로그래밍하는 기법



### 특징

- 캡슐화 - 속성을 객체 내부에 포함시켜 데이터의 정보를 은닉할 수 있는 기법
- 상속 - 부모의 특징을 자식이 물려받아 코드 재사용성을 높이는 기법
- 다형성 - 같은 이름의 메소드를 가지고 있지만 다양한 방법으로 발현되는 기법 ( overriding, overloading, class casting )



### Class & Instance



### Static

객체를 만들지 않고 사용하며, 클래스로더가 클래스를 로딩해서 메소드 메모리에 적재할 때 할당



### Instance vs Static





### Final

Fianl 변수 > 수정할 수 없는 값

Fianl 메소드 > 오버라이딩할 수 없는 메소드

Fianl 클래스 > 상속할 수 없는 클래스



### 접근제한자 

public / default / protected / private



### Overriding vs Overloading



###Abstract

추상 클래스 > 구현할 수 없고 상속하여 구현해야 하는 클래스 ( 포유류 > 개, 고양이의 관계 )

추상 메소드 > 메소드 선언만 진행하며 구현은 하위에서 처리 ( 강제 ) 



### Interface

객체 사용 설명서 - 객체 내부를 확인하지 않고 내부가 어떤 함수로 구성되었는지를 알 수 있음



- 추상 메소드

- 정적 메소드

- 디폴트 메소드



#### Abstract vs Interface

비슷한 특징을 가지지만 활용 방식에 차이가 있음

Abstract는 상위 카테고리 즉, 내가 포함된 범주를 나타내는 의미로 활용 ( 포유류  > 개, 고양이 )

Interface는 특징을 정의하고 하위에서 정의한 특징을 발현할 것이라는 의미로 활용 ( 날개 > 조류 )



### Nested

#### Nested Class

1. 클래스멤버 혹은 2. 메소드의변수로 선언된 클래스



인스턴스멤버 클래스 > 

정적멤버 클래스 >

로컬 클래스 > 



#### Closure

선언된 위치 메소드의 매개변수와 로컬변수에 접근이 가능한 기법 (단, final 변수만 접근 가능)



#### Nested Interface

위와 같음



#### 익명클래스와 익명객체

클래스의 선언과 객체의 생성이 동시에 진행되어 단 한번만 사용하는 객체를 생성하는 기법

객체를 이 외의 곳에서 재사용하지 않을 경우 활용 (클래스가 메소드영역에 적재되지 않음 - Static영역은 애플리케이션 전체에 영향)



## Collection

자바에서 객체를 효율적으로 관리하도록 API를 제공



- Structure
  - Collection
    - List
      - ArrayList
      - LinkedList
      - Vector
    - Set
      - HashSet
      - TreeSet
      - LinkedHashSet
  - Map
    - HashMap 
    - Hashtable
      - properties 
    - TreeMap
  - Stack
  - Queue



## Socket







> 참고
>
> - [JavaGC - https://12bme.tistory.com](https://12bme.tistory.com/57)