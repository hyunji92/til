#핸들러 스레드 : 고수준 큐 메커니즘
> 응용프로그램은 메세지 큐 및 전달 메커니즘으로 스레드와 명시적으로 연결 되어있다.
> 이러한 명시적 연결 대신에, 내부 메세지 전달 메커니즘이 자동으로 설정되는 편리한 래퍼인 `Handler Thread`를 사용할 수 있다.


###8.1 기본사항
>핸들러 스레드는 메세지 큐를 소유한 스레드. 핸들러 스레드가 시작되면 루퍼와 메시지 큐를 통해 큐를 설정하고 , 처리될 메시지가 들어오는것을 기다린다.

```java
HandlerThread handlerThread = new HandlerThread("Handler Thread");
handlerThread.start();

mHandler = new Handler(handlerThread.getLooper()){

	@Override
    public void handlerMessage(Message msg){
    	super.handlerThread(msg);
        // 여기서 메세지를 처리한다.
    }
}
```

메세지 저장용 큐는오직 하나이므로 순차적실행이보장되어 스레드 안전하지만, 테스크가 큐에 대기될 수 있어 잠재적으로 낮은 처리능력을 보인다.
핸들러 스레드는 내부적으로 루퍼를 설정하고 메세지를 수신하기 위한 스레드를 준비.

메세지를 처리하기 시작하기 전에 핸들러 스레드에 추가적인 설정이 필요하다면, HandlerThread,onLooperPrepared()를 오버라이드 해야한다. 응용 프로그램은 onLooperPrepared에서 어떤 초기화 코드든 정의할 수 있다.

`handler Thread 에 대한 접근 제한`
핸들러는 핸들러 ㅡ레드로 데이터 메세지 또는 데스크를 전달하는 데 사용될 수 있지만, 핸들러에 대한 접근을 서브 클래스 구현 안에 비공개 상태로 제한하여 루퍼에 접근할 수 없도록 보장할 수 있다.

###8.2 생명주기 
>실행중인 handlerThread 인스턴스는 종료되기 전까지 수신하는 메세지를 처리한다.
>종료된 핸들러는 재사용할 수 없다. 새로운 핸들러 스레드 인스턴스를 생성해야한다.


1. 생성 : HandlerThread 의생성자는  스레드의 name인수와 priority인수를 받는다.
	>HandlerThread(String name)
	>HandlerThread(String name, int priority)

	priority인수는 선택적이며, 리눅스 스레드우선순위와 같은 값으로 설정해야 한다.

2. 실행 : 핸들러 스레드는 메세지를 처리할 수 있는 동안 , 즉 루퍼가 스레드로 메세지를 전달 할 수 있는 동안 활성화된다. 핸들러가 생성될 수 있을 때 핸들러 스레드는 항상 메세지를 받을 준비 상태가된다.
3. 재설정 : 메세지 큐는 큐안의 메세지가 더 이상 처리되지 않도록 재설정될 수 있지만 , 그 뒤에도 스레드는 살아 있고 새로운 메세지를 처리할 수도 있다.재설정은 큐안의 모든 대기 메세지를 제거.
	> public void resethandlerThread() {
    	mHandler.removeCallbacksAndMessages(null);
    }

4. 종료 : 핸들러 스레드는 메서드 quit 또는 quitSafely를 사용하면 스레드가 종요되기 전에 전달 경계를 넘은 메세지가 처리되는 것을 보장할 수 있다.
	7장 '인터럽트'에 나오듯 현재 실행 중인 메세지를 취소하기 위해 핸들러 스레드로 인터럽트를 보낼 수 있다.

``` java
public void stopHandlerThread(HandlerThread andlerThread) {
	handlerThread.quit();
    handlerThread.interrupy();
}
```
