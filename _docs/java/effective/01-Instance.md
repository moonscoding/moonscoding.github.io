## Effective Java - Instance

> 객체 생성과 파괴



### Item01. 생성자 대신 정적 팩터리 메서드를 고려



### Item02. 생성자에 매개변수가 많다면 빌더를 고려

#### builder

> [Constructor - Guild1] 생성자에 매개변수가 많다면 빌더를 고려

점층적 생성자 패턴으 쓸 수도 있지만, 매개변수 개수가 많아지면 클라이언트 코드를 작성하거나 읽기 어려움

자바빈즈 패턴에서는 객체 하나를 만드려면 메서드를 여러 개 호출해야 하며 객체가 완전히 생성되기 전까지 일관성이 무너진 상태에 노출

빌더패턴은 명명된 선택적 매개변수(named optional parameters)를 흉내



빌더패턴은 계층적으로 설계된 클래스와 함께 쓰기 좋음

```java
public abstract class Pizza {
  public enum Topping {HAM, MUSHROOM, ONION, PEPPER, SAUSAGE}
  final Set<Topping> toppings;
  
  abstract static class Builder<T extends Builder<T>> {
    EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);
    public T addTopping(Topping topping) {
      toppings.add(Objects.requiredNonNull(toppings));
      return self();
    }
    
    abstract Pizza build();
    
    // 하위 클래스는 이 메서드를 재정의하여 this를 반환하도록 해야
    protected abstract T self();
  }
  
  Pizza(Builder<?> builder) {
    toppings = builder.toppings.clone(); // [Method - Guild1]
  }
  
}
```



### Item03. private 생성자나 열거 타입으로 싱글턴임을 보증

클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트 하기가 어려움



> 싱글턴 생성 방법1 

```java
public class Elvis {
  public static final Elvis INSTANCE = new Elvis();
  
  private Elvis() { 
  
  }
  
  public void leaveTheBuilding() {
    // ...
  }
}
```



> 싱글턴 생성 방법2

```java
public class Elvis {
  public static final Elvis INSTANCE = new Elvis();
  
  private Elvis() { 
  
  }
  
  public static Elvis getInstance() {
    return INSTANCE;
  }
  
  public void leaveTheBuilding() {
    // ...
  }
}
```



>  싱글턴 생성 방법3

```java
public enum Elvis {
  INSTANCE;
  
  public void leaveTheBuilding() {
    // ...
  }
}
```

원소가 하나인 열거 타입을 선언

직렬화를 할 수 있고 심지어 아주 복잡한 직렬화 상황이나 리플랙션 공격에서도 제2의 인스턴스가 생기는 일을 막아줌

대부분 상황에서 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법



### Item04. 인스턴스화를 막으려거든 private 생성자를 사용 

인스턴스화를 막는 상황 ? 단순히 정적 메서드와 정적 필드만을 담은 클래스를 만들고 싶을 때



추상 클래스로 만드는 것으로는 인스턴스화를 막을 수 없음 - 하위 클래스를 만들어 인스턴스화하면 그만

private 생성자를 추가하면 클래스의 인스턴스화를 막을 수 있음

AssertionError 호출로 내부 환경에서도 생성자가 호출되는 것을 경계

```java
private UtilityClass() {
  throw new AssertionError();
}
```



### Item05. 자원을 직접 명시하지 말고 의존 객체 주입을 사용

사용하는 자원에 따라 동작이 달라지는 클래스에 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않음

[의존객체주입패턴] 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식

```java
public class SpellChecker {
  
  private final Lexicon dictionary;
  
  public SpellChecker(Lexicon dictionary) {
    this.dictionary = Objects.requiredNonNull(dictionary);
  }
  
  public boolean isValid(String word) { ... }
  public List<String> suggestions(String type) { ... }
}
```



### Item06. 불필요한 객체 생성 피하기

> Ex1

```java
String s = new String("newString!");
```

```java
String s = "newString";
```



> Ex2

Strung.matches()는 정규표현식으로 문자열 형태를 확인하는 가장 쉬운 방법이나, 성능이 중요한 상황에서 바복해 사용하기에 적합하지 않음

```java
staic boolean isRomanNuneral(String s) {
  return s.matchs("regex");
}
```

```java
public class RommanNumerals {
	private static final Pattern ROMAN = Pattern.compile("regex");
  
  static boolean isRomanNumeral(String s) { 
  	return ROMAN.matcher(s).matches();
  }
}
```



### Item07. 다 쓴 객체 참조를 해제하라

객체 참조를 null 처리하는 일은 예외적이 경우여야 함

다 쓴 참조를 해제하는 가장 좋은 방법은 그 참조를 담은 변수를 유효범위 밖으로 밀어내는 것 ( 변수의 범위를 최소가 되게 정의 했다면 )

자기 메모리를 직접 관리하는 클래스라면 프로그래머는 항시 메모리 누수에 주의해야

캐시 역시 메모리 누수를 일으키는 주범 ( WeakHashMap을 사용해 캐시 사용 - 다 쓴 엔트리는 즉시 자동으로 제거 )



### Item08. finalizer와 cleaner 사용 피해라

finalize는 예측할 수 없고 상황에 따라 위험할 수 있어 일반적으로 불 필요

cleaner는 finalizer보다는 덜 위험하지만 여전히 예측할 수 없고 느리고 일반적으로 불 필요



finalizer & cleaner 는 제 때 실행되어야 하는 작업은 절대 할 수 없음

예컨대 파일 닫기를 finalizer & cleaner에게 맡기면 중대한 오류를 발생할 수 있음

상태를 영구적으로 수정하는 작업에서 절대 finalizer & cleaner에 의존해서는 안됨



### Item09. try-finally 보다 try-with-resources 사용

자원 닫기는 클라이언트가 놓치기 쉬워 예측할 수 없는 성능문제를 야기

```java
static String firstLineOfFile(String path) throws IOException {
  BufferedReader br = new BufferedReader(new FileReader(path));
  try {
    retun br.readLine();
  } finally {
    br.close();
  }
}
```

AutoCloseable 인터페이스 구현

```java
static String firstLineOfFile(String path) throw IOExeption {
  try(BufferedReader br = new BufferedReader(new FileReader(path))) {
    return br.readLine();
  }
}
```

```java
static String firstLineOfFile(String src, String dst) throw IOExeption {
  try(InputStream in = new FileInputStream(src);
      OutputStream out = new FileOutputStream(dst)) {
    byte[] buf = new byte[BUFFER_SIZE];
    int n;
    while((n=in.read(buf)) >= 0) {
      out.write(buf, 0, n);
    }
  }
}
```

