---
type: doc
layout: reference
category: Basics
title: Coding Conventions
---

 

# Coding Conventions ( 코딩 규칙 )

이 페이지는 kotlin 언어에 대한 현재의 코딩 스타일이 포함되어 있습니다.


## Naming Style

확실하지 않은 경우 java코딩 규칙의 예)


* camelCase (카멜 케이스) 를 사용하여 정한 이름 (이름을 정할때 underscore를 피하세요.)

* 첫글자는 대문자로 시작하는 유형

* 메서드와 속성은 소문자로 시작하는 유형

* 띄어쓰기 4 만큼의 공간 들여 쓰기

*public 함수들은 kotlin문서에 표시되도록 문서가 있어야합니다.

 

## Colon (콜론)

콜론 앞에 공백이 있어서 타입과 super타입을 구분하고 콜론은 인스턴스와 타입을나눌때는 공백을 가지지 않습니다.

```kotlin

interface Foo<out T: Any>: Bar {

    fun foo(a: Int):T

}
```



## Lambdas (람다)

람다 표현식에서 공백은 중괄호 주위에 사용해야하고, 매개 변수를 구분하는 화살표 주변과 body에서 공백을 사용해야 합니다. 가능한 한 람다는 괄호의 밖에 전달되어야합니다.

``` kotlin
list.filter { it > 10 }.map { element -> element * 2 }
```

 짧고 중첩없는 람다에서, 매개 변수를 선언하는 대신`it` 규칙을 사용하는 것이 좋습니다. 

 명시적으로, 매개변수를 사용한 중첩된 람다에서, 매개 변수는 항상 명시적으로 선언해야합니다.

   

  ## Unit

함수가 Unit을 반환하는 경우 반환 형식은 생략합니다.

``` kotlin
fun foo() { 
    // ": Unit" is omitted here
  }
```



  ## Functions vs Properties 

arguments가 없는 functions의 경우에는 읽기 전용 (read-only) 속성과 호환 가능 합니다. 

의미가 비슷하지만, 다른 하나를 선호하는 경우에 대한 몇 가지 `문체 규칙`이 있습니다.



기본 알고리즘이 다음과 같은 경우 function에 비해 property를 선호 합니다.

* does not throw ( Exception을 던지지 않습니다.)
* has a `O(1)` complexity. ( `O(1)`복잡도를 가집니다.)
* is cheap to calculate (or caсhed on the first run) - *호출과 동일한 결과를 반환합니다.
* returns the same result over invocations ( 호출과 동일한 결과를 반환합니다.)