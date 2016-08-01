#Java8 익명 클래스
자바는 클래스의 선언과 인스턴스화를 동시에 수행할 수 있도록 익명 클래스(anonymous)라는 기법을 제공한다.

> 익명 클래스는 자바의 지역 클래스 (Local class ,  블록 내부에 선언된 클래스)와 비슷한 개념이다. 말 그대로 이름이 없는 클래스 이다. 익명 클래스를 이용하면 클래스 선언과 인스턴스화를 동시에 할 수 있다. 즉, 즉석에서 필요한 구현을 만들어서 사용할 수 있다.

1. 아래 코드처럼 익명 클래스는 여전히 많은 공간을 차지한다.

    ```java
    List<Apple> redApple =  fulterApple (inventory , new Apple Predicate()){ //반복
        public boolean test(Apple a ){
            return "red".equals(a.getColor());
        }
    });
    button.setOnActioin (new EventHandler<ActionEvent>(){ //반복
        public void handle(ActionEvent event){
            System.out.printIn(" a click ");
        }
    }
    ```
    ]
2. 많은 프로그래머들이 아직 익명클래스 사용에 익숙 하지 않다. 
	익명클래스로 인터페이스를 구현하는 여러 클래스를 선언하는 과정을 조금 줄일 숭 있지만 여전히 만족스럽지는 않다. 코드조각을 전달하는 과정에서 결국은 객체를 만들고 명시적으로 새로운 동작을 정의하는 메서드를 구현해야한다는 점은 변하지않는다 .
    ex) predicate의 test메서드나 EventHandler 의 handler 메서드 
	


