# 클래스간의 관계 결정



> 포함관계

```java
class Circle {
  	point c = new point();
  	int r;
}
```

> 상속관계

```java
class Circle extends Point {
  	int r;
}
```



두 경우를 비교해 볼때 Circle클래스를 작성하는데 Point클래스를 `포함` 시키거나  `상속`시키는 것에 별차이가 없어보인다. 하지만 어떻게 사용하는 것이 더 적절한지 문맥상으로 확인해 본다면

- circle은 point이다. = Circle is a Point. -> 상속관계
- circle은  point를 가지고있다. = Circle has a Point. -> 포함관계

문맥상 두번째 문장이 더 말이 된다.  Circle 과 Point는 포함관계가 더 적절(?) 하다. - `extends`

예를 더 들면 , Car클래스와 SportsCar클래스가있으면 `SportsCar는 Car이다.` 문자이 더 자연스럽다 ! 그렇다면  Car클래스를 조상 클래스로 `implements` 하는것이 더 맞겠지!

