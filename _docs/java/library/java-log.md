

## Log Library

Java에서 활용할 수 있는 로그 라이브러리와 특징

- java.util.logging
- Apache Commons logging
- log4j
- **gogback**



WAS에서 관리하는 로그와 차이는 ? 어떻게 통합할까 ?





### logback

- logback-core
- logback-classic
- logback-access -> HTTP 요청에 대한 강력한 디버깅



#### Dependency

MAVEN

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>${logback.version}</version>
</dependency>
```



#### Level

ERROR, WARN, INFO, DEBUG, TRACE



#### Config

Appender -> 어디에 어떻게 로그를 찍을 것인가

Logger -> 로그 환경을 어떻게 구성할 것인가



###Logging

LoggerFactory

```java
private static Logger logger = LoggerFactory.getLogger(MyClass.class);
```

Lombok

```
@Slf4j
public class MyClass {

}
```





## SLF4J

slf4j ?

facade pattern ? 

창구 일원화 방식으로 구현 종류와 상관없이 일관된 로깅 코드를 작성









## Log Storage





## Log Integration

로그 통합 - ScaleOut or MSA 같이 분리된 서버환경에서 통합된 로그 환경을 구성





> 참조
>
> - [https://sunnykwak.tistory.com](https://sunnykwak.tistory.com/85)
> - [https://beyondj2ee.wordpress.com](https://beyondj2ee.wordpress.com/2012/11/09/logback-사용해야-하는-이유-reasons-to-prefer-logback-over-log4j/)
> - http://logback.qos.ch/documentation.html