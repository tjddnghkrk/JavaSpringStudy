<br><br><br>
<h1> 람다식 </h1>

```java
@Override
    public Optional<Member> findeByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
        //하나라도 있는지 찾기
    }
```

<br>
다음과 같은 코드를 해석하기 어려워서 람다식에 대해 찾아보았다.<br>
람다 뿐 아니라 Stream/함수형 인터페이스 에 대한 지식이 필요했음을 알 수 있었음. <br><br>
위의 코드를 해석한 후에, Stream API, Lambda, 함수형 인터페이스에 대해 요약하겠다.<br><br><br>

> 코드 해석
    
    1. stream()으로 해당 배열의 스트림을 생성한다.
    2. 스트림에서 name과 이름이 같은 요소들을 필터링한다.
    3. 필터링한 값 중 가장 처음 값을 Optional로 리턴한다.
    4. 스트림은 1회용이므로 사용 후 사라진다.
    
> Optional
```
  개발을 할 때 가장 많이 발생하는 예외 중 하나가 바로 NPE (NullPointerException) 다.
  
  NPE를 피하기 위해서는 null을 검사하는 로직을 추가해야 하는데, null 검사를 해야하는 변수가 많은 경우 코드가 복잡해지고 로직이 상당히 번거롭다.
  그렇기 때문에 null 대신 초기값을 사용하길 권장하기도 한다.
  Java8에서는 Optional<T> 클래스를 사용해 NPE를 방지할 수 있도록 도와준다. 
  Optional<T>는 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE가 발생하지 않도록 도와준다.
 
  ** 메소드의 반환 값이 절대 null이 아니라면 Optional을 사용하지 않는 것이 성능저하가 적다. 
  Optional은 메소드의 결과가 null이 될 수 있으며, 클라이언트가 이 상황을 처리해야 할 때 사용하는 것이 좋다. **
````
    
<br>
    
> 1. 함수형 프로그래밍 (Lambda가 나온 이유)
<br>

 <h3> 프로그래밍 패러다임 </h3>
 
 프로그래밍 패러다임이란 프로그래머에게 프로그래밍에 대한 관점을 가지게 하고 코드의 작성 방법을 결정하는 역할을 함.
 
 1. 명령형 프로그래밍 : 무엇(What)을 할 것인지를 나타내기보다는 어떻게(How)에 초점
 - 절차지향 프로그래밍 : 수행되어야 할 순차적인 처리 과정을 포함
 - 객체지향 프로그래밍 : 객체들의 집합으로 프로그래밍의 상호작용 표현


  2. 선언형 프로그래밍 : 어떻게(How)보다 무엇(What)을 할 것인지를 설명
  - 함수형 프로그래밍 : 순수 함수를 조합하고 소프트웨어를 만드는 방식

<br>
<h3> 함수형 프로그래밍 </h3>

함수형 프로그래밍 : 거의 모든 것을 '순수 함수'로 나누어 문제를 해결하는 기법

작은 문제를 해결하기 위한 함수를 작성하여 가독성을 높이고 유지보수를 용이하게 함.

클린 코드(Clean code)의 저자는 함수형 프로그래밍을 대입문이 없는 프로그래밍으로 정의.


명령형 프로그래밍에서는 메소드를 호출하면 상황에 따라 내부의 값이 바뀔 수 있음. 

하지만 함수형 프로그래밍에서는 대입문이 없기에 메모리에 한 번 할당된 값은 새로운 값으로 변하지 않음 (Java에서는 임시로 사용되는 Stream으로 구현)

Java는 대표적인 객체지향 프로그래밍 언어로, 함수형으로 개발하기 위해서는 별도의 도구가 필요하고, JDK8부터 Stream API와 함수형 인터페이스 등을 제공한다.



<br>
    
> 2. Stream API란
<br>

 <h4> Stream API의 특징 </h4>

1. 원본의 데이터를 변경하지 않는다.
2. 일회용이다.
3. 내부 반복으로 작업을 처리한다.

```
1 -> 원본의 데어터를 조회하여 원본의 데이터가 아닌 별도의 요소들로 Stream을 생성한다. 데이터를 읽기만 할 뿐이며, 정렬이나 필터링 등의 작업은 별도의 Stream에서 작업

2 -> 한번 사용이 끝나면 재사용이 불가능하다. 만약 닫힌 Stream의 사용한다면 IllegalStateException 발생

3 -> for나 while이 아닌 내부 반복을 사용함으로써 코드가 간결해진다. 반복 문법을 메소드 내부에서 숨기고 있다.
```



 <h4> Stream API의 연산 종류 </h4>

1. 생성하기 - Stream 객체 생성
2. 가공하기 - 원본의 데이터를 데이터로 가공하기 위한 중간 연산
3. 결과 만들기 - 가공된 데이터들로부터 원하는 결과를 만들기 위한 최종 연산


<br>
    
> 3. 람다식(Lambda Expression)이란?
<br>

람다식은 함수를 하나의 식으로 표현한 것이다. 

함수를 람다식으로 표현하면 메소드의 이름이 필요 없기 때문에 익명 함수라고도 할 수 있다.

```
//기존방식
반환타입 메소드명 (매개변수, ..){
  실행문
}

