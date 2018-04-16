# RxJava - Backpressure 

> 배압은 본질적으로 소비자가 생산자에게 피드백을 주는 채널이다. 소비자는 언제든지 데이터 처리량을 일정 수준으로 제어할 수준.



# Backpressure의 필요성을 피하기 위해 유용한 operator들

Observable과 Observer를 동기화하는 경우에 소비자가 부하를 견디지 못하면 생산자를 차단한다. 이는 독립적이어야하는 생산자와 소비자 사이가 결합되는 상황이다. 배압은 소비자가 한번에 소비할 수 있는 만큼 데이터를 요청하도록 허용하는 간단한 프로토콜이며 실질적으로는 생산자에게 피드백 채널을 제공한다.



# Sample()

Sample 연산자는 Observable을 주기적으로보고 이전 샘플링 이후 가장 최근에 방출 된 항목을 방출합니다. 일부 구현에서는 ThrottleFirst 연산자도 비슷하지만 샘플 기간에 가장 최근에 방출 된 항목은 아니지만 해당 기간 동안 방출 된 첫 번째 항목을 내 보냅니다.

![Screen Shot 2018-01-13 at 2.38.17 PM](/Users/jeonghyeonji/Desktop/Screen Shot 2018-01-13 at 2.38.17 PM.png)

## Throttling

[sample(), throttleLast(), throttleFirst()](http://reactivex.io/documentation/operators/sample.html)이나 [throttleWithTimeout(), debounce()](http://reactivex.io/documentation/operators/debounce.html)같은 operator들은 Observable이 항목을 발행하는 속도를 단속할 수 있게 한다.

다음 다이어그램들은 위에서 보여준 폭주하는 Observable에 이들 operator 각각이 사용되는 것을 보여준다.

### sample (or throttleLast)

**sample** operator는 주기적으로 시퀀스속에 손을 집어넣고(“dips”), 각 퍼내기(dip) 기간 동안에 가장 최근에 발행된 항목만을 발행한다:

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.sample.png)

```
Observable<Integer> burstySampled = bursty.sample(500, TimeUnit.MILLISECONDS);

```

### throttleFirst

**throttleFirst** operator는 **sample**과 비슷하지만 가장 최근에 발행된 항목이 아니고, 이전의 퍼내기(“dip”) 이후에 처음 발행된 항목을 발행한다:

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.throttleFirst.png)

```
Observable<Integer> burstyThrottled = bursty.throttleFirst(500, TimeUnit.MILLISECONDS);

```

### debounce (or throttleWithTimeout)

**debounce**는 원본 Observable에서 지정된 기간안에 다른 항목을 뒤따르지 않는 항목들만을 발행한다:

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.debounce.png)

```
Observable<Integer> burstyDebounced = bursty.debounce(10, TimeUnit.MILLISECONDS);

```

## Buffers와 Windows

초과-생산하는 Observable의 항목들을 모운 뒤 그들을 더 적은 빈도로, 항목들의 컬렉션(또는 Observable)으로 발행하기 위해 **buffer()**나 **window()**같은 operator를 사용할 수 있다. 그러면 느린 소비자는 각 컬렉션에서 특정 항목 하나만을 처리하지, 이들 항목들의 어떤 조합을 처리할지 또는 컬렉션의 각 항목들에서 완료되어야 할 작업 계획을 적절하게 세울지를 결정할 수 있다.

다음 다이어그램들은 위에서 본 폭주하는 Observable에 이런 operator들 각각을 어떻게 사용할 수 있는 지를 보여준다.

### buffer

예를 들면, 주기적으로 폭주 Observable의 항목들의 버퍼를 닫은 뒤 발행할 수 있다.

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.buffer2.png)

```
Observable<List<Integer>> burstyBuffered = bursty.buffer(500, TimeUnit.MILLISECONDS);

```

아니면 화려하게 갈 수도 있다. 버퍼 폐쇠 지시자를 buffer operator로 발행하기 위해 debounce operator를 사용하여, 폭주 기간동안 항목들을 버퍼에 모우고 각 폭주의 끝에서 그것들을 발행한다:

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.buffer1.png)

```
// 원본 폭주 Observable을 멀티케스트하므로 이것을 소스로 사용할 수도 있고 
// 버퍼 폐쇠 선택자의 소스로도 사용할 수 있다:
Observable<Integer> burstyMulticast = bursty.publish().refCount();
// burstyDebounced은 우리의 버퍼 폐쇠 선택자가 될 것이다:
Observable<Integer> burstyDebounced = burstMulticast.debounce(10, TimeUnit.MILLISECONDS);
// 이것이 최종적으로 우리가 관심을 가지는 버퍼의 Observable이다:
Observable<List<Integer>> burstyBuffered = burstyMulticast.buffer(burstyDebounced);

```

### window

**window**는 **buffer**와 비슷하다. **window**의 한 변형은 주기적으로 규칙적 시간 간격동안의 항목들의 Observable window들을 발행할 수 있게 해준다:

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.window1.png)

