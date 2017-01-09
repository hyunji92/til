# 인텐트 서비스

> 서비스는 UI스레드에서 실행되기 때문에 그것만으로는  비동기 기술일 수 없다. 이런 단점은 service 클래스를 확장한 IntentService를 통해 해결 가능하다.
>
> IntentService는 service의 생명주기 속성을 포함 하고 백그라운드 스레드의 테스크 처리를 내장한다.

### 12.1기본사항

인텐트 서비스는 싱글 백그라운드 스레드에서 태스크 처리. 모든 태스크는 순차적으로 실행 된다.

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
    setIntentRedelivery(true);
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
  private final static String getUrl ="";
  private final static String postUrl="";
  
  private ResultReceiver mReceiver;
  
  public WebServiceActivity() {
    mReceiver = new ResultReceiver(new Handler()) {
      // 작업 결과 반환될 수 있도록 IntentServie로 전달되는 ResultReceiver를 생성한다.
      @Override
      protected void onReceiverResult(int resultCode, Bundle resultData) {
        int httpStatus = resultCode;
        String 	jsonResult = null;
        if(httpStatus == 200) {
          //ok
          if(resultData != null) {
            jsonResult =  resultData.getString(WebService.BUNDLE_KEY_REQUEST_RESULT);
            //생략됨 응답 처리
          }
        }
        else {
          // 생략됨 에러를 처리
        }
      }
    } ;
  }
  private void doPost(){
    // JSON형식의 콘텐츠로 POST요청을 발행한다.
    Intent intent = new Intent(this, WebService.class);
    intent.setData(Uri.parse(postUrl));
    intent.putExtra(WebService.INTENT_KEY_REQUEST_TYPE, WebService.POST);
    intent.putExtra(WebService.INTENT_KEY_JSON, "{\"foo\":\"bar\"}");
    intent.putExtra(WebService.INTENT_KEY_RECEIVER, mReceiver);
    startService(intent);
  }
  
  private void doGet(){
    //GET 요청을 발생한다.
    Intent intent = new Intent(this, WebService.class);
    intent.setData(Uri.Parse(getUrl));
    intent.putExtra(WebService.INTENT_KEY_REQUEST_TYPE, WebService.POST);
    intent.putExtra(WebService.INTENT_KEY_RECEIVER, mReceiver);
    
    startService(intent);
  }
}
```

*IntentService 는 onHandleIntent 에서 요청을 받고 순차적으로 처리한다. WebServiceActivity에서 전달된 데이터에 따라 `요청유형`, `URL`, `ResultReceiver`, 전송될 데이터 가 전달된다.*

```java
public class WebService extends IntentService {
  private static final String TAG = WebService.class.getName();
  public static final int GET = 1;
  public static final int POST = 2;
  
  public static final String INTENT_KEY_PEQUEST_TYPE = "com.eat.INTENT_KEY_REQUEST_TYPE";
  public static final String INTENT_KEY_JSON = "com.eat.INTENT_KEY_REQUEST_TYPE";
    public static final String INTENT_KEY_RECEIVER = "com.eat.INTENT_KEY_REQUEST_TYPE";
    public static final String BUNDLE_KEY_REQUEST_RESULT = "com.eat.INTENT_KEY_REQUEST_TYPE";
  
  public WebService(){
    super(TAG);
  }
  
  @Override
  protected void onHandleIntent(Intent intent) {
    Uri uri =  intent.getData();
    // 인텐트에서 필요한 data를 가져온다.
    int requestType = intent.getIntExtra(INTENT_KEY_REQUEST_TYPE, 0);
    Stting json = (String) intent.getSerializableExtra(INTENT_KEY_JSON);
    ResultReceiver receiver =  intent.getParcelableExtra(Intent_KEY_RECEIVER);
    
   try {
            HttpRequestBase request = null;
            switch (requestType){
                // 인텐트 데이터에 따라 요청 유형을 만든다
                case GET:
                    request = new HttpGet();
                    // 요청 설정 생략
                    break;

                case POST:
                    request = new HttpPost();
                    if(json !=  null){
                        ((HttpPost)request).setEntity(new StringEntity(json));
                    }
                    break;
            }

            if(request != null){
                request.setURI(new URI(uri.toString()));
                HttpResponse response = doRequest(request);// 네트워트 요청을 수행한다
                HttpEntity httpEntity = response.getStatusLine();
                int statuscode = responseStatus != null ? responseStattus.getStatusLine() : 0;

                if (httpEntity != null){
                    Bundle resultBundle =  new Bundle();
                    resultBundle.putString(BUNDLE_KEY_REQUEST_RESULT, EntityUtils.toString(httpEntity));
                    receiver.send(statuscode, resultBundle);//WebServiceActivity 에 성공한 결과를 반환한다.
                }else {
                    receiver.send(statuscode, null);
                }
            }else {
                receiver.send(0, null);
            }

        } catch (IOException e) {

        } catch (URISyntaxException e) {
            e.printStackTrace();
        }
    }

