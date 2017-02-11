# 9XD - 01 Anrdoid Developer Summit

> 2017 - 02 - 11 

## RxJava Basic

> 어떤 상황에서 Rx를 잘 쓸 수 있을지 알아보자.

1. observable - 하나, 여러개의 데이터를 처리할 수 있는 데이터 스트림
2. subsctiber - Observer 와Subscription 의 구현체 ( 이밴트를 계속 관찰 하고있는 Observer )
3. operator - Observable이 원하는 형태의 테이터를 내보 낼 수 있게 Transform 하고 데이터의 흐름을Compose하게 해주는 것. 
4. schedule -  Observable에 Multi threading을 적용할 수 있도록 도와주는것.

SubscbeOn - 어떤순간에 옵저버가 정의되있던간에  정의된 순간 부터의 스레드가 돌아간다. 스트림에 하나만 하는게 좋다.

AsyncTask 익명 클래스를 중간에 하나 만들어야해서 다른 클래스의 멤버변수나 함수에 접근할 일이 많아져 메모리 릭이 발생할 경우가 많이 생기는 단점이 있다.

api호출할때 Rx를 많이 쓴다.

## Rx만이 할 수 있는건 뭘까

비동기, 이벤트 기반의 프로그램들을 Observable sequences 을 사용하여 프로그래밍 할 수 있다.

- 안드로이드에서 사용할 때는 - Callback function -> hell
- functional operator들을 사용하여 reactive programming
- Rx 는 Event -based로 프로그래밍 하는것
- Ansync해결 방법 -> callback, Promise, Async/await, generator function, Observable

## Rx 사용할 수 있는 기능

- pull model

- Push model

  ```java
  //Rx model
  val observable = Obaerverable.range(1,5)
  observable.subsribe(::println)
    // 한종류의 데이터 타입만 사용하고 있다 int
    
  ```

- Composing Observables - Multiple Observable 을 사용이 큰 장점  - 서버에 두개의 정보를 요청하고 먼저온 리스펀스부터 보여준다,

```java
val observer1 = request1()
val observer2 = request2()
  
  Observable.amb(observer1, observer2)
  			.subscribe(::printin)
```

Immutable state를 사용하고,  Flexible한 구조로 해결가능.

1. 유연하게 설계를 할 수 있는 Push Model?
2. 그로인해 가능해지는 Composing Observable ?



