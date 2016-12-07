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

즉,안드로이드 플랫폼상에서의 관점으로 샆펴보면, `Context` 는 다음과 같은두 가지 역할을 수행하기 때문에 꼭 필요한 존재!!!
1.자신이 어떤 어플리케이션을 나타내고 있는지 알려주는 ID 역할
2.ActivityManagerService 에 접근할 수 있도록 하는 통로 역할


####Context 셍성시점
 Activity 와 Service 가 생성될 때 만들어지는 Context 와 BroadcastReceiver 가 호촐될 때( onReceive() ) 전해지는 Context 는 모두 서로다른 인스턴스이다.
즉, Context 는 어플리케이션이 시작될 때는 물론이요, 어플리케이션 컴포넌트들이 생성될때마다 만들어지는것이다.
물론, 새롭게 생성되는 Context 들이 부모와 완전히 독립되어 있는 존재는 아니고 '거의' 비슷한 내용을 담고 있다.

Context의 기능중, 시스템 API를 호출하는 기능에 관련된 한가지 문제점이 있다면, 어떤 어플리케이션 컴포넌트가 시스템 API를 호출하느냐에 따라서 서로 다른 결과가 나타나야 한다는것이다. 동일한 형태로 startActivity 메서드를 호출하더라고, 일반적인 Activity애서는 정상적으로 새로운 Activity를 시작하게 되지만 , Service에서 호출할 경우 예외가 발생한다.

*만일 어플리케이션을 구성하는 Service 와Activity 가 서로 동일한 Context인스턴스를 공유하고 있다면 동일한 메서드 호출에 대해 서로 다른 결과를 나타내도록 구현하지 못할거다*
따라서, 현재 안드로이드 시스템은 어플리케이션 Context 를 기반으로 컴포넌트를 위한 Context 를 생성할 때 해당 Context 가 어떤 종류의 컴포넌트인지 알 수 있도록 약간의 표시를 하곤한다.

그래서 결론은 android Context는 위에 참고 사이트와 같은 여러가지이유로 기존 플랫폼과는 다르게 어플리케이션을 관리하고있고, 때문에 기존 플랫폼들에서는 단순하게 시스템 API를 통해 할  수 있는 일들을, Context 인스턴스라는 귀찮지만 강력한 것을 통해 대행 처리한다고 생각하면 되겠다.







