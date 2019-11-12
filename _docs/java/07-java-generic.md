---

title: "Java Generic"
permalink: /docs/generic/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"

---



## Generic

- 컴파일 시 강한 타입 체크
- 타입 변환 제거





## WildCard







## Inheritance





## Casting

>  List<T>에 대한 Input & Output Parameter Casting

- `자료구조`간에 형변환은 자유롭지 않음

```java
public interface Parent { ... }

public class Child implements Parent { ... }

public class Main {
    public static void main(String[] args) {}
        Child c1 = new Child();
        List<Child> c2 = new ArrayList<>();
        
        inputGenericType(c1); 
        inputListGenericType(c2); // CompileError!
    }
    
    public void inputGenericType(Parent in) { ... }
    
    public void inputListGenericType(List<Parent> in) { ... }
}
```

