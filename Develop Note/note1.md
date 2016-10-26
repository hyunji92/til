#개발노트 

##JAVA

> 노트에 정리해 놓은 내용을 기록한다

###*API ( Application  programming  interface)
	클래스를 통해 객체를 만드는 방법중 하나 .
    클래스에 public으로 선언된 ' 정적 펙토리 메서드' 를 추가하는 것.
    "static factory method" public으로 생선된 생성자 대신임.

* API는 사용가능한 기능을 규정한 소프트웨어 컴포넌트로 만들어진 커다란`준비된 컬랙션` 이다.
* 연관된 클래스들과 인터페이스들의 라이브러리들로(패키지) 그룹화 되어있다.
    

###정적 팩토리 메서드 (static Factory Method)
장점
1. 생성자와는 달리 static factory method는 이름이 있다.
2. 외부 클래스에서 바로 접근이 가능한, 생성자 역할을 하는 method이다,
3. 중복 signitrue constructor의 형태가 가능하다.
4. 생성자와 달리 호출 될 때마다 새로운 객체, instance를 생성할 필요가 없다.
5. static Factory method는 반환하는 타입의 subtype을 반환할 수 있다.
   -> 생성자와는 달리 리턴값 자료형의 하위 자료형 객체도 반환할 수 있다는 것!

    static Factory Method 방식은 기존에 만들어져 있는 객체를 반환하는 "singleton"을 통해 불필요한 객체 생성을 피할 수 있다.


* 새로운 객체 생성시 new 명령어 통해 만들어지기 때문에 heap메로리를 차지한다
  -> 생성징에서 아무것도 안하는 경우는 큰문제가 되지 않지만 , 모바일 환경에서는 특히나 메모리가 부족하기 때문에 불필요한 객체 생성은 피해야 한다.

###서비스 제공자 프레임워크

1. 서비스 인터페이스
2. 제공자 등록 API (Provider registration API)
3. 서비스 접근 API(service acess API) : 유연한 정적 팩토리 서비스 제공자 프레임 워크의 근간을 이룬다
	-> 클라이언트에게 실제 서비스 구현체 제공
4. 서비스 제공자 인터페이스 (service provider interface)

* service 접근 API에 Adapter패턴을 적용하면 제공자에게 요구되는 것보다 풍부한 기능을 제공하는 서비스 인터페이스 객체를 반환할 수 있다.

단점
1. static Factory Method만 만들어놓은 클래스를 만든다면 public 아니 protected로 선언된 생성자가 없으므로 하위 클래스를 만들 수 없다.
2. static Factory method가 다른 정적 메서드와 확연히 구분되지 않는다.


###java compile
* java 확장자의 순수 텍스트 파일 
  -> javac 컴파일러에 의해 .class로 컴파일 된다
* .class 파일은 여러 프로세스애서 원레의 텍스트 코드를 포함하고 있지 않는다.
* 대신 JVM의 기계어인 bytecode로 대신 구성된다.

	
    .class 파일 내에는 java소스가 있는게 아니라 jvm이 해독할 수 있는 바이트 코드로 변환되어있다.
    왜나하면?! jvm은 다양한 운영체제 에서도 .class파일들이 작동 될 수 있어야 함으로
    


> 여러분이 알고있는 psvm `public static void main (String[] args)
> args :  아큐먼트는 프로그램 시행시 입력받는 테이터 ( 변수 )
> main메소드는 단일 아규먼트 , 배열형 String type을 받아들인다.
> 이배열은 여러 프로그램의 정보를 전달하는 런타임 시스템 매커니즘이다.