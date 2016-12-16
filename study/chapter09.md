#Executor프레임워크를 통한 스레드 실행 제어
> Executor : 관리자 , 집행자 
> 스레드에서 실행되기를 기다리는 태스크 수를 제어하는 작업자 스레드 풀과 큐를 설정한다.
> 비정상적으로 종료되는 스레드를 초래하는 에러를 확인한다.
> 완료되는 스래드를 기다리고 스레드로 부터 결과를 가져온다.
> 스레드의 일괄 처리를 실행하고 고정된 순서로 결과를 가져온다.
> 사용자가 더 빨리 결과를 확인할 수 있도록 알맞은 시간에 백그라운드 스레드를 시작한다.

###9.1 Excutor
Executor 프레임워크의 기본 구성요소는 간단한 Executor인터페이스이다 .
Executor의 주 목표는 실행으로부터 태스크 ( Runnable ) 의 생성을 분리하여, 앞에서 나열한 응용 프로그램 동작들을 가능하게 하는 것이다.
`이 인터페이스는 단 하나의 메서드를 포함한다`

```java
public interface Executor {
	void execute(Runnable command);
}
```

- Exexutor는 단순하지만 강력한 실행 환경의 기초.
- 태스크픞 만드는 것과 실행 사이에 더 나은 분리를 제공하기 때문에 기본 Thread 인터페이스보다 더 자주 사용된다.
- 혼자서는 어떤 테스크도 실행살 수 없는 단지 인터페이스이다.
- 구현을 통해 실제 실행을 제공하고 태스크가 실행되는 방식을 정의할 수 있다.

간단한 형태의 Executor구현은 모든 태스크에 대한 스레드를 만드는 것이다.
```java
public class SimpleExecutor implements Executor {
	@Override
    pulblic void execute(Runnable runnable){
    	new Thread(runnable).start();
    }
}
```
위의 예제는 직접 익명의 내부 클래스로 생성 하는 것보다 더 많은 기능을 제공하지 않는다.
- 그럼에도 디커플링, 확장성, 메모리 참조 감소 들과 같은 장점을 제공한다.
위의 예제처럼 익명내부 클래스로 외부 클래스에 대한 참조르 보유하지안흥면 스레드가 참조하는메모리도 줄어든다.

요약 :  Executor 수현은 사용자가 변경을 단순화 하는데 도움을 줄 수 있다.
- 제어 가능 다른 실행 동작
- 태스크 큐잉
- 태스크 실행 순서
- 태스크 실행 유형 

####병렬 프로그래밍의 필요성
- 작업을 실행하는 가장 간단한 방법은 단일 스레드에서 순차적으로 작업을 실행하는것.
- but, 작업글중 cpu연산과 IO연산이 산재하는 경우 매우 낮은 처리량으로 인해 어플리케이션 품질 저하 우려.

##예시
```java
class SingleThreadServer {
	public static void main(String[] args) throws IOException {
    	ServerSocker socker = new ServerSocker();
        for(;;) {
        	Sorcker conn =  socker.accept();
        }
    }
    private void handleRequest(Socket socker){
    //....
    }
    /**
    이 예제는 클라이언트가 접속되고 handleRequest의 모든 작업을 수행 할때까지 다른 요청은 처리할 수 없기 때문에
    성능이 매우 떨어진다.
    웹서버가 클라이언트의 요청을 처리하는 것의 대부분은 IO작업이고 , CPU연산은 아주 약간일 것이고 
    IO작업으로 인해 CPU가 대기상황인 것을 묵인 한다면 자원의 효율적인 활용이라는 면에서도 매우 나쁜 구조일것이다.
    */
}
```
##각각 스레드로 분리해본다
```java
calss SingleThreadServer {
	public static void main (String[] args) throw Exception {
    	ServerSorcker socker = new ServerSocker(80);
        for(;;){
        	final Socker connection =  socker.accept();
            
            Runnable run =  new Runnable() {
            	publci void run() {
                	handleRequest(connection);
                }
            }
            new Thread(run).start();
        }
    }
    
    private void handleRequest(Socket socker) {
    //........
    }
}
```

- 앞의 예제와 달리 순처적으로 작업 수행하던 방법에서 문제되었던 handleRequest() 수행시간이 해결되었다.
- 서버는 handleRequest()의 수행과 상관없이 클라이언트 접속요청을 받는다.
- 동시에 한개이상의 작업을 처리할 수 잇게 하는것은 여러게의 프로세서를 효율적으로 사용할 수 있게 되어 속됴향상, IO연산 수행시 CPU의 대기상테도 없어진다.