```
Observable<Observable<Integer>> burstyWindowed = bursty.window(500, TimeUnit.MILLISECONDS);

```

원본 Observable에서의 특정 갯수의 항목들을 모을 때 마다 새로운 window를 발행하는 것을 선택할 수 도 있다:

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.window2.png)

```
Observable<Observable<Integer>> burstyWindowed = bursty.window(5);

```

# Backpressure의 대안으로 흐름-제어로 호출 스택 막기

과대생산하는 Observable을 처리하는 다른 방법은 호출 스택을 막는 것이다(과대생산 Observable을 지배하는 스레드를 보류). Rx의 “반응성”과 비차단(non-blocking) 모델에 위배되는 문제를 가진다. 하지만 문제적 Observable이 안전하게 차단될 수 있다면 실행 가능한 선택권이 될 수 있다. 현재 RxJava는 어떤 operator도 이것이 가능하게 한 적이 없다.

만약 Observabler과 여기에 작동하는 모든 operator들과 여기에 구독되는 모든 observer들이 모두 동일한 스레드에서 작동한다면, 호출 스택 블록킹의 도움으로 backpressure의 한 형태를 효율적으로 수립할 수 있다. 하지 많은 Observable의 operator들은 기본적으로 별개의 스레드에서 작동됨을 알고 있어야 한다(이 operator들의 Javadocs은 이를 나타낼 것이다).

## Subscriber가 “reactive pull” backpressure를 설정하는 방법

**Observable**를 **Subscriber**로 구독할 때, **Subscriber**의 **onStart()** 메소드에서 **Subscriber.request(n)**을 호출하여 reactive pull backpressure를 요청할 수 있다(n은 다음 **request()** 호출 이전에 Observable이 발행하길 원하는 항목의 최대값이다).

그리고, 이 항목(또는 항목들)을 **onNext()**에서 처리한 다음, 그 **Observable**에게 다른 항목(또는 항목들)을 발행하도록 지시하기 위해 다시 **request()**를 호출 할 수 있다. 여기 **Subscriber**가 someObservable에게 한 번에 항목 하나를 요청하는 예제가 있다:

```
someObservable.subscribe(new Subscriber<t>() {
    @Override
    public void onStart() {
      request(1);
    }

    @Override
    public void onCompleted() {
      // 우아하게 시퀀스-종료 처리
    }

    @Override
    public void onError(Throwable e) {
      // 우아하게 오류 처리
    }

    @Override
    public void onNext(t n) {
      // 발행된 항목 "n"으로 무엇인 가를 한다.
      // 다른 항목을 요청한다:
      request(1);
    }
});

```

reactive pull backpressure를 작동하지 않게 하고 Observable이 자신의 속도로 항목들을 발행하도록 하기 위해 매직 넘버인 **request(Long.MAX_VALUE)**를 **request**에 전달할 수 있다. **request(0)**는 적법하지만 효과는 없다. 0보다 작은 값을 **request*에 전달하면 예외 발생의 원인이 될 것이다.

## Reactive pull backpressure isn’t magic

Backpressure은 과대생산 Observable이나 느린 소비 Subscriber의 문제를 해결해 주지 않는다. operator 체인의 문제를 더 잘 처리 할 수 있는 지점으로 옮기는 것 뿐이다.

이제 고저가 있는 [zip](http://reactivex.io/documentation/operators/zip.html)의 문제를 자세히 살펴보자.

우리는 A와 B 두 Observable을 가지고 있다. B는 A보다 항목을 더 자주 발행하는 경향이 있다. 이 두 개의 Observable을 **zip** 할 때, **zip** operator는 A의 항목 n과 B의 항목 n을 결합한다. 그런데 그동안 B이 n+1에서 n+m의 항목을 이미 발행하였다. **zip** operator는 그 항목들을 유지해야 하며, 그래서 그것들을 A의 n+1부터 n+m까지의 항목들과 결합할 수 있다. 하지만 그동안 m은 계속 커지며 이 항목들을 유지하기 위한 버퍼의 크기는 계속 증가한다.

당신은 B에 throttling을 추가할 수 있다. 하지만 이는 B가 발행하는 일부 항목들을 무시한다는 의미일 것이며, 적절하지 않을 수도 있다. 당신이 진정으로 원하는 것은 B에게 속도를 줄일 필요가 있다는 신호을 보내는 것이다. 그리고 B가 발행의 무결성을 유지하는 방식으로 어떻게 이를 할 것인지를 결정하게 한다.

Reactive pull backpressure 모델은 당신이 이를 하도록 한다. 이것은 일반적인 수동적 push Observable 행동과 대조적으로 Subscriber에서 일종의 능동적 pull을 만든다.

RxJava에서 구현된 **zip** operator는 이 기술을 사용한다. 이것은 원본 Observable을 위해 항목들의 작은 버퍼를 유지한다. 그리고 이것는 자신의 버퍼가 가득차게 될 것이면 각 원본 Observable에서 더 이상의 항목을 요구하지 않는다.

(많은 RxJava operator들은 reactive pull backpressure를 수행한다. 어떤 operator들은 그들이 동작하는 Observable과 동일한 스레드에서 작업을 하며, 단순히 이전 것의 처리를 종료하기 전까지 다른 항목을 발행할 기회를 Observable에게 주지 않는 것으로 블로킹의 한 형태를 행사하므로, 이런 다양한 종류의 backpressure을 필요로 하지 않는다. 다른 operator에은 다른 방식의 흐름 제어를 처리하기 위해 명시적으로 디자인되었기 때문에 backpressure은 부적절하다. Observable 클래스의 메소드인 이들 operator들의 RxJava javadoc는 어느 것이 reative pull backpressure를 사용하지 않는지와 그 이유를 표시한다.)

이 작업을 수행하려면, Observable A와 B가 **request()**에 올바르게 반응해야 한다. 만약 Observable이 reative pull backpressure를 지원하도록 작성되지 않았다면(그러한 지원은 Observable의 필요 조건이 아니다), backpressure의 단순한 형태를 강제하는 다음의 operator들 중 하나를 적용 할 수 있다:

**onBackpressureBuffer**

원본 Observable에서의 모든 발행의 buffer를 유지하고 하류(downstream) Subscriber들에게로 그들이 일으키는 **request**들에 따라 그것들을 발행한다.

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.obp.buffer.png)

