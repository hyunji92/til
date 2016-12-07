#메모리 관리

> 메모리 누수는 응용 프로그램에서 크래시를 초래할 뿐만 아니라 성능 저하, 응용프로그램의 실행에 해로울 수 있다.

###6.1 가비지 컬렉션
> 달빅 vm은 메모리가 너무 커질 때, 힙이라는 공유 메모리로부터 가비지 컬렉터로 할당된 메모리를 주기적으로 회수하는 메모리 관리
시스템이다. 각 프로세스 == 응용 프로그램은 자신만의 vm과 가비지 컬렉터를 가진다.

- 메모리 누수
    - 메모리를 너무 오랜 시간 할당하고 해제하지 않는 경우 
    - 더는 사용되지 않는데도 GC가 회수 할 수 없는 응용프로그램에 의해 할당된 메모리)
    

응용프로그램은 지속해서 생명주기 동안 새로운 객체를 생성하고, 객체는 인스턴스 변수로 할당되든 지역변수로 할당되든 
번위와 관계없는 힙에 만들어진다.
객체가 더 이상 사용되지 않을 때, GC는 새로운 할당을 위한 메모리를 확보하기 위해 힙에서 객체를 제거한다.

- 객체가 ``도달가능하면`` 객체는 언제는 사용될 수 있고 가비지 컬렉션 대상에서 제외되는 것으로 간주.
- 객체가 ``도달할 수 없으면`` GC는 객체를 마무리하고 메모리를 회수할 수 있다.
