# kotlin in Action 06

###Null safety in Kotlin

NULL 을 하기 위한 `?` 에는  type를 써야한다.

```kotiln
// null type error
	var temp = null
	temp = "ABC" 

// 알맞은 사용법
	var temp: string? = null
	temp = "APC"
```

- kotlin 에서 기본적으로 선언된 변수는 모두 null이 허용되지 않는다.
- null을 허용하려면 명시적으로 nullable하다는 것을 명시해 줘야한다

```java
Testtext testtext = null;
testtext.toString();

//100% NullPointException 발생
```

kotlin에서는 `?`를 사용하여 null 입력이 작성 가능한 `modern languages!!!`

```kotlin
val a: Int? = null
a.toLong() // null 입력이 가능한 즉, nullable한 integer 사용가능
```

위의 예제는 변수 `a`가 null일 수 있습니다. 하지만 kotlin이기 때문에 `smart cast` 를 사용하여 `non-nullable` type으로 바꿔 줍니다. 위의 예제는 사실상 

```kotlin
val a Int? = null
//...
if(a != null) {
  a.toLong()  // if 문이 숨어져 처리된다고 보시면 됩니다.
}
```

대신에 이는 변수가 동시에 수정 될 수 없는 경우에만 작동합니다. 안그러면 값이 다른 스레드에 의해 변경될 수 있고, 그 시점에서 체크를 했을때는 값이 다를 수 있기 때문이죠.  `val`  속성이나 `local`변수 일때만 가능합니다.

그리고 kotlin으로 null인지 체크는 코드를 더 단순화 하여 표현할 수 있습니다.

```kotlin
a?.toLong()
```

위의 코드는 변수가 null이 아닐 때만 실행됩니다. null이면 아무것도 수행되지 않는거!

```kotlin
val myLong = a?.toLong() ?: 0L
```

그리고 `?:`연산자를 사용하여 null인 케이스에 대한 대안값을 제공할 수도 있죠! 물론 exception던질 때도 사용 가능!!!

```kotlin
val myLong = a?.toLong() ?: throw IllegalStateException() // 짜잔
```

하지만 반대로 null이 가능한 유형을 처리하고 싶을 수도 있죠

그렇다면

```kotlin
val a :Int? = null
a!!.toLong()
```

위의 예제 처럼 `!!` 연산자로 사용할 수 있습니다. 이 코드는 컴파일 되지만 분명히 충돌할 수도 있습니다. 따라서 아주 특정한 상황에서만 사용해야 하는거죠.

(최신 안드로이드 버전은 @ Nullable과 @ Non-Nullannotations를 사용하여 null이거나 null을 반환 할 수있는 함수를 식별 할 수 있습니다.)

null 이 의심스러울때는 nullable객체를 사용하고 가능한 null처리를 할 수 있도록합니다. 

`!!` **은 객체가 null이 아닌것이 확실할때 사용해야 한다는 것을 기억하세요.**



### if/else 대신 사용하기 `?:`Elvis Operator

```kotiln
var temp: String? = null ( "123" )
val size = temp?.length ?:0 // temp가 null이 아니라면 
```

### Elvis Operator를 사용하여 NullPointException

```kotiln
var temp: String? = null
val size = temp?.length ?: throw NullPointException("temp is null")

val size =  temp!!.length
```

### null filter

```kotlin
//null filter
val intList: List<Int> =  nullableList.filterNotNull()
intList.forEach(::print)
```

