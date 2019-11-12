---
typora-root-url: ../../../moonscoding.github.io
---

## Garbage Collection



> stop-the-world

GC 실행을 위해 JVM이 애플리케이션 실행을 멈추는 것



Java는 프로그램 코드에서 메모리를 명시적으로 지정하여 해제하지 않음

해당 객체를 null로 지정하거나 System.gc()를 사용하는 방식으로 메모리 해제가 가능하나

System.gc() 메서드는 예상치 못한 성능에 영향을 줄 수 있음



- 대부분 객체는 금방 접근 불가능 상태(unreadable)

- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재



> Reachability

어떤 객체에 유효한 참조가 있으면 reachable / 없으면 unreachable

unreachable 객체를 가비지로 간주해 GC 수행

- stack area
- heap area
- method area



<img src="/assets/images/image-20191104235741887.png" alt="image-20191104235741887" style="zoom:60%;" />


> Young & Old 영역

`Young 영역 `

새롭게 생성한 객체가 존재

대부분 객체가 금방 접근 불가능한 상태가 되어 Young 영역에 생성되었다가 사라짐

이 영역에서 객체가 사라질 때 Minor GC 발생



`Old 영역`

접근 불가능 상태가 되지 않아 Young 영역에서 살아 남은 객체가 복사되는 영역

Young 영역보다 크게 할당되며 보다 적게 GC 발생

이 영역에서 객체가 사라질 떄 Major GC(Full GC) 발생



`Perm 영역` (Permanent Generation 영역)

객체나 억류된 문자열 정보를 저장하며 Old영역에서 살아남은 객체가 영원히 남아 있는 곳은 아님

이 영역에서도 GC 발생, Major GC에 포함

![image-20191104222000888](/assets/images/image-20191104222000888.png)



## Young 영역

- Eden 영역
- Survivor 영역 (2개)



1. 새로 생성한 대부분 객체는 Eden 영역 
2. Eden 영역에서 GC가 한번 발생하면 이미 살아남은 객체는 Survivor 영역중 하나로  이동
3. Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓임
4. 하나의 Survivor 영역이 가득 차게 되면 그 중 살아남은 객체를 다른 Survivor 영역으로 이동 (Survivor Switching)
   가득 찬 Survivor 영역은 아무 데이터도 없는 상태로
5. 이 과정을 반복하며 살아남은 객체는 Old 영역으로 이동



> bump-the-pointer & TLABs(Thread-Local Allocation Buffer)

`bump-the-pointer`

Eden 영역에 마지막 할당 객체를 추적, 마지막 객체는 Eden 영역의 맨 위에 있음

다음에 생성되는 객체가 있을 경우, 해당 객체의 크기가 Eden 영역에 넣기 적당한지 확인



`TLABs`

Thread-Safe 환경에서 사용하는 기법으로 각각의 스레드가 Eden영역의 작은 덩어리를 가질 수 있도록



## Old 영역

데이터가 가득차면 GC 실행



- Serial GC - 싱글코어를 위한 방식임으로 실제 환경에서 사용하지 않음
- Parallel GC
- Parallel Old GC( Parallel Compacting GC)
- Concurrent Mark & Sweep GC 
- G1(Garbage First) GC



### Serial GC

적은 메모리와 CPU 코어 개수가 적을 때 적합 `Why?`



`Mark-Sweep-Compact`

1. Old 영역에 살아 있는 객체를 식별 (Mark)
2. Heap 앞 부분부터 확인하여 살아 있는 것만 남김 (Sweep)
3. 각 객체들이 연속되게 쌓이도록 Heap 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눔 (Compaction)



### Parallel GC

메모리가 충분하고 CPU 코어의 개수가 많을 대 유리, Throughput GC라고도 부른다.

Serial GC와 알고리즘은 같으나 멀티 스레드로 동작



### Parallel Old GC

`Mark-Summary-Compaction`

1. Mark
2. 단계는 앞서 GC를 수행한 영역에 대해 별도로 살아 있는 객체를 식별 (Summary)
3. Compaction



### CMS GC

stop-the-world 시간이 매우 짧아 응답속도가 중요한 경우 사요, Low Latency GC라고도 부른다.



1. Initial Mark 단계에서 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾은 것으로 종료 (멈추는 시간은 매우 짧음)

2. Concurrent Mark 단계에서 살아 있다고 확인한 객체에서 참조하는 객체들을 따라가면서 확인 (다른 스레드와 동시에 진행)
3. Remark 단계에서 Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체 확인
4. Concurrent Sweep 단계에서  쓰레기를 정리 (다른 스레드와 동시에 진행)



단점.

- 다른 GC보다 CPU 사용량 높음
- Compaction 단계가 기본으로 제공되지 않음



### G1 GC

Java 1.7 등장, 성능에 장점이 있음

