# Null safety in Kotlin 

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



#### [Nothing](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-nothing.html)

- Javadoc 설명 : Nothing has no instances. You can use Nothing to represent “a value that never exists”: for example, if a function has the return type of Nothing, it means that it never returns (always throws an exception).
- 함수가 명시적으로 return이 존재하지 않는다고 표기하기위한 class. 한마디로 종료되지 않는 blocking 함수 같은 것. 이것을 종료하려면 `exception` 만이 가능

```
  //일반적인 return 값이 없는 void 함수
  fun noReturn() {
    println("This method returns nothing.")
  }

  //return이 존재하지 않는다고 선언한 함수....
  fun noReturn() : Nothing {
    println("This method never returns.")
  }

```

- 위의 Nothing 반환 함수는 Nothing type의 값을 return 하라고 error가 발생한다. 하지만 목적은 return을 안하는 것이기 때문에 선언 시에 생각했던 로직이 변경되었거나 처음 디자인과는 다르게 잘못 만들어진 부분이 있는 것이니 return Nothing 선언을 변경하거나 코드 수정을 해야한다. 얼마나 유용할지는 아직 코알못이라 잘 모르겠네.