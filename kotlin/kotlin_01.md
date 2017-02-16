#Kotlin

> 공부하다가 기억 해야할 것들 정리해 본다.



### Lazy호출 시점

- init
- onCreate
- setSupportActionBar
- lazy call



### NULL 처리 주의 사항

- NULL 을 하기 위한 `?` 에는  type를 써야한다.

  ```kotiln
  // null type error
  	var temp = null
  	temp = "ABC" 

  // 알맞은 사용법
  	var temp: string? = null
  	temp = "APC"
  ```

  ​

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



### 생성자 기본

```java
//java example
public class Constructor{
  public Constructor(String name) {
  this.name = name;
  }
}
```

```kotlin
//kotlin example
class Constructor(val name: String) {
  fun get(): String = name
}
```



### 다중생성자

> java Example

```kotlin
//java Example
public class MultiConstructor {
  private String name;
  private int age;
  
  public MultiConstructor(Stting name){
    this.name = name;
    this.age = 0;
  }
  public MultiConstructor(String name, int age){
    this(name);
    this.age = age;
  }
}
```

> Kotlin Example

```kotlin
class MultiConstructor(val name: String, val age: Int, var temp: Int){
  var n: String = ""
  // 초기에 위에서 사용한 변수들을 모두 정의해 놓고 사용해야한다.
  // 두번째 생성자
  constructor(name: String) : this(name, 0, 0)
  
  fun get() = temp
  
  constructor(name: String, age: Int, n: String) :  this(name, age, 0){
    this.n = n
  }
}
```

> default를 통한 초기화

```kotlin
class DefaultCoonstructor(val name:String, val age: Int = 0)
```

> 생성자에서의 초기화

```kotlin
class Constructor{
  class initClass(){
    //var name: Strring = "InitClass"
    var name: String
    
    //생성자인 Constructor에서는 초기화 하지못하고 init에서가능
    init{
      name = "InitCalss"
    }
    
    //밑에처럼 초기화 해야 name의 값이 변경됨
    constructor(name: String) : this() {
      this.name = name
    }
    class Constructorinit(val name: String){
      //...
    }
  }
}
```

