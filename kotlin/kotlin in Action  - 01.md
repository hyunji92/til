# kotlin in Action  - 01

> 2018.01.19 

### 함수형 프로그래밍 & 객체지향 프로그래밍

- 순수함수 : 함수는 주어진 입력으로 계산하는 것 이외에 프로그램의 실행에 영향을 미치지 않아야 하며, side effect가 없어야 한다

```Kotlin
fun firstFunction(a: int, b: int){
  var sum = a + b
  println(sum)
}
```

- 무명함수 - anonymous function

  ```kotlin
  fun getItemCount(): Int {
          return items.count()
      }

  //------------------------------------

  fun getItemCount() = items.count()
  fun screenName() = FirebaseAnalyticsKeys.FRAGMENT
  ```

- kotlin builder pattern - https://proandroiddev.com/dsl-builder-in-kotlin-be5816ba3ca7

  ```kotlin
  class Person(val name: String, val surname: String, val age: Int) {
      companion object {
          class Builder {
              private var name: String = ""
              private var surname: String = ""
              private var age: Int = 0

              fun name(name: String): Builder {
                  this.name = name
                  return this
              }

              fun surname(surname: String): Builder {
                  this.surname = surname
                  return this
              }

              fun age(age: Int): Builder {
                  this.age = age
                  return this
              }

              fun build() = Person(name, surname, age)
          }
      }
  }

  fun main(args: Array<String>) {
      Person.Companion
            .Builder()
            .name("Piotr")
            .surname("Slesarew")
            .age(28)
            .build()
  }
  ```

- DSL (Domain-Specific Language)

- Anko - https://academy.realm.io/kr/posts/getting-started-with-kotlin-and-anko/

- ByteCode

- findViewbyId , get(), set(), data class...

- Nullable

  ```kotlin
  val s: String? = null
  val ss: String = ""

  fun string(resid: Int): String = sContext!!.getString(resid)
  ```

- https://medium.com/@kimtaesoo188/kotlin-weekly-64-kotlin-programmer-dictionary-statement-vs-expression-21f42013b0e0

- Val,var

  ```kotlin
  val == value, immutable
  var == variable, mutable
  ```

  ​