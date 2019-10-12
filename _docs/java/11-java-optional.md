---

title: "Java Optional"
permalink: /docs/optional/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"

---



## Optional

### of()

```java
Optional<Integer> optInt = Optional.of(1);
System.out.println(optInt);
```

```
Optional[1]
```

```java
Optional<Integer> optInt = Optional.of(null);
System.out.println(optInt);
```

```
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



### empty()

```java
Optional<String> optStrA = Optional.empty();
System.out.println(optStrA);
```

```
Optional.empty
```



### isPresent()

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





## Functional

### map()

- Optional\<U> map(Function\<? super T, Optional\<U>> mapper)

```java
int length = Optional.ofNullable("hello")
    .map(String::trim)
    .map(String::length)
    .orElse(0);
```



### flatMap()

- Optional\<U> flatMap(Function\<? super T, Optional\<U>> mapper) 

```java
Optional<String> optString = Optional.of("input");

System.out.println(optString.map(str->Optional.of("hello"))); // "Optional[Optional[hello]]"
System.out.println(optString.map(str->"hello")); // "Optional[hello]"

System.out.println(optString.flatMap(str->"hello"));
System.out.println(optString.flatMap(str->Optional.of("hello"))); // "Optional[hello]"
```



### filter()

```java
int length = Optional.ofNullable("hello")
    .filter(o -> o.length() > 5) // "Optional.empty"
    .map(String::length)
    .orElse(0);
```



### ifPresent()

- ifPresent(Consumer<? super T> consumer)

```java
Optional<String> optString = Optional.of("hello");
optString.ifPresent(str -> {
    System.out.println("length : " + str.length());
});
```



## Example

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

