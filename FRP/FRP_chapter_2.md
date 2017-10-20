# chapter 2

> - Stream 과 Cell 타입
> - 기본연산 : map, merge , hold, snapshot, filter, lift , never, constant
> - StreamLoop 와 CellLoop를 통한 전방 참조
> - hold와 snapshot을 사용해 누적기 만들기 



## FRP의 핵심

FRP가 "시간에 따라 변하는 값"을 표현 하다는 것은 FRP개념을 접해본 사람들이라면 한번쯤 읽어봤을 문단일 것이다. 전통적인 동적인 값을 간접적으로 상태와 값의 변화를 통해 표현해 왔다. 

또 명령형 패러다임은 시간에 대해 이산적이 표현이 없는데 이는 값의 완전한 이력에 대한 표현이없기 때문이다. 개별적으로만 변한값을 캐치할 수 있다. 반면, FRP는 이런 값의 변화를 직접 잡아내고 연속적으로 변하는 값에 대한 이력을 알 수 있다.



## 2.1 스트림 타입 

Cell:셀 - 시간에 따라 변하는 값

Stream:스트림 - 이벤트의 흐름을 표현 



> DEFINITION 

**이벤트** - 프로그램의 한 부분에서 다른 부분으로 비동기적으로 메세지를 전달하는것 .
**스트림** - 개별 이벤트 스트림. 다른 FRP 시스템에서는 `이벤트 스트림` , `관찰자` , `시그널` 이라고도 부른다.  어떤 이벤트가 스트림을 통해 전달 되는 경우 이를 일컬어 스트림을 내보낸다 한다.

####java code

```java
 transactions.stream().filter((Transaction t) -> t.getYear() == 2011) // Stream<Transaction>
                .map(t -> t.getTrader().getName()) // Stream<String>
                .distinct()
                .map(String::length) // Stream<Inteager>
                .collect(Collectors.toList()); // Stream 을 List로 List<String>
```



#### kotlin code

```kotlin
transactions.stream().filter { t: Transaction -> t.year == 2011 } // Stream<Transaction>
            .map { t -> t.trader.name } // Stream<String>
            .distinct()
            .map<Int>({ it.length }) // Stream<Inteager>
            .toList()// Stream 을 List로 List<String>
```

비슷…. 다른점은 tolist 정도???



#### kotlin code  `to list` 내부 코드

```kotlin
/**
 * Returns a [List] containing all elements produced by this stream.
 */
@SinceKotlin("1.1")
public fun <T> Stream<T>.toList(): List<T> = collect(Collectors.toList<T>())
```

