#프로세스간 통신
>이장은 스레드가 프로세스 경계에 걸쳐 통신하는 방법을 다룬다.
안드로이드 응용 프로그램 스레드는 프로세스 메모리를 공유하는 프로세스 내에서 자주 통신을 한다.
안드로이드 플랫폼은 바인더 프레임 워크를 통해 `프로세스간 통신 : IPC `도 지원한다

가장 일반적인 IPC사용 사례는 인텐트, 시스템 서비스, 콘텐트 프로바이더등 안드로이드의 고수준 구성요소에 의해 처리된다.


##5.1 안드로이드 RPC

>프로세스간 통신 IPC는 리눅스 os에 의해 관리된다.


리눅스 IPC기술은 프로세스 사이의 RPC메커니즘을 수행하는 바인더 프레임워크로 대체되었다.
이를통해 프로세스는 로컬에서 메서드를 실행하듯 서버 프로세스의 원격메서드를 호출할 수 있고 
데이터가 서버 프로세스로 전달되어 스레드에서 실행되고 호출한 스레드로 결과값을 반환할 수 있게 되었다.

RPC(Remote Procedure Call) 프레임워크를 제공, 이 RPC메커니즘의 하부는 다음과 같다.

- 메서드 데이터 분해 , 마샬링
- 원격 프로세스로 마샬링된 정보를 전송
- 원격프로세스에 정보를 재구성 , 언마샬링
- 원래 프로세스로 반환값을 전송

####5.1.1 바인더
> 원격 바인더는 응용프로그램이 다른 프로세스에서 실행하는 스레드들 사이에 함수와 데이터 즉 메서드 호출을
보낼 수 있게 한다.

함수와 데이터 모두를 전송하는 원격 프로시저 호출을 `트렌재션`이라고 한다.
클라이언트 프로세스가 transact메서드를 호출하면 서버 프로세스는 onTransact메서드로 그 호출을 받는다.

트랜잭션 데이터는 프로세스 사이의 전송을 위해 바인더에 의해 최적화된 android.os.parcel 객체로 구성된다.
즉 인수와 반환값은 Parcel객체로 전송된다.
Parcel객체는 리터럴 인수와 android.os.Parcelable을 구현한 커스텀 객체를 포함한다
```text
Parcelable 인터페이스는 Serializable보다 효율적인 방법으로 마샬링 언마샬링을 정의한다.
```

onTransact 메서드는 바인더 스레드의 풀에 속한 스레드에서 실행된다. 이풀은 다른 프로세스에서 들어ㅣ오는 요청을 처리하기 위해서만
존재한다 

`프로세스간 통신 :  IPC` 는 양방향으로 동작할 수 있다 서버 프로세스가 클라이언트 프로세스에 트랜잭션을 방행할 수 있고
앞세서 서버 프로세스였던 것이 클라이언트가 되고, 클라이언트가 서버가 된다. 
이와같이 두프로세스 사이에서 양방향 통신 메커니즘이 성립할 수 잇다.

```text
서버 프로세스가 트랜잭션을 시작할 경우 transact를( 클라이언트 프로세스에 요청을 보내는 ) 호출하면 클라이언트 프로세스는
 바인더 스레드로 들어오는 요청을 받지는 않지만 해당 스레드에서 첫번째 트랜잭션을 마치기위해 기다린다.
``` 

#####바인더는 또한 IBinder.FALG_ONEWAY 를 설정함으로써 비동기 트랜잭션을 지원한다.
이플래그를 설정하면 클라이언트 스레드는 transact를 호출하고 즉시 반환한다.

##5.2 AIDL
> AIDL : 안드로이드 인터페이스 정의 언어의 인터페이스를 기술하는 가장 간단하고 일반적인 방법은 .aidl파일에 정의하는것이다
AIDL 파일을 컴파일하면 IPC를 지원하는 자바 코드를 생성한다..

좋은 참고 사이트를 발견 ....
 - http://blog.naver.com/PostView.nhn?blogId=tempests05&logNo=20146137109
 
생성된 자바 인터페이스는 모든 클라이언트 응용프로그램과 서버응용 프로그램에 포함된다. 인터페이스 파일은 데이터의 마샬링, 언마샬링 뿐마아니라 트랜잭션까지 다루는 Proxy와 Stub
두개의 내부클ㄹ래스를 정의한다.

```text
이처럼 AIDL의 생성은 자동으로 바인터 프래임워크를 감싸는 자바 코드를 생성하고 통신 계약을 설정한다.
```

####5.2.1 동기식 Remote Procedure Call
>원격 프로세스에서 스레드 이름을 반환하는 개념 코드로 동기식 RPC와 의미를 살펴본다.

서버 프로세스의 바인더 객체에 접근하는 클라이언트 프로세스는 Proxy구현체를 가져오고 원격에서 실행할 메서드를 호출한다.
실행은 원격 프로세스내 바인더 스레드에서 일어났다

>원격으로 수명이 짧은 작업을 호출

필요한 경우 하나 또는 여러 클라이너트에서 동시 호출은 다수의 바인더 스레드를 활용할 수 있지만 구현이빠르게 반환하기 때문에 바인더 스레드는 효율적으로 재생용 될 수 있다.

>원격으로 수명이 긴 작업을 호출

모든 클라이언트 호출은 오랫동안 하나의 바인더 스레드를 점유한다 결과적으로 여러 번의 호출은 사용할 스레드를 제한하여 바인더 스레드 풀을 한계에이르게 한다.
이경우 원격 메서드 중 하나를 호출하는 다음 스레드는 바인더 큐에 들어가고 사용 가능한 바인더 스레드가 있을 때까지 실행을 시작하는 것을 기다려야 한다.

>차단된 메서드 호출

바인더 스레드 차단도 원격 메서드가완료 될 때까지 클라이언트 스레드를 차단한다. 여러 클라이언트 스레드가 동시에 서버 프로세스에서 차단된 메서드를 호출하면 바인더 스레드 풀은 사용할 수 잇는 스레드가
없어지게 되어 다른 클라이언트 스레드들이 원격 호출에서 결과값을 얻는것을 막게된다.

차단된 자바 스레드는 보통 인터럽트가 가능하다. 즉, 스레드가 차단된 스레드의 실행을 완료하기 위해 차단되어 있는 스레드를 중단시킬 수 있다
그러나 반대로 클라이언트 프로세스의 스레드는 서버 프로세스의 스레드에 직접 접근할 수없으므로 원격 스레드를 중단 시킬 수없고 ,
동기식 RPC의 반환을 기다리는 클라이언트 스레드는 인터럽트를 알아채거나 처리할 수도 없다.