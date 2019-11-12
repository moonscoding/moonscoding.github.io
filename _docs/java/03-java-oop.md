---

title: "Java OOP"
permalink: /docs/oop/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"

---



## Class

- class
- object(instance)
- field
- construcrtor
- method
- inheritance
- polymorphism



### FIeld



### Constructor



#### this

같은 클래스내에 다른 생성자 호출 

Default Constructor 설정에 유리

```java
public class Company {
  
  String name;
  int count;
  Category category;
  
  Company(String name) {
    this(name, 10, Category.IT);
  }
  
  Company(String name, int count) {
    this(name, count, Category.IT);
  }
  
  Company(String name, int count, Category category) {
    this(name, count, category);
  }
  
}
```





### Method



> [Method - Guild1] 적시에 방어적 복사본

클라이언트가 여러분의 불벽식을 깨뜨리려 혈안이 되어 있다고 가정하고 방어적인 프로그래밍



#### Overloading vs Override

- Overloading
  - `메소드이름과 파라미터`에 따라 다른 기능으로 인식하는 기법
- Override 
  - ...



### Instance vs Static

- instance 
- static
  - static class
  - static field
  - static method
  - static block



### AccessModifier

접근제한자

- private
- protected
- default
- public



### Inheritance

상속



### Polymorphism (Casting)

다형성



## Interface



## Nested

중첩



## Anonymous

익명구현객체 왜 사용할까 ?



## Fianl

- final field
- static final field
- final method
- final class



## Abstact





