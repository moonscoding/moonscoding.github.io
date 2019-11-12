---

title: "Java Stream"
permalink: /docs/stream/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"
typora-root-url: ../../../moonscoding.github.io
---



## Stream

- 람다
- 내부반복자
- 파이프라인 ( 중간처리 & 최종처리 )
- 병렬처리 ( 포크조인 )



### Structure

![image-20191011221755334](/assets/images/image-20191011221755334.png)



## Creating

### from collection

```java
List<String> list = new ArrayList<>();
Stream<String> stream = list.stream();
```



### from array

```java
String[] arr = {"soul", "music", "child"};
Stream<String> stream = Arrays.stream(arr);
```



### from range

```java
IntStream stream = IntStream.of(1,2,3,4,5);
IntStream stream = IntStream.rangeClosed(1, 100);
```



### from file

```java
// Files.lines()
Path path Paths.get("../memo.txt");
Stream<String> stream = Files.lines(path, Charset.defaultCharset());

// BufferedReader lines()
File file = path.toFile();
FileReader fr = new FileReader(file);
BufferedReader br = new BufferedReader(fr);
stream = br.lines();
```





## Filtering

### distinct()

```java
List<String> list = Arrays.asList("a", "b", "c", "c");
list.stream.distinct().forEach(a -> {
  System.out.println(a);
});
```

```
a
b
c
```



### filter()

```java
IntStream stream = IntStream.rangeClosed(1, 5);
stream.filter(a -> a % 2 == 0).forEach(a -> {
  System.out.println(a);
});
```

```
2
4
```



## Mapping

### map()

- `map()`, `mapToInt()`, `mapToLong()`, `mapToDouble()`, `mapToObj()`

```java
List<Item> items = Arrays.asList(
	new Item("musiq", 1),
  new Item("soul", 2),
  new Item("child", 3)
);
items.stream()
  .mapToInt(Item::getCount)
  .forEach(count -> System.out.println(count));
```

```
1
2
3
```



### flatMap()

- `flatMap()`, `flatMapToInt()`, `flatMapToLong()`, `flatMapToDouble()`

- return EntryStream - 각각의 분리된 Stream을 하나의 전체 스트림으로 통합하여 반환



**flatMap()**

```java
List<String> list = Arrays.asList("apple google", "facebook netflex");
list.stream()
  .flatMap(company -> Array.stream(company.split(" ")))
  .forEach(word -> System.out.println(word));
```

```
apple
google
facebook
netflex
```



**flatMapToInt()**

```java
List<String> inputList = Arrays.asList("10,20", "30,40");
inputList.stream()
  .flatMapToInt(data -> {
    String[] strArr = data.split(",");
    int[] intArr = new int[strArr.length];
    for(int i=0; i<strArr.length; i++) {
      intArr[i] = Integer.parseInt(strArr[i].trim());
    }
    return Arrays.stream(intArr);
  })
  .forEach(n -> System.out.println(n));
```

```
10
20
30
40
```



## Converting

### asStream()

- `asDoubleStream()`, `asLongStream()`

```java
int[] arr = {1,2,3,4,5};
IntStream intStream = Arrays.stream(arr);
DoubleStream doubleStream = intStream.asDoubleStream();
```



### boxed() & unboxed()



**boxed()**

- `int -> Integer`,  `long -> Long`, `double -> Double`

```java
int[] arr = {1,2,3,4,5};
IntStream intStream = Arrays.stream(arr);
intStream.boxed().forEach(obj -> System.out.println(obj.intValue()));
```

```
1
2
3
4
5
```



**unboxed()**

- `Integer  -> int`, `Long -> long`, `Double -> double`
- Native API 존재하지 않음

```java
stream().mapToInt(Integer::intValue);
stream().flatMapToInt(IntStream::of);
```



## Sorting

### sorted()

**Number Sort**

```java
IntStream stream = Arrays.stream(new int[]{3,2,1});
stream.sorted().forEach(n -> System.out.println(n));
```

```
1
2
3
```



**Object Sort**

- [1] `sorted()` - Comparable 구현
- [2] `sorted(Comparator<T>)` - Comparator 구현

```java
@Data
public class Item implements Comparable<Item> {
  private String name;
  private int count;
  
  @Override
  public int compareTo(Item o) {
    return Integer.compare(count, o.count);
  }
}

List<Item> items = Arrays.asList(
  new Item("apple", 102),
  new Item("google", 101),
  new Item("facebook", 103)
);

// [1] Comparable
items.stream()
  .sorted()
  .forEach(item -> System.out.println(item.getName()));

// [2] Comparator
items.stream()
  .sorted(new Comparator<Item>() {
  	@Override
  	public int compare(Item o1, Item o2) {
    	return Integer.compare(o1.count, o2.count);
  	}
	})
  .forEach(item -> System.out.println(item.getName()));
```