#####스레드의 과도한 생성시

1. 스레드 생성에 의한 오버헤드 : 만들고 취소 / 종료후 자원을 반환하는데도 꽤많은 비용이 필요하다.
2. 자원관리 : 프로세서보다 많은 수의 스레드가 만들어진다면, 프로세서의 수를 초과하는 스레드는 대기상태에 머물게 되는데, 대기상태에 있는 스레드가 많을 수록 메모리를 많이 차지하게된다.
3. 안정성 : 스레드 무한정 생성 불가, Out of Memory 에러 발생.

###Executor FrameWork
- 매우 간단한 구조의 인터페이스, 다양한 종류의 작업 스케줄링을 지원하는 강력한 프레임워크의 근간
```java
public interface Executor {
	void exrcutor (Runnable command);
}
```

- Executor 는 작업의 등록과 실행을 분리하는 표준적인 방법이다. 소비자 - 생산자 패턴을 따르며
- 일반적으로 소비자- 생산자 패턴을 구현하는 가장 쉬운 방법중하나.


```java
// 직렬 실행자 직려려려려려려렬
private static class SerialExecutor implements Executor {
	final ArrayDeque<Runnable> mTask = new ArrayDeque<Runnable>();
    Runnable mActive;
    
    public sychronized void execute(final Runable) {
    	mTask.offer(new Runnalble) {
        	public void run(){
            	try{
                	r.run();  // r.run()이 완료될 때마다 
                } finally {
                	scheduleNext(); // 이부분이 호출 
                    // 이부분은 큐에서 다음 태스크를 가져와서 
                    // 스레드가 태스크를 실행할 수있는 '스레드 풀' 안의 다른 executor로 보낸다.
                }
            }
        });
        if(mActive == null){
        	scheduleNext();
        }
    }
    protected sychronized void scheduleNext(){
    	if((mActive = mTask.poll) != null) {
        	THREAD_POLL_EXECUTOR.execute(mActive);
        }
    }
}
```
- 태스크 큐잉
	`ArrayDeque<Runnable> mTask` ( 데큐 - 양방향으로 꺼낼 수 있는 큐) : 데큐는 스레드에 의해 처리될 때가지 삽입된 태스크들을 보유한다.

- 태스크 실행 순서 
	모든 태스크는 mTask.offer()를 통해 데큐의 끝에 넣어진다. FIFO

- 태스크 실행 유형
	태스크는 직렬로 실행, 같은 스레드에서 실행될 필요는 없다. 

`위에서 보이듯 EXecutor는 비동기 실행에 유용 

###Sample Executor
```java
class ExecutionServer {
	private static final Executor executor =  Executors.newFixedThreadpool(100);
    
    public static void main (String[] args) throw IOException {
    	ServerSocker socker = new ServerSocket(80);
        fot(;;){
        	final Socket connection =  socket.accept();
            
            Runnable run =  new Runnable(){
            	public void run(){
                	handlerRequest(connection);
                }
            }
            executor.excute(run);
        }
    }
    private void handleRequest(Socket socket){
    //....
    }
}
```

- 위세서 보는것과 같이 작업을 정의하는 영역과 실행하는 영역을 분리하면 실행정책을 자유롭게 변경 가능한 장점 
	- 최대 몇개의 작업을 큐에 넣을 수 있는지
	- 작업의 우선순위는 어떻게 지정하는지
	- 작업은 어떤스레드에서 실행할 것인지
	- 최대 몇개의 작업을 동시에 병렬로 진행할 것인지
	- 시스템 자원이 부족한 경우 어떤작업을 희생할지
	- `pre-Execution / Post-Execution : 작업전/후 동작 지정 가능


###9.2 스레드 풀

> 스레드 풀은 `테스크 큐` 와 `작업자 스레드` 집합의 조합
> `작업자 스레드` 는 생산자-소비자 구조를 형성한다.
> 생산자 큐에 태스크를 추가하고 , 작업자 스레드는 새로운 백그라운드 실행을 수행할 준비가 된 유휴 스레드가 있을 때 마다 태스크를 소비한다.

