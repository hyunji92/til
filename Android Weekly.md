## Android Weekly

* ViewModel - 이상적으로, ViewModel은 Android에 대해 아무 것도 알지못함.

  ​			이렇게하면 테스트 가능성, 누출 방지 및 모듈성이 향상. 

  ​			조건문, 루프 및 일반적인 결정은 ViewModel 또는 앱의 다른 레이어에서 수행되어야하며 

  ​			액티비티 또는 Fragments 에는 적용되지 않아야합니다.

  Views should only know how to display data and send user events to the ViewModel (or Presenter). This is called the [*Passive* *View*](https://martinfowler.com/eaaDev/PassiveScreen.html)pattern.

* ​

 