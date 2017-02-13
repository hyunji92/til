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
val size = temp?.length ?:0
```

### Elvis Operator를 사용하여 NullPointException

```kotiln
var temp: String? = null
val size = temp?.length ?: throw NullPointException("temp is null")
```