    private HttpResponse doRequest(HttpRequestBase request) throws IOException{
        HttpClient client =  new DefaultHttpClient();
        //HttpClient 설정 생략
        return  client.execute(request);
    }
}
```

 #### 12.2.2 broadcast Receiver 에서 비동기 실행

broadcastReceiver 는 응용프로그램의 짐입점. 프로세스에서 시작되는 첫 번째 안드로이드 구성요소가 될 수 있다.

broadcastRecdiver는 UI스레드에서 호출되는 onReceiver콜백으로 인텐트를 보낸다. 따라서 오래 걸리는 작업이 실행되어야 하는 경우 비동기 실행이 필요하다. 따라서 오래 걸리는 작업이 실행되어야 하는 경우 비동기 실행이 필요하다.



*but, broadcastReceiver 구성요소는 onReceiver의 실행동안에만 활성화된다. 따라서 BroadcastReceiver가 진입점이고 프로세스가 빈상태라면 태스크가 끝나기 전에 런타임이 프로세스를 죽일 수 있어 구성요소가 소멸한  후 비동기 태스크는 실행중으로 남아 있을 수 있다.*

빈프로세스 문제를 회피하기 위해 브로드캐스트 리시버에서의 비동기 실행을 위한 가장 이상적인 후보는 인텐트 서비스인것. 

- 브로드 캐스트 리시버에서 시작 요청 
- 백그라운드 실행동안 새로운 구성요소를 활성화
- onReceiver가 종료해도 문제가 되지 않는다.



*주기적인 긴 작업*

응용프로그램이 자신이 실행되지 않을 때에도 주기적인 태스크를 시작해야 할때 , AlarmManager시스템 서비스 활용. 



*Activity 에서 broadcastReceiver와 AlarmManager설정*

```java
public class AlarmBroadcastActivity extends Activity {

    private static final long ONE_HOUR = 60 * 60 *1000;

    AlarmManager am;
    AlarmReceiver alarmReceiver;

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        alarmReceiver = new AlarmReceiver();
        registerReceiver(alarmReceiver, new IntentFilter("com.eat.alarmreceiver"));//AlarmManager 인텐트 받을 브로드캐스트 리시버 등록

        PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 0, new Intent("com.eat.alarmreceiver"), PendingIntent.FLAG_UPDATE_CURRENT);
        am = (AlarmManager) (this.getSystemService(Context.ALARM_SERVICE));
        am.setRepeating(AlarmManager.ELAPSED_REALTIME, SystemClock.elapsedRealtime() + ONE_HOUR , ONE_HOUR, pendingIntent);
        //AlarmManager 매시간 응용프로그램을 시작하도록 설정한다./

        /**AlarmManager 는 매시간 시작되며, 네트워크 작업을 처리할 수 있는 인텐트 서비스로 호출 리다이렉트*/
    }



    private class AlarmReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            context.startService(new Intent(context, NetworkCheckIntentService.class));
        }
    }
}
```



*NetworkCheckIntentService.java*

```java
public class NetworkCheckIntentService extends IntentService {

    public NetworkCheckIntentService() {
        super("NetworkCheckerThread");
    }

    @Override
    protected void onHandleIntent(Intent intent) {
        if (isNewNetWorkDataAvailable()){
            addStatusBarNotification();
        }
    }

    private void addStatusBarNotification() {
        Notification.Builder mBuilder =  new Notification.Builder(this)
                //.setSmallIcon()
                .setContentTitle("new title")
                .setContentText("new data can be download ");

        NotificationManager mNotificationManager =  (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
        mNotificationManager.notify(1, mBuilder.build());
    }

    private boolean isNewNetWorkDataAvailable() {
        // 네트워크 요청 코드 생략됨. 더미값을 반환한다.
        return  true;
    }
}
```

### 12.3 IntentService 와 Service

 IntentService 는 동일한 선언, 프로세스 순위에 대한 동일한 영향, 클라이언트를 위한 동일 한 시작 요청 절차 등의 특성 상속.

*IntentService를 사용하는 응용프로그램이 onHandleIntent 를 구현하도록, 서비스의 의미를 처리하는 시작요청을 구현한다. 따라서, IntentService의 사용은 일반적으로 사용되는 태스크 제어 서비스와일치, 비동기 실행과 구성요소 생명주기 관리에 대한 자원을 내장*

1. 클라이언트에 의한제어
   - 구성요소의 생명주기가 다른 구성요소에 의해 제어되길 원한다면 사용자 제어서비스를 쓰면된다.
2. 동시적인 태스크 실행
   - 동시에 태스크를 실행하려면 서비스에서 여러 스레드 시작
3. 순차적이고 재배열 가능한 태스크
   - 태스크 큐를 우회하기 위해 태스크가 우선시 될 수 있다. 예로 음악서비스는 일반적으로 정지요청 우선시 , 큐에서 다른 태스크에 앞서 실행될 수 있도록하기위해 서비스가 필요한것.



