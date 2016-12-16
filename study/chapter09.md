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


