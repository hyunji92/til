

# 3. 함수 정의와 호출

- `kotlin` 의 collection, List 인터페이스에는 자료를 조회하는 함수만 포함되어 자료가 한번 할당되면 수정이 불가.

  ```kotlin
  //자료 수정 불가능
  fun immutable(): List<String>(){
    //...
  }
  //자료 수정 가능
  fun mutable(): MutableList<String>(){
    //...
  }

  /*
  위의 두 함수가 반환하는 타입이 모두 자바의 List(java.util.list)로 반환된다.
  */
  ```

- `kotlin` collection 메서드 종류

  `listOf()= List`, `arrayListOf() = ArrayList`, `setOf() = Set` ….

  ```kotlin
  /** Returns a new read-only list of given elements.  The returned list is serializable (JVM). */
  public fun <T> listOf(vararg elements: T): List<T> = if (elements.size > 0) elements.asList() else emptyList()


  /** Returns a new [MutableList] with the given elements. */
  public fun <T> mutableListOf(vararg elements: T): MutableList<T>
          = if (elements.size == 0) ArrayList() else ArrayList(ArrayAsCollection(elements, isVarargs = true))
  ```

- `class` 

  ```koltin
  class foo {
    // 접근 제한자를 지정하지 않는 경우 default -> `public` 
  }
  ```

- `new`키워드 없음

  ```kotlin
  //java
  Foo foo = new Foo();

  //kotlin
  val foo: Foo = Foo()
  ```

## Property

Java 에서는 클래스 내에 자료를 저장하고 접근하기 위해 field와 메서드를 사용한다. - > get(), set()  method

= 필드가 추가될수록 필요한 메서드가 배로 늘어나게 된다.

- `kotlin  property` 

  - property는 자료를 저장할 수 있는 필드오 getter/setter메서드를 함께 제공 

  ```kotlin
  //java
  Context context = getContext();

  //kotlin
  val context = baseContext
  ```


- kotlin default paramate 값
- @JvmOverloads
- 최상위 함수 선언
- 최상위 프로퍼티
- Extention Function
- Extention property
- 컬렉션 처리 : 가변 길이 인자, 중위 함수 호출, 라이브러리 지원
- kotlin `pair()` `to()`
- kotlin `local function
- ​