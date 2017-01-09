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
    }

    public Post(Date publishDate) {
        this(publishDate, new Date());
    }

    public Post(Date publishDate, Date writeDate) {
        this(publishDate, writeDate, new Date(), 0);
    }

    public Post(Date publishDate, Date writeDate, Date updateDate, int views) {
        this.publishDate = publishDate;
        this.writeDate = writeDate;
        this.updateDate = updateDate;
        this.views = views;
    }
}
```

