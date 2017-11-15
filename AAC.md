# Android Architecture Components

개발자들이 복잡한 생명주기 및 권장 앱 아키텍쳐에 대한 결여와 같은문제가 있다는 것을 많이 피력해 왔습니다.

그리하여 구글에서 공식적으로 AAC를 제공하게되었습니다.

액티비티와 프래그먼트의 생명주기를 자동으로 관리하여 메모리와 리소스의 누수를 방지하도록 하고 SQLite 데이터베이스에 자바 테이터 객체를 유지하도록 해주는 AAC를 함께 보도록 할께요.

> step1에서 프로젝트 세팅을 마치신후 step2로 넘어가 ViewModel 을 add 해보도록 하겠습니다.
>
> 안드로이드는 화면을 세로에서 가로로 변경할시 액티비티가 다시 실행되어 onCreate부터 다시 타게 되는데요.
>
> step2에서는 ViewModel을 사용하여 화면 회전전체에서 상태를 유지하고 이전 단계에서 관찰한동작을 해결합니다. 이전단계에서는 이미지로 보셨다시피 타이머가 실행되어 있죠!
>
> 이 타이머는 화면 회전과 같이 작업이 삭제되었을때  재설정 됩니다. viewModel을 사용하면 액티비티 또는 프래그 먼트의 전체 라이프 사이클에 걸쳐서 데이터를 유지할 수 있습니다!
>
> 액티비티와 프래그먼트는 사용자가 앱을 사용할때 자주 생성되고 파괴되는 수명이 짧은 개체이기 때문에 타이머와 같은 지속되어야 하는 작업을 하기에는 적합하지 않습니다.
>
> 그래서 사용하게 되는 Viewmodel은 네트워크 통신 뿐만 아니라 데이터의 지송성과 조작에 관련한 작업을 하는데 아주 적합하다고 할 수있습니다.
>
> `chronoActivity2` 를 열어볼까요? 클래스가 ViewModel을 검색하여 사용하는 방법을 확인해 보려고합니다.
>
> `chronoActivity2`의  사이트에 나온 this는  LifeCycleOwner의 인스턴스를 넘김니다. `of`메서드에 라이프사이클 오너의 인스턴스를 넘기면  ViewModelProvider instance 를 return하게 됩니다.
>
> 프레임 워크에서 LifeCycleOwner의 범위, 생명주기가 살아있는한은 ViewModel을 활성 상태로 유지합니다. 그럼 이 코드랩의 코드에서 보면 ViewModel은 화면 회전과 같이 소유자가 삭제된 경우에도 데이터가 삭제되지 않고  새인스턴스가 기존 ViewModel에 다시 연결 됩니다.
>
> 그림을 보시면 액티비티가 삭제되고 다시 실행 되는 플로우에도 뷰모델은 그대로 스코프를 유지하는 것을 보실 수 있습니다.
>
> step2를 실행해보시고 앱을 나왔다 다시들어가거나 가로세로를 바꿔보시고 제대로 작동하는지 확인해보세요!



# ViewModel

액티비티에 관련된 UI 데이터 접근을 제공하고 LiveData, LifeCycle컴포넌트와 결합하도록 설계되었습니다. ViewModle은 액티비티가 정리 될 때까지 살아있게 됩니다.

ViewModel은 Activity또는 Fragment에 대한 UI데이터를 포함하는 도우미 클래스 입니다. UI컨트롤러 로직과 데이터를 보기위한 내용을 분리하는 역할을 하는거죠.

ViewModel은 구성변경으로 인해 Activity/Fragment가 제거되고, 다시 생성되는 경우를 포함하여 해당 Activity/Fragment의 범위가 활성 상태인 동안만 유지됩니다.이를 통해 ViewModel은 다시 생성된 활동 또는 프래그먼트 인스턴스ㅇ에서 UI데이터를 사용 가능하도록 할 수 있습니다. 

LiveData를 사용하여 Viewmodel내에 저장된 UI데이터를 래핑하면 데이터가 인식 가능한 생명주기 코드에(홈에) 제공됩니다. LiveData는 알림과 관련된 사항을 처리하는 반면, ViewModel은 데이터가 적절하게 유지되도록 합니다.



