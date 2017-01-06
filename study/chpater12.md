# 인텐트 서비스

> 서비스는 UI스레드에서 실행되기 때문에 그것만으로는  비동기 기술일 수 없다. 이런 단점은 service 클래스를 확장한 IntentService를 통해 해결 가능하다.
>
> IntentService는 service의 생명주기 속성을 포함 하고 백그라운드 스레드의 테스크 처리를 내장한다.

### 12.1기본사항

인텐트 서비스는 싱클 백그라운드 스레드에서 태스크 처리. 모든 태스크는 순차적으로 실행 된다.

- 인텐트 서비스가 실행 중인 경우 - 인텐트는 백그라운드 스레드가 처리를 위해 준비 될 때까지 큐에서 대기한다
- 인텐트 서비스가 실행 중이 아닐경우 - 새로운 구성요소 생명주기가 시작되고, 더 이상 처리할 인텐트가 없을 때 구성요소의 생명주기가 끝난다. ( 실행할 태스크가 있는 동안에만 IntentService가 실행 )

### *NOTE*

```reStructuredText
IntentService에서 백그라운드 태스크 실행자는 핸들러 스레드다.
AsyncTask의 기본 실행자와 달리 IntentService실행자는 응용프로그램 단위가 아닌 인스턴스 단위다.
그래서 응용프로그램은 여러 IntentService의 인스턴스를 가질 수 있다. 
순차적으로 태스크를 실행하지만 다른 IntentSercive인스턴스와는 독립적이다.
```



IntentService 구성요소

*AndroidManifest.xml*

```java
<service android:name=".SimpleIntentService"/>
```



*IntentService 구현*

```java
public class SimpleIntentService extends IntentService {
  public SimpleIntentServiece() {
    super(SimpleIntentServiece.Class.getName());
    setIntentTedelivery(true);
  }
  
  @Override
  Protected void onHandlerIntent(Intent intent) {
    //백그라운드에 호출됨
  }
}
```

*생성자는 디버깅 목적을 위해 백그라운드 스레드 이름인 문자열로 슈퍼클래스로 호출하여야한다*

프로세스가 죽은 경우 인텐트 서비스가 복원되어야 하는지 생성자에서 지정할 수 도있고  기본적으로는 보류중인 인텐트 서비스가 시작 요청이 있을때만 복원된다.

setIntentRedelivery(true) 호출은 마지막 전달된 인텐트를 재전달한다.



### *NOTE*

```text
IntentService - START_NOT_STICKY 
			  - START_REDELIVERY_INTETNT 
IntentService는 두가지 유형을 처리하는데 전자는 기본 설정이고, 후자는 setIntentRedelivery(true) 설정이 필요하다.
```



IntentService를 사용하기 원하는 클라이언트는 Context.startService로 시작 요청을 생성하고 서비스가 처리해야 하는 데이터와 함께 인텐트를 전달한다.

```java
public class SimpleActivity extends Activity {
  
  public void onButtonClick(View v) {
    Intent intent = new Intent(this, SimpleIntentService.calss);
    Intent.putExtra("data" , data);
    startService(intent);
  }
}
```



### 12.2 인텐트 서비스를 사용하는 좋은 방법

IntentService 는 `순차 태스크 처리`를 이용해서 태스크를 UI 스레드에서 백그라운드 스레드로 쉽게 넘기고 싶을 때 적합하다.

`순차 태스크 처리` 는 태스크에 프로세스 순위를 높이기 위해 항상 활성화 되어 있는 구성요소를 준다.





#### 12.2.1 순차적으로 정렬된 태스크

IntentService는 구성요소를 시작한 곳과 독립적이며, 순차적으로 실행되어야 하는 태스크에 사용할 수 있다. 



#### *웹 서비스 통신*

웹서비스 같은 네트워크 자원과의 통신은 종종 순차적인 방식으로 처리된다. 가져온 하나의 자원은 다른 자원과 상호작용하는 방법에 대한 향후 지침을 포함한다.

HTTP프로토콜은 GET, POST, PUT, DELETE 요청의 유형을 이용하여 네트워크 자원과 상호작용한다. 

요청은 상호작용 또는 스케줄된 시스템 동작에 따라 생겨날 수 있지만, IntentService에 의해 처리될 수 있다.



*아래 예제에서는 요청은 일반적으로 사용자 액션에 의해 초기화 되는 액티비티로 부터 시작된다. 가장 일반적인 유형의 요청으로 데이터를 가져오는 GET과 데이터를 전송하는 POST를 다룬다. 두 요청 모두 IntentService로 떠넘겨지고 , 요청에 의한 응답으로 ResultReceiver반환된다*

```java
public Class WebServiceActivity extends Activity {
  
}
```