//람다 방식
(매개변수, ...) -> { 실행문 ... }
```

-> 불필요한 코드를 줄이고, 가독성을 높인다.

<br>

```
람다식 특징 :
람다식 내에서 사용되는 지역변수는 final이 붙지 않아도 상수로 간주된다.
람다식으로 선언된 변수명은 다른 변수명과 중복될 수 없다.
 

람다식 장점 :

코드를 간결하게 만들 수 있다.
식에 개발자의 의도가 명확히 드러나 가독성이 높아진다.
함수를 만드는 과정없이 한번에 처리할 수 있어 생산성이 높아진다.
병렬프로그래밍이 용이하다.

 
람다식 단점 :
람다를 사용하면서 만든 무명함수는 재사용이 불가능하다.
디버깅이 어렵다.
람다를 남발하면 비슷한 함수가 중복 생성되어 코드가 지저분해질 수 있다.
재귀로 만들경우에 부적합하다.
```


> 4. 함수형 인터페이스(Functional Interface)란?

함수형 인터페이스란 함수를 1급 객체처럼 다룰 수 있게 해주는 어노테이션으로, 인터페이스에 선언하여 단 하나의 추상 메소드만을 갖도록 제한하는 역할

*1급 객체*

아래 3 가지조건을 충족한다면 1급 객체라고 할수 있다.

1. 변수나 데이타에 할당 할 수 있어야 한다.
2. 객체의 인자로 넘길 수 있어야 한다.
3. 객체의 리턴값으로 리턴 할수 있어야 한다.
 
```

주로 사용하고 있는 언어인 JAVA에서는, 함수가 1급 객체에 해당하지 않는다. Kotlin, JavaScript 등의 언어에서는 변수에 함수를 할당하고 사용할 수 있다. 하지만, JAVA는 불가능하다.
그렇지만, JAVA의 Lambda는 그렇다면 어떻게 되는것인가?라는 질문을 던질 수 있다.
이는 메서드가 1개만 존재하는 인터페이스/클래스를 통해, 마치 함수를 전달하는 것처럼 여겨서, 함수를 1급 객체로 취급하지 않는 JAVA의 단점을 어느정도나마 해결한 것이라고 볼 수 있다.
 
 ```

[ Java에서 제공하는 함수형 인터페이스 ]
Java에는 자주 사용될 것 같은 함수형 인터페이스가 이미 정의되어 있으며, 총 4가지 함수형 인터페이스를 지원하고 있다.

```
Supplier<T>
Consumer<T>
Function<T, R>
Predicate<T>

각 인터페이스들의 예제 -> https://mangkyu.tistory.com/113 [MangKyu's Diary]
  ```


  > 5. Stream API 활용 및 사용법
  
  
  1. Stream 생성
  
  1) Collections에서는 stream()이 정의되어 있음
  
  ```
  List<String> list = Arrays.asList("a", "b", "c"); 
  Stream<String> listStream = list.stream();
```
  
  2) 배열 - Stream의 of 메소드 또는 Arrays의 stream 메소드를 사용  
  ```
  // 배열로부터 스트림을 생성 
  Stream<String> stream = Stream.of("a", "b", "c"); //가변인자 
  Stream<String> stream = Stream.of(new String[] {"a", "b", "c"}); 
  Stream<String> stream = Arrays.stream(new String[] {"a", "b", "c"}); 
  Stream<String> stream = Arrays.stream(new String[] {"a", "b", "c"}, 0, 3); //end범위 포함 x
  ```
  
  2. Stream 가공
  ```
  Filter - 필터링
  Map - 데이터 변환
  Sorted - 정렬
  Distinct - 중복 제거
  Peek - 특정 연산 수행 (결과에 영향 주지 않음. ex. 스트림 연산들 중간에 출력하고 싶을 때)
  ```

  3. 최종 연산
  ```
  최댓값/최솟값/총합/평균/갯수 - Max/Min/Sum/Average/Count
  데이터 수집 - collect
  조건 검사 - Match
  특정 연산 수행 - forEach

  사용 방법 : https://mangkyu.tistory.com/114 [MangKyu's Diary]
```

    
    
<br>
> Reference
<br><br>

https://mangkyu.tistory.com/70 (Optional)

https://mangkyu.tistory.com/112 (Stream API에 대한 이해 (1/5))

https://mangkyu.tistory.com/113 (람다식(Lambda Expression)과 함수형 인터페이스(Functional Interface) (2/5))

https://mangkyu.tistory.com/114 (Stream API의 활용 및 사용법 - 기초 (3/5))

https://mangkyu.tistory.com/115 (Stream API의 활용 및 사용법 - 고급 (4/5))

https://mangkyu.tistory.com/116 (Stream API 연습문제 풀이 (5/5))

https://mangkyu.tistory.com/111 (함수형 프로그래밍이란?)

 https://developer-cheol.tistory.com/42 (1급 객체)

