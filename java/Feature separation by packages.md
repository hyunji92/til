# **Feature separation by packages**  ( 패키지 별 기능 분리)

Instead of using a *package by layer approach*, we have structured the application by *package per feature*

- 계층별 패키지 접근방식 대신에  기능에 의한 패키지 별로 어플리케이션을 구조화 했다.

This greatly improves readability and modularizes the app in a way that parts of it can be changed independently from each other

- 이는 가독성을 크게 향상 시키고 앱의 일부분이 서로 독립적으로 변경 될 수 있는 방식으로 앱을 모듈화 할 수 있다.

Each feature package (shown in bold above) contains a Contract interface - this defines the interface for the View and Presenter callbacks of each feature

- 각 기능 패키지에는 Contract interface 가 들어 있다. Contract interface들은 각 기능의 View 및 Presenter 콜백 용 인터페이스를 정의한다.

