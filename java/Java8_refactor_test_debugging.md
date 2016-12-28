#효과적인 Java8 Programming

##가독성과 유연성을 개선하는 리팩토링 
###1.1 코드 가독성 개선
 - 어떤 코드르 다른 사람이 보아도 쉽게 이해할 수 있음
 - 코드의 장황함을 줄여서 쉽개 이해할 수 있는 코드를 구현한다
 - 메서드 레퍼런스와 스트림 API를 이용해서 코드의 의도(코드가 무엇을 수행하려는 것인지)를 쉽게 표현할 수 있다
 
>   익명클래스 -> 람다 표현식 으로 리팩토링
    람다 표현식 -> 메서드 레퍼런스 로 리팩토링
    명령형 데이터 처리 -> 스트림 으로 리팩토링
 
 
###1.2 익명 클래스를 람다 표현식으로 리팩토링
장황한 코드와 쉽게 에러를 일으키는 익명 클래스를 람다 표현식으로 리팩토링 해본다

but,  모든 익명 클래스를 람다 표현식으로 변환 할 수 있는 것은아니다
      
 1. 익명 클래스에서의 this , super는 람다 표현식에서 다른 의미를 갖는다.     
 2. 익명클래스는 감싸고 있는 클래스의 변수를 가릴 수 있다.
 3. 콘텍스트 오버로딩에 따른 모호함이 초래 될 수 있다. 익명클래스는 인스턴스화할 때 명시적으로 형식이 정해진다
      하지만 람다의 형식은 텍스트에 따라 달라진다 
      
```java
doSomething(() -> System.out.println("Danger "));

//여기서 doSomething(Runnable R)와  doSomething(Task t) 어느 것을 가리키는지 알 수 없는 문제 방생
```

###1.3 람다 표현깃을 메서드 레퍼런스로 리팩토링

```java
Map<CaloricLevel, List<Dish>> dishesByCaloricLevel = menu.stream()
                                                     .collect(groupingBy(Dish::getCaloricLevel));
                                                        // 람다 표현식을 메서드로 구현
                                                        // Dish 클래스에 getCaloricLevel추가
```


- comparing과 maxBy같은 정적 헬퍼 메서드를 활용하는것도 좋은 방법

```java
inventory.sort(comparing(Apple::getWeght)); //  코드가 문제 자체를 설명
```

- sum, maximum등 자주이용하는 리듀싱 연산은 메서드 레퍼런스와 함께 사용할 수 있는 내장 헬퍼 메서드를 제공한다.

```java
int totalCalories = menu.stream().map(Dish::getCalories)
                                 .reduce(0, c1 -> c1 + c2);
                                 
int totalCalories = menu.stream().collect(summingInt(Dish::getCalories));
// 내장 컬렉터를 이용하면 코드 자체로 문제를 더 명확하게 설명할 수 있다

```

###1.4 명령형 데이터 처리를 스트림으로 리팩토링

>스트림 API는 데이터 처리 파이프라인의 의도를 더 명확하게 보여준다 스트림은 `쇼트서킷` 과 `게으름`
 이라는 강력한 최적화와 멀티코어 아키텍처를 활용할 수 있게 제공되어진다

- 스트림 API를 이용하면 무네를 더 직접적으로 기술 하고 병렬화 할 수 있다.


```java
// 필터링 추출, 두가지 패턴의 명령형 코드를 쉽게 구현할 수 있따
menu.parallelStream()
    .filter(d -> d.getCalories() > 100 )
    .map(Dish::getName)
    .collect(toList());
```

- 명령형 코드의 break, countinue, return 등의 제어 흐름문을 모두 분석하여 같은 기능을 수행하는
스트림 연사으로 유추하는것은 쉬운일이 아니다. ( 명령형 코드 -> 스트림 API )

###1.5 코드 유연성 개선
> 다양한 람다를 전달해서 다양한 동작을 표현할 수 있따. 람다 표현식을 이용하려면 함수형 인터페이스 적용이 필요하다. 
  
- 조건부 연기 실행 ( Conditional deferred execution )
```java
if( logger.isLoggable(Log.FINER)){
    logget.finer("Problem :" + getnerateDiagnostic());
    }
    // 위 코드의 문제
    // 1. logger상태가 isLoggerable이 라는 메서드에 의해 클라이언트 코드로 노출된다
    // 2. 메세지를 로깅 할때마다 logger객체의 상태를 매번확인하는것은 코드를 어지럽힐 뿐
```
==>

```java
logger.log(Level.FINER, "Problem : " + generateDiagnostic());
// 내부적으로 확인하는 log메서드를 사용하는것이 바람직
```
예제의 logger 수준을 FINER로 설정한 특정조건에서만 메세지가 생성될 수 있도록 메시지 생성과정을 연기, defer해야한다
java8에서 이와같은 logger문제 해결하도록 Supplier를 인수로 갖는 오버로드된 log메소드 제공

```java
public void log (Level level, Supplier<String> msgSupploer){
    if(logger.isLoggable(level)){
    log(level, msgSupplier.get()); //람다 실행
    }
   } 
```

- 실행 어라운드 
매번 같은 준비 종료 과정을 반복적으로 수행하는 코드가 있다면 이를 람다로 변환하자.
코드 중복을 줄일 수 있다. 3장예제 참조

## 람다로 객체지향 패턴 리팩토링
> 다양한 디자인 패턴은 공통적인 소프트웨어 문제를 설계할 때 재사용할 수 있는 검증된 청사진을 제공한다.
예로, 구조체와 동
