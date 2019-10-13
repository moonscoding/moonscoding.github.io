---

title: "Java Collection"
permalink: /docs/collection/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"
typora-root-url: ../../../moonscoding.github.io
---



## Collection

객체를 효율적으로 관리하기 위한 자바의 자료구조 모음

![image-20191013215139015](/assets/images/image-20191013215139015.png)



## List

- 순서 유지
- 중복 가능



### ArrayList

내부 저장 구조에 Array를 사용하는 List

저장용량이 초과하면 자동으로 저장용량을 늘림

```java
List<String> list = new ArrayList<String>();
List<String> list = new ArrayList<String>(30);
```



#### Vector

ArrayList와 동일한 내주 저장 구조를 가짐

ArrayList와 다른 점은 `동기화(Synchronized)`된 메소드를 가짐으로  `ThreadSafety` 하다는 것 [Syncronized & ThreadSafety?]()

```java
List<String> list = new Vector<String>();
List<String> list = new Vector<String>(30);
```



### LinkedList

다음 객체의 주소를 참조하여 체인처럼 관리하는 List

```java
List<String> list = new LinkedList<String>();
```



### List API

- `add()`
  - [1] boolean add(E e)
  - [2] void add(int index, E e)
- `set()`
  - set(int index, E e);
- `contains()`
  - boolean contains(Object o)
- `get()`
  - E get(int index)
- `isEmpty()`
  - boolean isEmpty()
- `size()`
  - int size()
- `clear()`
  - vold clear()
- `remove()`
  - E remove(int index)
  - boolean remove(Object o)



## Set

- 순서 없음
- 중복 불가



### HashSet

객체의 `hashCode()`로 비교한 후 같다면 `equals()` 로 다시 한번 두 객체를 비교하여 저장여부를 판단

```java
Set<String> set = new HashSet<String>();
```



### TreeSet

BinaryTree 기반의 Set Collection - [BinaryTree?]()

`Comparable` 구현한 객체를 요구하며 구현하지 않은 객체는 `Comparator` 객체와 함께 생성되야 함 - `ClassCastException`

```java
TreeSet<String> treeSet = new TreeSet<String>();
```

```java
TreeSet<E> treeSet = new TreeSet<E>(new AscendingComparator());
TreeSet<E> treeSet = new TreeSet<E>(new DescendingComparator());
```



#### TreeSet API 

**Search Method**

- `first()` - 제일 낮은 객체 반환
  - E first()
- `last()` - 제일 높은 객체 반환
  - E last()
- `lower()` - 주어진 객체보다 바로 아래 객체 반환
  - E lower(E e)
- `higher()` - 주어진 객체보다 바로 위 객체 반환
  - E heighter(E e)
- `floor()` - 주어진 객체와 동등한 객체가 있다면 반환, 없다면 바로 아래 객체 반환
  - E floor(E e) 
- `ceiling()` - 주어진 객체와 동등한 객체가 있다면 반환, 없다면 바로 위의 객체 반환
  - E ceiling(E e)
- `pollFirst()` - 제일 낮은 객체 반환 및 제거
  - E pollFirst()
- `pollLast()` - 제일 높은 객체 반환 및 제거
  - E pollLast()



**Sort Method**

- `descendingIterator()` - 내림차순으로 정렬된 Iterator 반환
  - Iterator\<E> descendingIterator()
- `descendingSet()` - 내림차순으로 정렬된 NavigableSet 반환
  - NavigableSet\<E> descendingSet()
  - 오름차순 - descendingSet() x2



**Range Method**

- `headSet()` - 주어진 객체보다 낮은 객체들을 NavigableSet으로 반환, 두 번째 인자로 주어진 객체의 포함여부 파악
  - NavigableSet\<E> headSet(E e, boolean inclusive)
- `tailSet()` - 주어진 객체보다 높은 객체들을 NavigableSet으로 반환, 두 번째 인자로 주어진 객체의 포함여부 파악
  - NavigableSet\<E> tailSet(E e, boolean inclusive)
- `subSet()` - 시작과 끝으로 주어진 객체 사이의 객체들을 NavigableSet으로 반환, 포함여부는 위와 같음 
  - NavigableSet\<E> subSet(E fromE, boolean fromInclusive, E toE, boolean toInclusive)



### Set API

- `add()`
  - boolean add(E e)
- `contains()`
  - boolean contains(Object o)
- `isEmpty()`
  - boolean isEmpty()
- `size()`
  - int size()
- `iterator()`
  - Iterator\<E> iterator()
    - `hasNext()`
      - boolean hasNext()
    - `next()`
      - E next()
    - `remove()`
      - void remove()
      - Set Collection 데이터에서 삭제

```java
Set<String> set = new HashSet<String>();
Iterator<String> iterator = set.iterator();
```

- `clear()`
  - vold clear()
- `remove()`
  - boolean remove(Object o)



## Map

- Key & Value (Entry) 구조
- Key 중복 불가



### HashMap 

객체의 `hashCode()`로 비교한 후 같다면 `equals()` 로 다시 한번 두 객체를 비교하여 저장여부를 판단

```java
Map<String, Integer> map = new HashMap<String, Integer>();
```



#### Hashtable

HashMap과 동일한 내부 저장 구조

HashMapr과 다른 점은 `동기화(Synchronized)`된 메소드를 가짐으로  `ThreadSafety` 하다는 것 [Syncronized & ThreadSafety?]()

```java
Map<String, Integer> map = new Hashtable<String, Integer>();
```



#### Properties