- 스레드를 풀의 형태로 관리
- 관리해야할 작업을 큐에 넣어 두었다가 , 다음의 메카니즘으로 동작
	1. 작업큐에서 다음작업을 가져옴
	2. 가져온 작업 실행
	3. 다음작업을 가져오거나 가져올 수 있는 다음 작업이 나타날 때까지 대기한다.

스레드 풀은 스레드의 최대 개수로 정의 된다. 이는 응용프로그램의 메모리를 소비하는 백그라운드 수가 많아져서 스레드 풀에 과부하가 걸리는 것을 막기 위해서이다.
모든 작업자 스레드의 생명주기는 스레드 풀 생명주기에 의해 제어된다.

- 풀 내부 스레드를 활용하는것이 작업마다 스레드를 생성하는 방법보다 더 효율적이다. 
	풀 내부 스레드는 새로운 작업에 대해 전에 쓰던 스레드를 재사용하기 때문이다. 스레드 생성에 따른 메모리 오버헤드 뿐만 아니라,
    생성동작에 걸리는 시간적 오버헤드까지 없어지므로 반응속도 또한 향상된다.

- 아래의 스레드유형은 Executors팩토리 클래스에서 만들어진 미리 정의된 스레드 풀 유형을 이다.
    - `newFixedThreadPool` `고정 크기` 처리할 작업이 등록되면 실제 작업할 스레드를 하나씩 생성, 스레드의 수는 제한되어있다.
    - `newCachedThreadPool` `동적 크기` 큐에 있는 작업의 수보다 생성이 되어있는 스레드수가 많으면 최과 범위에있는 스레드 종료.
    - `newSingleThreadPool` `싱글 스레드 실행자`  단일 스레드 동작, Exception발생으로 비정상적인 종료가 된다면 스레드 다시 생성
    - `newScheduledThreadPool` 일정시간 이후에 실행하거나 반복실행을 위해서 사용한다.스레드 수 고정

####스레드의 종료
JVM 은 모든 스레드가 종료하지 않으면 자신도 종료하지 않고 대기하므로 Executor를 제대로 종료시켜야 JVM도 종료됨
Executor는 정상적이건간에 종료절차를 밟아야할 필요가 있다.

종료까지 고려하게 바까보자 / 페이지 182

```java
class ExecutionServer {
	//private static final Executor executor =  Executors.newFixedThreadpool(100);
    private static final Executor Service mExecutorService = Executors.newFixedThreadPool(100);

    public static void main (String[] args) throw IOException {
    	ServerSocker socker = new ServerSocket(80);
        fot(;;){
        	final Socket connection =  socket.accept();
            // try/catch문 추가
            try{
            	Runnable run =  new Runnable(){
            	public void run(){
                	handlerRequest(connection);
                    }
                 }
            	executor.excute(run);
            } catch (RejectedExecutionException e) {
            	if(!exec.isShutdown()) {
                	Log.e("ExecutorService", "task rejected!");
                }
            }
        }
    }		// try/catch문 추가
    private void handleRequest(Socket socket){
    	if(/* isrequestToFinish */){
        	mExecutorService.shutdown();
        }else{
			//.....
        }

    }
}
```

ExecutorService는 세 가지의 Status를 갖는데 <Running, Shutting Down, Terminated>이다.

- shutdown()
메소드가 불리면 곧바로 종료 절차를 진행하게되고 isShutdown()은 true가 된다. 이때에는 더이상의 추가 작업은 거부되지만, 이미 등록된 작업(대기중인 작업 포함)은 모두 마친다.
모든 작업이 종료되면 ExecutorService는 isTerminated()의 리턴값으로 true를 가진다.
- shutdownNow()
메소드가 불리면 종료 절차를 진행하게 되는데 대기중인 작업은 취소시키고, 이미 running인 작업도 가능하면 중단시킨다
- awaitTermination()
ExecutorService가 terminated 상태로 갈때까지 기다린다. (물론 isTerminated()를 폴링할 수도 있다)

####9.2.2 커스텀 스레드 풀
>Executors의 미리 정의된 스레드 풀 유형은 ThreadPoolExecutor클래스에 기반하는데 , 이 클래스는 상세한 스레드 풀의 동작을 만드는데 직접 사용될 수 있다. 

#####ThreadPoolExecutor 설정
이속성들은 ThreadPoolExecutor에 의해 스레드의 생성과 종료, 태스크의 큐잉에도 사용된다.