```
google
apple
facebook
```



## Looping

### peek()

- 중간처리 - 최종처리가 실행되지 않으면 지연되어 동작하지 않음

```java
int[] arr = new int[] {1,2,3,4,5};

Arrays.stream(arr)
  .filter(a -> a%2 == 0)
  // Not-Working
  .peek(n -> System.out.println(n)); 

int sum = Arrays.stream(arr)
  .filter(a -> a%2 == 0)
  // Working 
  .peek(n -> System.out.println(n)) 
  .sum(); 
```



### forEach()

- 최종처리

...



## Matching

- 최종처리



### allMatch & anyMatch & noneMatch

**allMatch()**

- return -> boolean

```java
int[] arr = {2,4,6};
boolean result = Arrays.stream(arr)
  .allMatch(a -> a%2 == 0); 
```

```
true
```



**anyMatch()** 

```java
int[] arr = {2,4,6};
boolean result = Arrays.stream(arr)
    .allMatch(a -> a%3 == 0);
```

```
true
```



**noneMatch()**

```java
int[] arr = {2,4,6};
boolean result = Arrays.stream(arr)
    .allMatch(a -> a%3 == 0);
```

```
false
```



### findFirst & findAny

- `findFirst()` - Optional
- `findAny()` - Optional



**findFirst()**

```java
int[] arr = {1,2,3,4};
int first = Arrays.stream(arr)
    .filter(a -> a % 2 == 0)
    .findFirst()
    .get();
```

```
2
```



**findAny()**

```java

```



## Counting

- `count()` - long
- `max(Comparator<T>)` - Optional\<T>
- `max()` - Optional 
- `min(Comparator<T>)` - Optional\<T>
- `min()` - Optional
- `average()` - OptionalDouble
- `sum()` - int, long, double



**max() & min()**

```java
int[] arr = {1,2,3,4};
int min = Arrays.stream(arr)
    .min() 
    .getAsInt();

List<Integer> list = Arrays.asList(1,2,3,4);
int min = list.stream()
    .min(Integer::min) 
    .get();
```

```
1
1
```





## Optional

### orElse()

```java
List<Integer> list = new ArrayList<>();
double avg = list.stream()
    .mapToInt(Integer::intValue)
    .average()
    .orElse(0.0);
```



### ifPresent()

```java
List<Integer> list = new ArrayList<>();
list.stream()
    .mapToInt(Integer->intValue)
    .average()
    .ifPresent(a -> System.out.println("average : " + a));
```



## Customizing

### reduce()

- [1] `reduce(Operator op)` -> OptionalInt
- [2] `reduce(int init, Operator op)` -> int

```java
List<Item> items = Arrays.asList(
	new Item("apple", 102),
  new Item("google", 101),
  new Item("facebook", 103)
);

int sumA = items.stream()
  .map(Item::getCount)
  .reduce((a, b) -> a + b)
  .get();

int sumB = items.stream()
  .map(Item::getCount)
  .reduce(0, (a, b) -> a + b);
```



## Collecting

### Collect()

- [1] List - Collectors.toList()
- [2] Set - Collectors.toSet()
- [3] Map - Collectors.toMap(Function keyMapper, Function valueMapper)
- [4] ConcurrentMap - Collectors.toConcurrentMap(Function keyMapper, Function valueMapper) - `ThreadSafety`



**Collectors.toList()**

```java
List<Item> itemList = items.stream()
  .collect(Collectors.toList());
```



**Collectors.toSet()**

```java
Set<Item> itemSet = items.stream()
  .collect(Collectors.toSet());
```



**Collectors.toMap(keyMapper, valueMapper)**

```java
Map<String, Item> itemMap = items.stream()
  .collect(Collectors.toMap(s -> s.getName(), s -> s));
```



**Collectors.toCollection(Supplier s)**

```java
HashSet itemSet = items.stream()
  .collect(Collectors.toCollection(HashSet::new));
```



### Customizing

- collect(Supplier<R>, BiConsumer<R>, BiConsumer<R, R>)
  - Supplier<R> - 수집될 컨테이너 객체 생성
  - BiConsumer<R> - 생성된 컨테이너 객체에서  요소를 수집
  - BiConsumer<R, R> - 수집된 컨테이너 객체를 결합 (순차처리에서는 호출되지 않고 병렬처리에서만 호출)

