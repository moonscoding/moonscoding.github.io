---

title: "Java Optional"
permalink: /docs/optional/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"

---



## Define

### empty()

```java
Optional<String> optStrA = Optional.empty();
System.out.println(optStrA);
```

```
Optional.empty
```



### of()

```java
Optional<Integer> optIntA = Optional.of(1);
Optional<Integer> optIntB = Optional.of(null);
System.out.println(optIntA);
System.out.println(optIntB);
```

```
Optional[1]
NullPointerException
```



### ofNullable()

```java
Optional<Integer> optIntA = Optional.ofNullable(null);
System.out.println(optIntA);
```

```
Optional.empty
```



## Call

### get() 

```java
Optional<Integer> optIntA = Optional.of(1);
Optional<Integer> optIntB = Optional.ofNullable(null);
System.out.println(optIntA.get());
System.out.println(optIntB.get()); 
```

```
1
NoSuchElementException
```



### orElse()

- orElse(T other)
  - null일 경우, default 값을 설정

```java
Optional<Integer> optIntA = Optional.of(1);
Optional<Integer> optIntB = Optional.ofNullable(null);
System.out.println(optIntA.orElse(0));
System.out.println(optIntB.orElse(0));
```

```
1
0
```



### orElseGet() 

- orElseGet(Supplier<? extends T> other)
  - null일 경우, 함수인자(Supplier)를 통해 default 값을 설정

```java
Optional<Integer> optIntA = Optional.of(1);
Optional<Integer> optIntB = Optional.ofNullable(null);

Supplier<Integer> supplier = new Supplier<Integer>() {
    @Override
    public Integer get() {
        return 0;
    }
};
System.out.println(optIntA.orElseGet(supplier));
System.out.println(optIntB.orElseGet(supplier)); 
```

```
1
0
```



### orElseThrow()

- orElseThrow(Supplier<? extends X> exceptionSupplier)
  - null일 경우, 함수인자(Supplier)를 통해 예외 반환

```java
Optional<Integer> optIntA = Optional.of(1);
Optional<Integer> optIntB = Optional.ofNullable(null);

Supplier<Exception> supplier = new Supplier<Exception>() {
    @Override
    public Exception get() {
        return new Exception("orElseThrowException");
    }
};

try {
    System.out.println(optIntA.orElseThrow(supplier));
} catch (Exception e) {
    System.out.println(e.getMessage());
}

try {
    System.out.println(optIntB.orElseThrow(supplier));
} catch (Exception e) {
    System.out.println(e.getMessage());
}
```

```
1
orElseThrowException
```



### isPresent()

- [1] ifPresent()
  - return -> boolean
- [2] ifPresent(Consumer<? super T> consumer)

```java
Optional<Integer> optIntA = Optional.of(1);
Optional<Integer> optIntB = Optional.ofNullable(null);
System.out.println(optIntA.isPresent());
System.out.println(optIntB.isPresent());
```

```
true
false
```

```java
Optional<String> optString = Optional.of("hello");
optString.ifPresent(str -> {
    System.out.println("length : " + str.length());
});
```

```
length : 5
```



## Stream

> Optional 객체는 원소가 1개인 Stream과 같이 사용 가능



### map()

- Optional\<U> map(Function\<? super T, Optional\<U>> mapper)
  - return Optional

```java
int length = Optional.ofNullable("hello")
    .map(String::trim)
    .map(String::length)
    .orElse(0);
```

```
5
```



### flatMap()

- Optional\<U> flatMap(Function\<? super T, Optional\<U>> mapper) 
  - return Optional.get()

```java
Optional<String> optString = Optional.of("hello");

// map
System.out.println(optString.map(str->str));
System.out.println(optString.map(str->Optional.of(str)));

// flatMap
System.out.println(optString.flatMap(str->str)); // * Compile Exception
System.out.println(optString.flatMap(str->Optional.of(str)));
```

```
Optional[hello]
Optional[Optional[hello]]

Optional[hello]
```



### filter()

```java
int length = Optional.ofNullable("hello")
    .filter(o -> o.length() > 5) // * Optional.empty
    .map(String::length)
    .orElse(0);
```

```
0
```





## Customizing

### getAsOptional()

- 리스트중 특정 인덱스 항목을 요청할 때

```java
public static <T> Optional<T> getAsOptional(List<T> list, int index) {
    try {
        return Optional.of(list.get(index));
    } catch (ArrayIndexOutOfBoundsException e) {
        return Optional.empty();
    }
}
```



> 참고

- https://www.daleseo.com/java8-optional-after/