G1(Garbage First) GC는 기존 Young & Old 영역이 아닌 바둑판의 각 영역에 객체를 할당하고 GC를 실행

그런 후 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행



## Soft, Weak, Phantom Reference



### Reference Object & Referent



java.lang.ref 에서는 3가지 `reference object`를 지원

WeakReference()로 생성된 객체가 `reference object`이며, new Sample()로 생성된 객체가 `referent`

- `soft reference`
- `weak reference`
- `phantom reference`



<img src="/assets/images/image-20191105001121826.png" alt="image-20191105001121826" style="zoom:67%;" />

```java
WeakReference<Sample> wr = new WeakReference<Sameple>(new Sameple());
Sample ex = wr.get();
```



<img src="/assets/images/image-20191105001205640.png" alt="image-20191105001205640" style="zoom:67%;" />

```java
ex = null;
```



기존의 GC는 대상여부를 reachable / unreachable로만 판단하였으나

java.lang.ref 패키지를 사용하여 reachable 객체들을 GC 때 동작을 다르게 지정할 수 있음 

즉 GC 대상 여부를 판별하는 부분에 사용자 코드가 개입할 수 있음



<img src="/assets/images/image-20191105000446938.png" alt="image-20191105000446938" style="zoom:60%;" />

unreachable 뿐 아니라 weakly reachable 객체도 가비지 객체로 간주되어 메모리에서 회수

root set으로 부터 시작된 참조 사슬에 포함되어 있음에도 GC가 동작할 때 회수되어 참조는 가능하나 

반드시 유효할 필요는 없는 LRU 캐시와 같은 임시 객체들을 저장하는 구조를 쉽게 구현 가능



### Strengths of Reachability



- `strogly reachable`

- `softly reachable`
- `weakly reachable `
- `phantomly reachable`



**strongly reachable**

root set으로 시작하여 어떤 reference object도 중간에 끼지 않은 상태로 참조 가능한 객체 



**softly reachable**

strongly reachable 객체가 아닌 객체중 weak reference, phantom reference 없이 

soft reference 만 통과하는 참조 사슬이 하나라도 있는 객체



**weakly reachable**

strongly reachable 객체도 softly reachable 객체도 아닌 객체 중  

phantom reference 없이 weak reference 만 통과하는 참조 사슬이 하나라도 있는 객체



**phantomly reachable**

strongly reachable 객체도 softly reachable 객체도 weakly reachable 객체도 아닌 객체 중  

finalize 되었지만 아직 메모리가 회수되지 않은 상태



**unreachable**

root set으로 부터 시작되는 참조 사슬로 참조되지 않는 객체



#### Softly Reachable & SoftReference

strong reachable이 아니면서 오직 SoftReference 객체로만 참조된 객체

Heap에 남아 있는 메모리 크기와 객체의 사용 빈도에 따라 GC 여부 결정



weakly reachable 객체와 달리 GC 가 동작할 때 마다 회수되지 않으며 자주 사용될수록 더 오래 살아남음



#### Weakly  Reachable &WeakReference

softly reachable 객체와는 달리 GC 수행마다 회수 대상

WeakReference 내의 참조가 null이 되고, weakly reachable 객체는 unreachable 객체와 마찬가지 상태가 되어 회수됨



LRU 캐시와 같은 애플리케이션에서는 softly reachable 보다 weakly reachable 객체가 유리하여 많이 사용



#### Phantom Reachable & PhantomReference

finalize와 메모리 회수 사이에 관여

PhantomReference로만 참조되는 객체는 먼저 finalize 된 이후에 phantomly reachable로 간주



객체에 대한 참조가 PhantomReference만 남게 되면 해당 객체는 바로 finalize 처리



#### ReferenceQueue

java.lang.ref 패키지가 제공하며 SoftReference 객체나 WeakReference 객체가 참조하는 객체가 GC 대상이면 

SoftReference, WeakReference 객체 내의 참조는 null로 설정,  Reference 객체 자체는 ReferenceQueue에 enqueue 처리 (GC에 의해)

PhantomReference 객체는 내부의 참조를 null로 설정하지 않고 참조된 객체를 phantomly reachable 객체로 만든 후에 enqueue 처리

```java
ReferenceQueue<Object> rq = new ReferenceQueue<Object>();
PhantomReference<Object> pr = new PhantomReference<Object>(referent, rq);
```



> 전체 순서

1. soft references
2. weak references
3. finalize
4. phantom references
5. 메모리 회수



> 내부 캐시등을 구현하고자 한다면 WeakReference 혹은 WeakHashMap 만으로 충분하다는 필자의 의견



> 참조
>
> - [https://d2.naver.com](https://d2.naver.com/helloworld/1329)
> - [https://d2.naver.com](https://d2.naver.com/helloworld/329631)