이 operator의 실험적 버전(RxJava 1.0에서는 지원하지 않는다)은 버퍼의 용량을 설정할 수 있게 해준다; 이 operator의 적용은 버퍼가 가득찰 때 Observable가 오류와 함께 종료되는 결과를 초래할 것이다.

**onBackpressureDrop**

하류의 Subscriber에서의 연기된 request가 없는 한 원본 Observable의 발행을 버린다. 이 경우 **request**를 만족시키기 위해 충분한 항목들을 발행할 것이다.

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.obp.drop.png)

**onBackpressureBlock (experimental, not in RxJava 1.0)**

blocks the thread on which the source Observable is operating until such time as a Subscriber issues a request for items, and then unblocks the thread only so long as there are pending requests

![img](https://github.com/ReactiveX/RxJava/wiki/images/rx-operators/bp.obp.block.png)

만약 backpressure를 지원하지 않는 Observable에 이 operator들 중 어떠한 것도 적용하지 않았고, 그리고 만약 Subscriber인 당신 또는 당신과 Observable 사이의 어떤 operator가 reactive pull backpressure를 적용하려고 시도한다면, 당신은 **MissingBackpressureException**을 당신의 **onError()** 콜백을 통해 통지받을 것이다.



## Hot and cold Observables, and multicasted Observables

**Cold** Observable은 특정한 항목들의 시퀀스를 발행하며, 자신의 Observer가 자신이 준비되었음을 알았을 때, 그 Observer가 원하는 속도(rate)대로 시퀀스의 완전성에 지장을 주지 않고 그 시퀀스를 발행할 수 있다. 예를 들어 당신이 고정된 Iterable를 Observable로 변환하면, 이 Observable은 나중에 구독되든지, 또는 얼마나 빈번하게 이 항목들이 관찰되었는지(observer)에 상관없이 동일한 항목들의 시퀀스를 발행할 것이다. Cold Observable이 발행한 항목의 전형에는 데이터베이스 질의, 파일 검색, 웹 요청의 결과들이 해당된다.

**Hot** Observable은 자신이 생성되는 즉시 발행하기 위해 항목들의 생성을 시작한다. Subscriber들은 일반적으로 hot Observable이 발행하는 항목들의 시퀀스의 중간 어딘가에서, subscription이 만들어진 이후에 Hot Observable이 발행한 첫번째 항목으로 시작하여 관찰(observing)하기 시작한다. 이런 Observable은 자신만의 속도(pace)로 항목들을 발행하며, 이를 따라가는 것은 자신의 observer의 책임이다. Hot Observable이 발행한 항목들의 전형에는 mouse와 키보드의 이벤트, 시스템 이벤트, 또는 주식 가격들이 해당된다.

Cold Observable이 multicast(**ConnectableObservable**로 변환되고 그것의 **connect()**메소드가 호출되었을 때)일 때, 사실상 *hot*이 되며 backpressure와 흐름-제어를 위해 hot Observable처럼 취급되어야 한다.

Cold Observable들은 아래에 서술된 backpressure의 reactive pull 모델에 이상적이다. Hot Observable은 일반적으로 reactive pull 모델에 잘 대처하지 못하며, [onBackpressureBuffer나 onBackPressureDrop operator](http://reactivex.io/documentation/operators/backpressure.html), throttling, buffers, 또는 windows의 사용 같은 이 페이지에서 논의된 몇몇 다른 흐름-제어 전략을 적용하는 것이 더 낫다.

