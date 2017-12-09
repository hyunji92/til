# Chapter 8 - [Broadcast receiver]

- broadcastReceiver는 안드로이드에서 observer패턴을 구현한 방식이다
- 안드로이드 4대 컴포넌트로 불리는 Activity, ContentProvider, Service, *BroadCasrReceiver 중에 하나이다.
- BroadCast란 말 그대로 방송용 이벤트, 방송을 듣는 리시버가 등록 되어있다면 리시버에서 받아서 처리한다.
- 옵저버 패턴을 사용하여 인터페이스를 통한 느슨한 결합을 통해 옵저버를 register/unregister하는 방법을 제공한다.
- BroadCastReceiver -> 옵저버  Event -> sendBroadcast()에 전달되는 Intent.

![android_broadcastreceiver](/Users/jeonghyeonji/Desktop/android_broadcastreceiver.png)

- Context 에는 registerReceiver()와 unregisterReceiver() 메서드가 있다. 이는 액티비티, 서비스, Application 에서 사용될 수 있다. 각 컴포넌트는 실행중인 상태에서 브로드캐스트를 받으려고 할 때 브로드캐스트 리시버d를 등록한다.



## 8.1 BroadCastReceiver 구현

> BroadCastReceiver에는 추상 메서드가 onReceive() 하나뿐이다.

```java
public abstract class BroadcastReceiver {
    public BroadcastReceiver() {
        throw new RuntimeException("Stub!");
    }

    public abstract void onReceive(Context var1, Intent var2);

    public final BroadcastReceiver.PendingResult goAsync() {
        throw new RuntimeException("Stub!");
    }

    public IBinder peekService(Context myContext, Intent service) {
        throw new RuntimeException("Stub!");
    }
// ...이하생략
```

그리고 문득 `onReceice` 가 궁금해진다.

```java
  /**  public final class LocalBroadcastManager 클래스에 있는 메서드 
     * Like {@link #sendBroadcast(Intent)}, but if there are any receivers for
     * the Intent this function will block and immediately dispatch them before
     * returning.
     {@link #sendBroadcast (Intent)}와 비슷하지만 수신자가있는 경우
     *이 함수는 의도를 차단하고 즉시 그들을 파견합니다.
     */
    public void sendBroadcastSync(Intent intent) {
        if (sendBroadcast(intent)) {
            executePendingBroadcasts();
        }
    }

    private void executePendingBroadcasts() {
        while (true) {
            final BroadcastRecord[] brs;
            synchronized (mReceivers) {
                final int N = mPendingBroadcasts.size();
                if (N <= 0) {
                    return;
                }
                brs = new BroadcastRecord[N];
                mPendingBroadcasts.toArray(brs);
                mPendingBroadcasts.clear();
            }
            for (int i=0; i<brs.length; i++) {
                final BroadcastRecord br = brs[i];
                final int nbr = br.receivers.size();
                for (int j=0; j<nbr; j++) {
                    final ReceiverRecord rec = br.receivers.get(j);
                    if (!rec.dead) {
                        rec.receiver.onReceive(mAppContext, br.intent);
                    }
                }
            }
        }
    }
```

