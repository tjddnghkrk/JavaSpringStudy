<br><br><br>
**ConcurrentHashMap**

<br>
ConcurrentHashMap을 알기 전에 먼저 Thread-Safe에 대해 알아보았다. <br><br>
또한 JAVA에서 Thread-Safe를 고려해야 하는 상황에 대해 간단히 정리했다. <br><br><br>

> Thread-Safe
    
    동기화(Synchronize)라고 표현하기도 하며 어떠한 Class의 인스턴스가 여러개의 Thread에서 동시 참조되고 해당 객체에 Operation이 발생해도 정합성을 유지해줄때 Thread-Safe 하다 라고 표현
    
    cf. 정합성 - 데이터가 서로 모순이 없이 일관되게 일치
    
    @ThreadSafe 어노테이션을 이용해 해당 Class가 Thread-Safe 함을 표시하기도 함.
    
<br>
    
> Java에서 Thread-Safe 고려해야 하는 경우
<br>

    
>>    1. 스트링 빈(bean)

    
    스트링 빈(bean) : 인스턴트 멤버 변수가 있는 경우 쓰레드 세이프 하지 않다. 
    
    set 메소드를 이용하면 변수가 바뀌는데, 다른 쓰레드 입장에서는 예상치 못한 변화로 문제를 일으킬 수 있음
    
    -> 인스턴트 멤버 변수가 아닌지역 변수를 사용하는 것을 권장
<br>
    
>>    2. Cache

    
    동기화에 Cache도 고려하기. 직접 참조하지 않고, L1, L2 캐시를 참조하는 경우가 있기 때문에.
        
        → volatile로 해결.
        
        - Java volatile
            
            Java 변수를 Main Memory에 저장하겠다는 것.
            
            (Read, Write를 CPU cache가 아닌, Main Memory에서 하는 것.)
            
            Thread1 이 계속 값을 증가시키더라도 Thread2의 캐시는 업데이트 되지 않아서 값이 일치하지 않음.
            
            Read - Write의 값 일치는 보장하지만 여러 쓰레드가 Write 하는 경우는 보장할 수 없기에 synchronized 해야 함.
            
            캐시를 사용하지 않기에 성능에 문제를 줄 수 있음.
            
        
     또한 인스턴스 변수를 변경하지 않는 메서드(단순히 읽거나 리턴하는)라고 하더라도 동기화 처리(synchronized) 해주지 않으면 동기화가 깨진다.
        
<br>
 
 >>    3. ConcurrentHashMap

            
    
    - HashMap : 성능은 좋지만 Thread-safe하지 않다.
    
    - Hashtable : 성능이 좋지 않지만 Thread-safe하다.

    
    Hashtable은 method 전체에 synchronized 키워드를 사용 (메소드 전체가 임계구역)
    
    → 누군가 put으로 lock을 획득 중이면, 다른 쓰레드는 get도 진입할 수 없다.
    
    Thread-safe하지만 멀티 쓰레드 환경에서는 느리다.
    
    
    - ConcurrentHashMap : Hashtable보다 성능이 좋고 Thread-safe하다.
    
    
    get()에는 synchronized가 없고, put()의 중간에 synchronized가 있다. (메소드 전체가 임계구역이지 않다.)
    
    → 읽기 작업에는 여러 Thread가 읽을 수 있지만, 쓰기 작업에는 특정 세그먼트 or 버킷에 대한 lock을 사용
    
    검색 작업(get)은 lock이 이뤄지지 않으며 갱신작업(put)과 동시에 할 수 있기에 병목현상이 적다.
    
    
    결론 : ConcurrentHashMap은 Thread-safe하고 Hashtable보다 성능이 뛰어나다. 
    
    검색(get)은 검색 method가 호출되는 시점에, 가장 최신에 완료된 업데이트 작업의 결과가 반영된다.

<br>


 >>    4. StringBuffer
        
    
    - String vs StringBuffer, StringBuilder
    
    String은 immutable(불변)하고 StringBuffer와 StringBuilder는 mutable(가변)
    
    
    - StringBuffer vs StringBuilder
    
    StringBuffer는 멀티쓰레드환경에서 synchronized 키워드가 가능하므로 동기화가 가능하다. (thread-safe)
    
    StringBuilder는 thread-safe하지 않다. 그러다 동기화를 고려하지 않기에 싱글쓰레드에서 연산 (+ 등)이 빠르다.
<br>
    
  >>    5. int를 thread-safe하게 사용
        
    
    
    JAVA에서 동시성 문제를 해결하는 3가지 방법 
    
    (volatile - read/write만, synchronized - 동시성 보장하나 비용이 큼, ** Atomic Type **)
    
    
    int의 경우 AtomicInteger.
    
    → Atomic은 CAS(Compare And Swap) 방식에 기반하여 동기화 문제를 해결
    
    CAS란 변수의 값을 변경하기 전에 기존에 가지고 있던 값이 **내가 예상하던 값**과 같을 경우에만 새로운 값으로 할당하는 방법
    
    한 번에 단 하나의 쓰레드만 변수의 값을 변경할 수 있도록 한다.
    
    
<br>
> 레퍼런스

[https://nesoy.github.io/articles/2018-06/Java-volatile](https://nesoy.github.io/articles/2018-06/Java-volatile)

[https://jeong-pro.tistory.com/227](https://jeong-pro.tistory.com/227)

[https://siyoon210.tistory.com/140](https://siyoon210.tistory.com/140)

[https://doorisopen.github.io/developers-library/Java/2020-07-08-java-hashmap-and-thread-safe](https://doorisopen.github.io/developers-library/Java/2020-07-08-java-hashmap-and-thread-safe)

[https://jeong-pro.tistory.com/85](https://jeong-pro.tistory.com/85)