```java
@Data
class ITCompany {
    private List<Company> list;

    public ITCompany() {
        list = new ArrayList<Company>();
        System.out.println("[" + Thread.currentThread().getName() + "] ITCompany()");
    }

    public void accumulate(Company company) {
        list.add(student);
        System.out.println("[" + Thread.currentThread().getName() + "] accumulate()");
    }

    public void combine(ITCompany other) {
        list.addAll(other.getList());
        System.out.println("[" + Thread.currentThread().getName() + "] combine()");
    }
}
```

```java
ITCompany companyItem = totalItem.stream()
  .filter(s -> s.getCategory() == CompanyType.IT)
  .collect(ITCompany::new, ITCompany::accumulate, ITCompany::combine);
```

```
[main] ITCompany()
[main] accumulate()
[main] accumulate()
```

> SingleThread에선 별도로 conbine() 메소드가 실행되지 않음



### Grouping

- groupingBy()
  - [1] groupingBy(Function<T,K> f)
  - [2] groupingBy(Function<T,K> f, Collector<T, A, D> c)
  - [3] groupingBy(Function<T,K> f, Supplier<Map<K,D>> s, Collector<T, A, D> c)
- groupingByConcurrent() - `ThreadSafety`



**groupingBy(Function<T,K> f)**

```java
Map<Company.Category, List<Company>> mapByCategory = totalList.stream()
  .collect(Collectors.groupingBy(
    Company::getCategory));
```



**groupingBy(Function<T,K> f, Collector<T, A, D> c)**

```java
Map<Company.Category, List<String>> mapByCategory = totalList.stream()
  .collect(Collectors.groupingBy(
    Company::getCategory,
    Collectors.mapping(Company::getName, Collectors.toList())));
```



**groupingBy(Function<T,K> f, Supplier<Map<K,D>> s, Collector<T, A, D> c)**

```java
Map<Company.Category, List<String>> mapByCategory = totalList.stream()
  .collect(Collectors.groupingBy(
    Company::getCategory,
    TreeMap::new,
    Collectors.mapping(Company::getName, Collectors.toList())));
```



### Grouping + Counting + Joining

- `counting()`
- `averagingDouble()`
- `summingInt()`, ` summingLong()`, `summingDouble()`
- `maxBy(Comparator<T> c )` - Comparator를 이용한 최대 산출
- `minBy(Comparator<T> c)` - Comparator를 이용한 최대 산출
- `mapping(Function<T, U> f, Collector<U,A,R> c)`
- `joining(CharSequence delimiter)` - delimiter로 연결된 String 반환



**averagingDouble()**

```java
Map<Company.Category, Double> mapByCategory = totalList.stream()
  .collect(
		Collectors.groupingBy(
    		Company::getCategory,
    		Collectors.averagingDouble(Company::getCount)
    )
);
```



**mapping() + joining()**

```java
Map<Company.Category, String> mapByName = totalList.stream()
  .collect(
		Collectors.groupingBy(
    		Company::getCategory,
    		Collectors.mapping(
        		Company::getName,
      			Collectors.joining(",")
      )
    )
);
```



## Parallel

- 동시성 vs 병렬성
- 작성병렬성 vs 데이터병렬성



### ForkJoin



![image-20191012010951051](/assets/images/image-20191012010951051.png)

- ForkJoin Process
  - Fork - 전체 데이터를 서브 데이터로 분리
  - Parallel - 서브 데이터를 멀티 코어로 병렬 처리
  - Join - 서브 결과를 합해서 최종 결과 도출



### parallelStream()

```javascript
MaleStudent maleStudent = totalList.parallelStream()
    .filter(s -> s.getZender() == Student.Zender.MALE)
    .collect(MaleStudent::new, MaleStudent::accumulate, MaleStudent::combine);
```

```
[main] ITCompany()
[ForkJoinPool] ITCompany()
[ForkJoinPool] ITCompany()
...
[main] accumulate()
[ForkJoinPool] accumulate()
...
[main] combine()
[ForkJoinPool] combine()
```



### parallel()

```java
List list = Arrays.asList(1,2,3,4,5);
long start = System.nanoTime();
list.stream()
  .parallel()
  .forEach(n -> System.out.println(n));
long end = System.nanoTime();
System.out.println(end - start);
```



> Parallel 고려사항

- 자원의 수와 자원당 처리 시간
- 스트림 소스의 종류
  - `Array, ArrayList` -> 데이터 병렬처리에 유리한 구조
  - `HashSet, TreeSet, LinkedList` -> 데이터 병렬처리에 불리한 구조
- 코어의 수
  - `SingleCore` - 병렬처리에 불리 (동시성으로 진행)
  - `MultiCode` - 병렬처리에 유리