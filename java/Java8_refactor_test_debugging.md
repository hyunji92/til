#효과적인 Java8 Programming

##가독성과 유연성을 개선하는 리팩토링 
###1.1 코드 가독성 개선
 - 어떤 코드르 다른 사람이 보아도 쉽게 이해할 수 있음
 - 코드의 장황함을 줄여서 쉽개 이해할 수 있는 코드를 구현한다
 - 메서드 레퍼런스와 스트림 API를 이용해서 코드의 의도(코드가 무엇을 수행하려는 것인지)를 쉽게 표현할 수 있다
 
>   익명클래스 -> 람다 표현식 으로 리팩토링
    람다 표현식 -> 메서드 레퍼런스 로 리팩토링
    명령형 데이터 처리 -> 스트림 으로 리팩토링
 
 
###1.2 익명 클래스를 람다 표현식으로 리팩토링
장황한 코드와 쉽게 에러를 일으키는 익명 클래스를 람다 표현식으로 리팩토링 해본다

but,  모든 익명 클래스를 람다 표현식으로 변환 할 수 있는 것은아니다
      
 1. 익명 클래스에서의 this , super는 람다 표현식에서 다른 의미를 갖는다.     
 2. 익명클래스는 감싸고 있는 클래스의 변수를 가릴 수 있다.
 3. 콘텍스트 오버로딩에 따른 모호함이 초래 될 수 있다. 익명클래스는 인스턴스화할 때 명시적으로 형식이 정해진다
      하지만 람다의 형식은 텍스트에 따라 달라진다 
      
```java
doSomething(() -> System.out.println("Danger "));

//여기서 doSomething(Runnable R)와  doSomething(Task t) 어느 것을 가리키는지 알 수 없는 문제 방생
```

###1.3 람다 표현깃을 메서드 레퍼런스로 리팩토링

```java
Map<CaloricLevel, List<Dish>> dishesByCaloricLevel = menu.stream()
                                                     .collect(groupingBy(Dish::getCaloricLevel));
                                                        // 람다 표현식을 메서드로 구현
                                                        // Dish 클래스에 getCaloricLevel추가
```


- comparing과 maxBy같은 정적 헬퍼 메서드를 활용하는것도 좋은 방법

```java
inventory.sort(comparing(Apple::getWeght)); //  코드가 문제 자체를 설명
```

- sum, maximum등 자주이용하는 리듀싱 연산은 메서드 레퍼런스와 함께 사용할 수 있는 내장 헬퍼 메서드를 제공한다.

```java
int totalCalories = menu.stream().map(Dish::getCalories)
                                 .reduce(0, c1 -> c1 + c2);
                                 
int totalCalories = menu.stream().collect(summingInt(Dish::getCalories));
// 내장 컬렉터를 이용하면 코드 자체로 문제를 더 명확하게 설명할 수 있다

```