Hashtable의 하위클래스로 동일한 특징을 가짐

키와 값이 String 타입으로 제한되는 Map으로 파일에 저장된 .properties 파일을 읽을 때 주로 사용

```java
Properties properties = new Properties();
properties.load(new FileReader("/memo.properties"));
```



> [Java File Part]()
>  File Part. 에서 다시 한번 자세하게 다룰 예정



### TreeMap

BinaryTree 기반의 Set Collection - [BinaryTree?]()

Key로 Comparable 구현한 객체를 요구하며 구현하지 않은 객체는 `Comparator` 객체와 함께 생성 - `ClassCastException`

```java
TreeMap<String, Integer> treeMap = new TreeMap<String, Integer>();
```

```java
TreeMap<K, V> treeMap = new TreeMap<K, V>(new AscendingComparator());
TreeMap<K, V> treeMap = new TreeMap<K, V>(new DescendingComparator());
```



#### TreeMap API

**Search Method**

- `firstEntry()` - 제일 낮은 Entry 객체 반환
  - Map.Entry<K, V> firstEntry()
- `lastEntry()` - 제일 높은 Entry 객체 반환
  - Map.Entry<K, V> lastEntry()
- `lowerEntry()` - 주어진 객체보다 바로 아래 Entry 객체 반환
  - Map.Entry<K, V> lowerEntry(K key)
- `higherEntry()` - 주어진 객체보다 바로 위 Entry 객체 반환
  - Map.Entry<K, V> heighterEntry(K key)
- `floorEntry()` - 주어진 객체와 동등한 키가 있다면 반환, 없다면 바로 아래 객체 반환
  - Map.Entry<K, V> floorEntry(K key) 
- `ceilingEntry()` - 주어진 객체와 동등한 키가 있다면 반환, 없다면 바로 위의 객체 반환
  - Map.Entry<K, V> ceilingEntry(K key)
- `pollFirstEntry()` - 제일 낮은 Entry 객체 반환 및 제거
  - Map.Entry<K, V> pollFirstEntry()
- `pollLastEntry()` - 제일 높은 Entry 객체 반환 및 제거
  - Map.Entry<K, V> pollLastEntry()



**Sort Method**

- `descendingKeySet()` - 내림차순으로 정렬된 NavigableSet 반환
  - NavigableSet\<E> descendingKeySet()
- `descendingMap()` - 내림차순으로 정렬된 Entry 객체의 NavigableMap 반환
  - NavigableMap<K, V> descendingMap()
  - 오름차순 - descendingMap() x2



**RangeMethod**

- `headSet()` - 주어진 객체보다 낮은 객체들을 NavigableMap으로 반환, 두 번째 인자로 주어진 객체의 포함여부 파악
  - NavigableMap<K, V> headSet(E e, boolean inclusive)
- `tailSet()` - 주어진 객체보다 높은 객체들을 NavigableMap으로 반환, 두 번째 인자로 주어진 객체의 포함여부 파악
  - NavigableMap<K, V> tailSet(E e, boolean inclusive)
- `subSet()` - 시작과 끝으로 주어진 객체 사이의 객체들을 NavigableMap으로 반환, 포함여부는 위와 같음 
  - NavigableMap<K, V> subSet(E fromE, boolean fromInclusive, E toE, boolean toInclusive)



### Map API

- `put()`
  - V put(K key, V value)

- `containsKey()`
  - boolean containsKey(Object key)

- `containsValue()`
  - boolean containsValue(Object value)

- `entrySet()`
  - Set<Map.Entry<K,V>> entrySet()

- `keySet()`
  - Set\<K> keySet() 

- `values()`
  - Collection\<V> values()

- `get()`
  - V get(Object K)

- `isEmpty()`
  - boolean isEmpty()

- `size()`
  - int size()

- `clear()`
  - void clear()

- `remove()`
  - V remove(Object k)



## Stack & Queue

### Stack

Class

LIFO(Last In First Out)

```java
Stack<E> stack = new Stack<E>();
```



- `push()` - 삽입
  - E push(E e) 
- `peek()` - 마지막 삽입 객체 반환
  - E peek()
- `pop()` - 마지막 삽입 객체 반환 & 제거
  - E pop()



### Queue

Interface

FIFO(First In First Out)

```java
Queue<E> queue = new LinkedList<E>();
```



- `offer()` - 삽입
  - boolean offer(E e)
- `peek()` - 첫 번째 삽입된 객체 반환
  - E peek()
- `poll()` - 첫 번째 삽입된 객체 반환 & 제거
  - E poll()



## Syncronized



- `synchronizedList()` - 동기화 List로 반환
  - List\<T> synchronizedList(List\<T> list)
- `synchronizedSet()` - 동기화 Set으로 반환
  - Set\<T> synchronizedSet(Set\<T> set) 
- `synchronizedMap()` - 동기화 Map으로 반환
  - Map<K, V> synchronizedMap(Map<K, V> map)



## Parallel



### ConcurrentHashMap

Map의 구현클래스

요소별로 부분(Segment) 잠금을 사용하여 `ThreadSafety + Parallel`를 보장

```java
Map<K, V> map = new ConcurrentHashMap<K, V>();
```



### ConcurrentLinkedQueue

Queue의 구현클래스

`Lock-Free 알고리즘` 구현 여러 개의 스레드가 동시에 접근할 경우 잠금을 사용하지 않고 ThreadSafety를 보장

```java
Queue<E> queue = new ConcurrentLinkedQueue<E>();
```



