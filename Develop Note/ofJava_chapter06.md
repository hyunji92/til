# Chapter06 - 객체지향 프로그래밍

#### 3.7 JVM의 메모리구조

1. 메서드 영역 method area

   *.class파일을 읽어 분석 후 클래스테이터를 저장하는 영역이다.

2. 힙 heap

   인스턴스가 생성되는공간이다. 전역 변수, static은 힙에 생성. 프로그램 실행 중 생성되는 인스턴스는 모두 이 공간에 있다.

3. 호출 스택 call stack / execution stack

   호출스택은 메서드와 작업에 필요한 메모리 공간을 제공한다. 호출스택에 호출된 메서드를 위한 메모리가 할당. 이메모리는 메서드가 작업을 수행하는 동안 지역변수와 매개변수들과 연사의 중간결과 들을 저장하는데 사용된다. 





#### 4.1 오버로딩 Overloading

> 한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는것을 `method overloading` 이라고 한다. 
>
> 1. 메서드 이름이 같아야하고
> 2. 파라미터의 개수 또는 타입이 달라야한다.

- 가변인자 오버로딩은 `타입… 변수명` 과 같은 형식으로 서언하며 , 가변인자를 매개변수 중에서 제일 마지막에 선언해야한다. 그렇지 않으면 가변인자인지 아닌지를 구별할 수 없어 컴파일에러가 나기 때문이다.



#### 5.1 생성자 Constructor

> 인스턴스가 생성될 때 호출되는 `인스턴스 초기화 메서드`  인스턴스 변수의 초기화 작업에 주로 사용되고, 인스턴스 생성시에 실행 되어야 할 작업을 위해서 사용된다. 리턴값이 없다.

1. 생성자의 이름은 클래스의 이름과 같아야한다
2. 생성자는 리턴 값이 없다.

```java
package com.company;

import java.util.Date;


public class Main {
    public static void main(String[] args) {
        final int i;
        if(true) {
            i = 0;
        } else {
            i = 3;
        }
        //Lazy
//        System.out.print(i);

        Car car = new Car();
        car.gearType = "a";
        Car.color = "a";
        car.color = "b";

          

    }
}


class Car {
    String color = "";
    String gearType;
    int door;

    public String getColor() {
        return color;
    }


    {
      
//        color = "ffffff";
//        gearType = "eeeeee";
    }

    static {
      
    }
//
//    Car() {
//        this("white");
//    }
//
//    Car(String color) {
//        this(color, "auto");
//    }
//
//    Car(String color, String gearType) {
//        this(color, gearType, 4);
//    }
//
//    Car(String color, String gearType, int door) {
//        this.color = color;
//        this.gearType = gearType;
//        this.door = door;
//    }
}


class Post {
    Date publishDate;
    Date writeDate;
    Date updateDate;
    int views;


    public Post() {
        this(new Date());
    } // 파라미터가 없는 생성자

    public Post(Date publishDate) {
        this(publishDate, new Date());
    } // 파라미터가 있는 생성자

    public Post(Date publishDate, Date writeDate) {
        this(publishDate, writeDate, new Date(), 0); //1.이것을 호출하면 
    }

    public Post(Date publishDate, Date writeDate, Date updateDate, int views) {
      //2. this 에서 파라미터들 + view가 포함되어 초기화
        this.publishDate = publishDate;
        this.writeDate = writeDate;
        this.updateDate = updateDate;
        this.views = views;
    }
}
```





#### 5.4 생성자에서 다른 생성자 호출하기 - this(), this

>같은 클래스의 멤버들 간에 서로 호출할 수 있는 것처럼 생성자 간에도 서로 호출이 가능하다.
>
>- 생성자의 이름으로 클래스 대신 this를 사용한다.
>- 한 생성자에서 다른 생성자를 호출할 때는 반드시 첫줄에서만 호출이 가능하다.

`this`는 참초 변수로 인스턴스 자신을 가리키고, 인스턴스의 멤버에 접근할 수 있다.

하지만 `this`를 사용할 수 있는것은 인스턴스 멤버일 뿐이고, static메서드는 사용할 수 없다. 

`this` - 인스턴스 자신을 가리키는 참조변수, 인스턴스의 주소가 저장되어져있고 모든 인스턴스 메서드에서 지역변수로 숨겨진채로 존재한다

`this()`  - this(파라미터), 생성자와 같은클래스의 다른 생성자를 호출할때 사용한다.





#### 6.3 초기화 블럭 ( initialization block )

> 클래스 초기화 블럭 - 클래스 변수의 복잡한 초기화에 사용된다.
>
> 인스턴스 초기화 블럭 - 인스턴스 변수의 복잡한 초기화에 사용된다.



```java
class Car {
    String color = "";
    String gearType;
    int door;

    public String getColor() {
        return color;
    }


    {
      /*인스턴스 초기화 블럭*/
      //생성자와 같이 인스턴스를 생성할 때 마다 수행
//        color = "ffffff";
//        gearType = "eeeeee";
    }

    static {
      /*클래스 초기화 블럭*/   
      // 클래스가 메모리에 처음 로딩될 때 한번만 수행
    }
```



#### 6.4 멤버변수의 초기화 시기와 순서

```java
class Product {
    static int count =  0;
    int serialNo;
    {
        ++count;
        serialNo = count;
    }
    public Product() {}

  class ProductTest {
    public static void (String args[]){
      
      Product product1 = new Product();
        Product product2 = new Product();
        Product product3 = new Product();

        System.out.println("제품 번호 1 : " + product1.serialNo);
        System.out.println("제품 번호 2 : " + product2.serialNo);
        System.out.println("제품 번호 3 : " + product3.serialNo);
      
    }
  }
}
```

!!!! SQL디비 PK점차적으로 increments 되는것처럼 저렇게 `++count`해놓으면 일련번호가 증가하여 보인다.

생성자가 하나 밖에 없기 때문에 인스턴스 블럭 대신, Product클래스의 생성자를 사용해도 결과는 같다.



#### *응용 예제*

```java
class Document {
    static int count = 0;
    String name;
    
    Document(){
        this("제목 없음" + ++count);
    }
    
    Document(String name) {
        this.name = name;
        System.out.println("문서" + this.name +  "가 생성되었음 ");
    }
    class DcumentTest {
      public static void (Stirng args[]) {}
        Document document1 = new Document();
        Document document2 = new Document("자바.txt");
        Document document3 = new Document();
        Document document4 = new Document();
    }
  }
}
```

