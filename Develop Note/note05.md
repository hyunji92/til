#Android Context

개발자 사이트 클래스 overview 
>Class Overview
 Interface to global information about an application environment. This is an abstract class whose implementation is provided by the Android system. It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents, etc.
 
>어플리케이션 환경에 관한 글로벌 정보를 접근하기 위한 인터페이스. Abstract 클래스이며 실재 구현은 안드로이드 시스템에 의해 제공된다. Context 를 통해, 어플리케이션에 특화된 리소스나 클래스에 접근할 수 있을 뿐만 아니라, 추가적으로, 어플리케이션 레벨의 작업 - Activity 실행, Intent 브로드캐스팅, Intent 수신 등, 을 수행하기 위한 API 를 호출 할 수도 있다.

즉,
1.어플리케이션에 관하여 시스템이 관리하고있는 정보에 접근하기
>Context 인터페이스가 제공하는 API중 getPackageName(), getResource() 등의 메서드 1번의 일을하는 대표적인 메서드이다. 

2.안드로이드 시스템 서비스에서 제공하는 API를 호출할 수 있는 기능
>startActivity(), bindService() 와 같은 메서드가 2번과 같은 기능을 한다.

Context는 전역적인 어플리케이션 정보에 접근하거나, 어플리케이션 연관된 시스템 기능을 수행하기 위해 필요하다. 
그리고 안드로이드에서는 Context에 정의된 인스턴스 객체를 통해야만(Context에 정의된 인스턴스 메소드) 이 기능들을 수행할 수 있다.

안드로이드에서 어플리케이션과 프로세스는 서로 독립적으로 존재입니다. 이와 관련된 구체적인 내용이나 이러한 구조를 갖는 원인에 대해서는 - http://blog.naver.com/huewu/110085391353 ( 참고하시고 꼭 읽으시길 )

안드로이드 프로세스는 리눅스인 OS커널에서 관리되고, 어플리케이션과 프로세스가 별도로 관리되어 안드로이드의 어플리케이션 정보는  안드로이드의 시스템 서비스 중 하나인 `ActivityManagerService`에서 책임집니다.
`ActivityManagerService` 는 특정 토큰을 키값으로  key-value쌍으로 이루어진 값을 배열로 현재 작동중인 어플리케이션의 정보를 관리한다.

*Context 는 어플리케이션과 관련된 정보에 접근하고자 하거나 어플리케이션과 연관된 시스템 레벨의 함수를 호출하고자 할 때 사용된다.*

즉,안드로이드 플랫폼상에서의 관점으로 샆펴보면, Context 는 다음과 같은두 가지 역할을 수행하기 때문에 꼭 필요한 존재!!!
1.자신이 어떤 어플리케이션을 나타내고 있는지 알려주는 ID 역할 
2.ActivityManagerService 에 접근할 수 있도록 하는 통로 역할 