> step3에서는  LiveData를 사용하여 데이터를 감싸볼껍니다. 3단계에서는 이전단계에서 사용된 메트로놈 타이머를 사용자 정의 카메라고 바꾸어 1초바다 UI작업을 업데이트하게 합니다. timer 를 LiveDataTimerViewModel클래스에 추가하고 사용자와 UI간의 인터렉트을 관리하도록 남겨두고요!
>
> 타이머가 알림을 보내면 UI를 업데이트하도록하고, 메모리 누수를 피하기위해 ViewModel에는 활동에 대한 참조가 포함되지 않도록 합니다.  예를들면 화면의 로테이션이 바뀌는 사황이 왔을때 뷰모델이 없어져야할 작업이 참조될 수 도 있기때문이죠.  뷰모델은 해당 액티비티 또는 라이프 사이클오너가 더이상 존재하지 않을때까지 인스턴스를 보유한다고 했으니까요!
>
> 메모리 릭을 불러올 수 있기때문에 조심해야할 것은 뷰모델이 컨텍스트나 뷰에 강한 참조를 저장하면 안됩니다! 
>
> 이렇게 지금까지 보신 예제를 보시면 뷰모델에서 직접 뷰를 수정하지 않고 데이터 코드만 observe관찰 하도록 해서 액티비티/프래그먼트를 구성하고 변경된 데이터를 수신하는것을 옵저버 패턴이라고 하죠.
>
> 다음으로는 LiveData를 알아볼껀데요 여러분이 현업이나 프로젝트에서 RXJava를 사용해보셨다면 옵저버패턴을 사용해 보셨을 텐데요, 그렇다면 이내용이 이해가 더 잘 가실 수 있을 것 같습니다

# LiveData

>LiveData는 생명주기를 인식하는 특수한 관찰가능 클래스이며 active 한 observer에게만 변경된 데이터를 알립니다.
>
>먼저 사이트에 제가 전에 언급한 LifeCycleOwner가 나오는데요, 내용을 보시면.
>
>`ChronoActivity3`는 수명주기 상태를 제공 할 수있는 LifecycleActivity의 인스턴스입니다. 아래의 코드선언을 보시죠.
>
>LifecycleRegistryOwner는 ViewModel 및 LiveData 인스턴스의 생명주기를 액티비티 / 프래그먼트에 바인딩하는 데 사용됩니다. 프래그먼트의 해당하는 클래스는 LifecycleFragment입니다.
>
>chronoActivity를 업데이트 해볼가요.ChronoActivity3에 아래 코드를 따라해보세요 . 다운로드 받으신 프로젝트에 액티비티 3를 들어가셔서 subscrivbe()메서드 안에 투두에 작업을 하시면 됩니다. 그리고 LiveDataTimerViewModel 클래스에서 새 경과 시간 값을 설정할건데요. 다음 주석을 찾아보시면 됩니다.
>
>ㄱ리고 앱을 실행해 보시게 되면  다른 앱으로 이동하지 않는 한 로그가 1 초마다 업데이트됩니다. 화면을 회전해도 앱 작동 방식에는 영향을 미치지 않을것입니다.!

LiveData 개체는 활동 또는 LifecycleOwner가 활성 상태 일 때만 업데이트를 보냅니다. 다른 앱으로 이동하면 돌아올 때까지 로그 메시지가 일시 중지됩니다. LiveData 개체는 해당  LifeCycleOwner가 STARTED 또는 RESUMED 일 때만 구독을 활성으로 간주합니다.

LIveData는 Activity가 ViewModel의 변경을 폴링할 필요 없이 Stream data를 ViewModel로 부터 Activity나 UI로 보내줍니다.Activity는 LiveData에 연동하여 데이터 변경에 대처합니다. LiveData로 모든 데이터를 나타낼 수 있습니다. 생명주기를 인식하도록 컴포넌트를 만들 수 있고 앱의 요구사항에 맞게 ViewModel도 만들 수 있습니다.

LivaData는 식별가능한 수명주기 인식 데이터 홀더 클래스입니다.

- 수명 주기가 활성 상태(STARTED 또는 RESUMED)인 동안 관찰자가 데이터에 대한 업데이트를 가져옴
- [LifecycleOwner](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html)가 제거될 때 관찰자가 제거됨
- [LifecycleOwner](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html)가 구성 변경으로 인해 다시 시작되거나 백 스택에서 다시 시작되는 경우 관찰자가 최신 데이터를 가져옴

이는 메모리 누수를 일으키는 많은 경로를 제거하는 데 도움이 되고 중지된 액티비티에 대한 업데이트를 방지함으로써 충돌을 줄여 줍니다.