*하한 : 위아래로 일정한 범위를 이루고 있을때의 아래쪽의 한계

```java
ThreadPoolExecutor executor = new ThreadPoolExecutor (
	int corePoolSize 		//핵심 풀 크기
    /*
    실제로 스레드 풀은 0개 스레드로 시작하지만, 핵심 풀 크기에 도달하면 스레드 개수는 최소치 이하로 떨어지지않는다.
    */
    int maximumPoolSize		//최대 풀 크기
    /*
    동시에 실행할 수 있는 스레드의 최대 개수
    */
    long keepAliveTime		//생존 유지 시간
    /*
    유휴 스레드는 처리하기위해 들어오는 태스크 준비-> 활성상태 유지 -> 생존시간이 설정된 경우 시스템은 중요치 않은 	 풀 스레드를 회수할 수 있다 .
    */
    TimeUnit unit			//생존 유지 시간의 단위
    BlockingQueue<Runnable> workQueue);

```


####9.2.3 스레드 풀 설계
> 스레드 풀을 설정하기 위해서는 필요한 것보다 더 많은 메모리를 사용하지 않고 하드웨어에 의해 허용되는 가장 높은 속도록 작업을 처리하는 스레드 풀을 생성하는것이다.
> 유휴스레드는 이전 스레드를 소멸하고 새로운 스레드를 생성하는 오베헤드를 제거하여 큐안에 추가된 새로운 태스크를 실행하는데 사용

띠리서, 유휴시간을 길게 하면 메모리 사용은 더 많이ㅏ지지만 약간의 성능향상을 얻을 수 있다.

일반적으로 제한된 작업 스레드 수를 가진 안드로이드 응용 프로그램의 경우, 유휴 시간을 미세 조정 하여 얻는 성능 이득은 작고 거의 필요하지 않다.



#####제한 또는 무제한 태스크 큐
> 스레드 풀은 일반적으로 제한 또는 무제한 태스크 큐와 함께 사용된다. 
> 무제한 큐는 무한 증가 할 수 있어서 메모리가 고갈 될 수 있다.
> 제한 큐의 자원 소비는 더 잘 관리될 수 있다.
> 한편 제한 큐는 그 크기와 포화 정책을 모두 준비 해야한다.
> 포화정책이란 거부된 태스크를 생산자가 어떻게 처리할지를 뜻한다.

- 제한 큐
	1. ArrayBlockingQueue
	2. priorityBlockingQueue

- 무제한 큐
	1. LinkedBlockingQueue


#####크기
스레드 풀의 크기를 정하는 것. 스레드의 최대 개수가 너무 작으면 충분한 속도로 큐에서 태스크를 꺼내지 않아 성능이 저해될 수 있다.
스레드 풀의 크기는 하드웨어,CPU의 개수를 기준으로 하는것이 좋다.
하지만 스레드 최적 개수는 찾는것이 정확하지는 않다.

#####역동성
고정된 크기의 스레드 풀로 정의하지 않으면 스레드 수는 스레드 풀의 생명주기 동안 변화한다.
이런 역동성은 `핵심 스레드 : core Thread`  와 `생존 유지 시간 : keepAliveTime`에 의해 만들어진다.
스레드 풀은 큐에 들어간 태스ㄹ크를 기다리면 풀이 사라있도록 유지하는 핵심 스레드 집합을 정의한다.

#####스레드 설정
> ThreadPoolExecutor 는 작업자 스레드 개수와 풀의 생성과 종료, 모든스레드의 속성 정리
> 작업자스레드는 ThreadFactory 인터페이스의 구현을 통해 설정된다.
> 스레드 풀은 우선순위, 이름, 예외 핸들러와 같은 작업자스레드의 속석을 정의할 수 있는것

```text
*Note
런타임 : ART 는 Android RunTime 의 약자, 앱을 설치할 때 완전히 네이티브 앱으로 변환하여 설치한다. 이 과정을 Ahead-Of-Time( AOT ) 컴파일이라 한다.
ART 를 사용하면, 새로운 가상머신(Dalvik)을 매번 생성하고, 인터프리트 된 코드 실행하는 시간을 제거하여 performance 가 엄청 향상될 수 있다.
다시 말해 VM 자체가 필요없어, iOS 와 비슷한 성능의 이점을 얻을 수 있다는 것
```

