## kotlin - apply, let, also, run



### let

```kotlin
fun<T, R> T.let(block: (T) -> R): R
// Calls the specified function block with `this` value as its `argument` and returns its `result`.
// let()을 Safe Calls - `?.`과 함께 사용하면  if( null != obj).. 를 대체할 수 있다.
```



###apply

```kotlin
fun<T> T.apply(block: T.() -> Unit):T
// apply()는 함수를 호출하는 객체를 이어지는 블록의 리시버로 전달 -> 객체 자체를 반환
// TODO: 리시버란 바로 이러지는 블록 내에서 메서드 및 속성에 바로 접근할 수 있도록 할 객체를 의미.
```

- 새로운 객체 생성과 동시에 연속된 작업 

### run

```kotlin
// run()함수는 인자가 없는 익명함수처럼 동작하는 형태/ 객체에서 호출하는 형태 두가지가 있다
// 객체 없이 run()함수를 사용하면 익명할 수처럼 사용가능 
// - 이어지는 블럭 내에서 처리할 작업들을 넣어주거나, 일반 함수와 마찬가지로 값을 반환 하지않거나
// - Unit 특정 값을 반환 할 수 있다.
fun<R> run(block: () -> R): R
// 객체에서 run()함수를 호출하면, 호출하는 객체를 이어지는 블록의 리시버로 전달하고 블록의 결과값을 반환
fun<T, R> T.run(block: T.() -> R): R

```

- 이미 생성된 객체에 연속된 작업 필요할 때



### with

```kotlin
fun <T, R> with(receiver: T, block: T.() -> R):Rㅜ
```