[LiveData](https://developer.android.com/topic/libraries/architecture/livedata.html)는 Fragment 또는 Activity와 같은 수명 주기 소유자와 연결된 많은 리스너에 의해 관찰될 수 있습니다.



> step4에서는 라이브사이클이 이벤트를 구독하는 것을 해볼건데요.
>
> LifeCycleOwner는 lifecycle-aware  구성 요소의 새 인스턴스로 전달되어 라이프 사이클의 현재 상태를 알 수 있도록합니다.
>
> 사이트의 아래 코드를 사용하여 라이프 사이클의 현재 상태를 쿼리 할 수 있습니다. 현재의 state를 알수있는거죠.
>
> 

# LifeCycle

일반적인 Android 관찰 모델은 [onStart()](https://developer.android.com/reference/android/app/Activity.html#onStart())에서 관찰을 시작하고 [onStop()](https://developer.android.com/reference/android/app/Activity.html#onStop())에서 관찰을 중지합니다.  이는 아주 간단한 얘기로 들리지만, 한 번에 여러 개의 비동기 호출이 수행되는 경우가 종종 있고,  이러한 비동기 호출은 모두 구성 요소의 수명 주기를 관리하는 작업을 담당합니다.생명주기를 인지하는 컴포넌트를 쉽게 만들수 있게 여러 클래스와 인터패이스를 Lifecycle 패키지가 제공합니다.

[LifecycleOwner](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html)는 [getLifecycle()](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html#getLifecycle()) 메서드에서 Lifecycle 객체를 반환하는 인터페이스인 반면, [LifecycleObserver](https://developer.android.com/reference/android/arch/lifecycle/LifecycleObserver.html)는 해당 메서드에 주석을 추가함으로써 구성 요소의 수명 주기 이벤트를 모니터링할 수 있는 클래스입니다. 이를 모두 결합하면 수명 주기 이벤트를 모니터링하고 현재 수명 주기 상태를 쿼리할 수 있는 Lifecycle-aware 컴포넌트를 생성할 수 있습니다.

```@OnLifecycleEvent(Lifecycle.Event.ON_RESUME)```





# Room / 데이터 지속성(Persistence)

Room은 SQLite를 완벽히 활용하면서 원할 한 데이터베이스 엑세스를 지원하는 객체 매핑 추상화 계층을 제공합니다. (Object-mapping abstraction layer) 



Room은 개발자가 자료를 질의 하기 위한  SQL을 직접 작성해야합니다. 그리고 반환된 객체에 자식에 대한 lazy loading을 지원하지 않습니다.

이것은 개발자에게 보이지 않게 SQL을 생성하는 많은 ORM에 비해 실질적인 강점이기는 합니다. 비효율적인 질의를 하지 않게 되기때문이죠.

### 데이터베이스, 엔터티 및 DAO

Room에는 다음과 같은 세 가지 주요 구성 요소가 있습니다.

- [엔터티](https://developer.android.com/reference/android/arch/persistence/room/Entity.html) - 단일 데이터베이스 행에 대한 데이터를 나타내며, 주석이 추가된 자바 데이터 객체를 사용하여 생성됩니다. 엔티티는 각각 자체적인 테이블에 유지됩니다. 

```java
@Entity
public class User {
    @PrimaryKey
    private int uid;
    private String name;
    // Getters and Setters - required for Room
  	// Room을 사용하려면 엔티티로 유지할 자바 데이터 객체에 주석을 지정
    public int getUid() { return uid; }
    public String getName() { return name; }
    public void setUid(int uid) { this.uid = uid; }
    public void setName(String name) { this.name = name; }
}
```



- [DAO](https://developer.android.com/reference/android/arch/persistence/room/Dao.html)(데이터 액세스 객체) - 데이터베이스에 접근하는 메서드를 정의하여 주석을 사용해 SQL을 각 메서드에 연결합니다.

```java
// 데이터베이스를 엑세스 및 수정하기 위한 SQL 로 DAO클래스를 정의
@Dao
public interface UserDao {
    @Query("SELECT * FROM user")
    List getAll();
    @Insert
    void insertAll(User... users);
}
```



- [데이터베이스](https://developer.android.com/reference/android/arch/persistence/room/Database.html) - 주석을 사용하여 엔티티 밎 데이터베이스 버전의 목록을 정의하는 홀더 클래스. 이 클래스에서 컨텐츠는 DAO의 목록을 정의합니다. 또한, 기본 데이터베이스 연결을 위한 기본 엑세스 지점이기도 합니다.

```java
// 이러한 엔티티를 포함한 데이터 베이스를 생성
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```



![Screen Shot 2017-05-17 at 5.08.05 PM](/Users/jeonghyeonji/Downloads/Screen Shot 2017-05-17 at 5.08.05 PM.png)

####**Room을 사용하려면 엔티티로 유지할 자바 데이터 객체에 주석을 지정하고 이러한 엔티티를 포함한 데이터 베이스를 생성한후, 데이터베이스를 엑세스 및 수정하기 위한 SQL 로 DAO클래스를 정의합니다.**





