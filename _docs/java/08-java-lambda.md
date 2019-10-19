---

title: "Java Lambda"
permalink: /docs/lambda/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"

---



## Lambda

함수적 프로그래밍 > Parallel + EventDriven

람다식 > 익명함수 생성식

- 람다식 > 매개변수를 가진 코드블록 > 익명구현객체



> 익명구현객체

```java
Runnable runnable = new Runnable() {
  public void run() { ... }
}
```

> 람다식

```java
Runnable runnable = () -> { ... };
```



### Method References

메소드 참조 > 메소드를 참조해서 매개변수의 정보 및 리턴 타입을 알아내어 람다식에서 불필요한 매개 변수를 제거



[1] 메소드 참조를 사용하지 않을 경우

```java
(a, b) -> Math.max(a, b);
```



[2] 메소드 참조

static method & instance method 모두 가능

```java
Math :: max
```

```
obj :: instanceMethod
```



[3] 생성자 참조

```
Class :: new
```





## Functional Interface

함수적인터페이스 > 하나의 추상 메소드가 선언된 인터페이스만 람다식의 타겟 타입



### @FunctionalInterface 

두 개 이상의 추상 메소드가 선언되지 않았는지 확인하는 어노테이션 ( 필수요소아님 )

```java
@FunctionalInterface
public interface MyFunctionalInterface  {
  public void method();
  public void otherMethod();
}
```

```
CompileException!
```



## Closure

람다식 실행 블록에 `[1] 클래스의 멤버(필드와 메소드)`및 `[2] 로컬 변수`를 사용할 수 있음



### [1] Instance Field

람다식 내부에서 this는 중첩객체 Inner Class의 this

final 키워드 없이 자유롭게 접근 가능

```java
public class Outter {
  public int outterField = 10;
  
  class Inner {
    int innerField = 20;
    
    void method() {
      MyFunctionalInterface fi = () -> {
        System.out.println("outterField : " + outterField);
        System.out.println("outterField : " + Outter.this.outterField);
        
        System.out.println("innerField : " + innerField);
        System.out.println("innerField : " + this.innerField);
      };
      fi.method();
    }
  }
}

public class Main {
  public static void main(String[] args) {
    Outter outter = new Outter();
    Outter.Inner inner = outter.new Inner();
    inner.method();
  }
}
```

```
outterField : 10
outterField : 10
innerField : 20
innerField : 20
```



### [2] Local Field

익명객체 내부에서 메소드의 매개변수 OR 로컬변수는 final 특성을 가져야 사용 가능 ( Java8 부터는 final 키워드 생략 가능 )

```java
public class LocalVariable {
  void method(int argumentNum/* final */) {
    int localNum = 10; /* final */ 
    
    MyFunctionalInterface fi = () -> {
      System.out.println("argumentNum : " + argumentNum);
      System.out.println("localNum : " + localNum);
    };
    fi.method();
  }
}

public class Main {
  public static void main(String[] args) {
    LocalVariable localVariable = new LocalVariable();
    localVariable.method(20);
  }
}
```

```
argumentNum : 20
localNum : 10
```



## Strandard Functional

표준 API의 함수적 인터페이스



### [1] Consumer (accept)

| param | return |
| ----- | ------ |
| O     | X      |



### [2] Supplier (get)

| param | return |
| ----- | ------ |
| O     | X      |

- `get()`, `getAsBoolean()`, `getAsDouble()`, `getAsInt()`, `getAsLong()`



### [3] Function (apply)

| param | return |
| ----- | ------ |
| O     | O      |

주로 매개값을 리턴값으로 매핑( 타입변환 )

- `apply()`,  `applyAsInt()`, `applyAsLong()`, `applyAsDouble()`



### [4] Operator (apply)

| param | return |
| ----- | ------ |
| O     | O      |

주로 매개값 연산하고 결과 반환

- Function, BiFunction

- `apply()`,  `applyAsInt()`, `applyAsLong()`, `applyAsDouble()`



#### 최대최소 minBy() & maxBy()

BinaryOperator 함수적 인터페이스는 minBy(), maxBy() 정적 메소드를 가짐

compare() 반환값으로 첫 번째가 작으면 음수, 같으면 0, 첫 번째가 크면 양수 반환



Comparator

```java
@FunctionalInterface
public interface Comparator<T> {
  public int compare(T o1, T o2);
}
```



minBy

```java
BinaryOperator<Integer> bo = BinaryOperator.minBy((numA, numB) -> Integer.compare(numA, numB));
int min = bo.apply(10, 20);
System.out.println(min);
```

```
10
```



### [5] Predicate (test)

| param | return |
| ----- | ------ |
| O     | O(boolean)      |

매개값 조사후에 결과에 따라 boolean(true/false) 반환



#### 논리연산 and(), or(), negate()

Predicate 종류의 함수적 인터페이스는 and(), or(), negate() 디폴트 메소드를 가짐

```java
PredicateAB predicateAB = predicateA.and(predicateB);
boolean result = predicateAB.test(num);
```



#### 비교 isEqual()

정적메소드 isEqual()

```java
Predicate<Object> predicate = Predicate.isEqual(targetObject);
boolean result = predicate.test(sourceObject);
```





### 순차처리 andThen() & compose()

`Consumer, Function, Operator` 종류의 표준 함수 인터페이스는 andThen() & compose() 디폴트 메소드를 가짐

두 개의 함수적 인터페이스를 순차적으로 연결하고, 첫 번째 처리 결과를 두 번째 매개값으로 제공



[1] andThen()

interfaceA > interfaceB

```java
InterfaceAB interfaceAB = interfaceA.andThen(interfaceB);
Object result = interfaceAb.method(); 
```



[2] compose()

interfaceB > interfaceA

```java
InterfaceAB interfaceAB = interfaceA.compose(interfaceB);
Object result = interfaceAb.method(); 
```

