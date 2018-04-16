# Android Architecture Components

먼저 Android 앱은 훨씬 복잡한 구조를 가지고 있습니다. 이미지로 보실 수 있듯이 안드로이드 개발자 분이시라면 대표적으로 꼭 알아야하고 중요한 구성요소로 컴포넌트을 보실 수 있습니다.  안드로이드 플랫폼안을 밑에 단까지 알고 쓴다면 더욱 많은것을 알아야하죠.

****

그동안  안드로이드 어플리케이션 개발을 위한 특별히 권장되는 아키텍처가 없었습니다. 

그리하여 현업에 개발자들을 마음대로 개발하기도 하고,

MVC나 MVP,MVVM 등의 개발 패턴을 도입하는 곳도 있었습니다.

****

이렇듯 안드로이드 프레임워크로 시스템 및 구성요소의 생명주기 문제에 대한 고유한 솔루션이 없었습니다.

이런 불편한 사항에도 불구하고 안드로이드의 표준 아키텍처 패턴의 부재는 오래 지속되어 왔는데요

이제는 구글에서 공식적으로 AAC - 즉 안드로이드 아키텍처 컴포넌트를 제공하여 전세계에 안드로이드 어플리케이션의 아키텍처에 대한 공식 권장사항을 발표하고 이를 구현할 수 있도록 가이드를 제시해 주게 되었습니다.

****

AAC를 간단하게 설명하면 액티비티와 프래그먼트의 생명주기를 자동으로 관리하여 메모리와 리소스의 누수를 방지하도록 하고 SQLite 데이터베이스에 자바 테이터 객체를 유지하도록 가이드를 제시하는데요. 오늘 여러분과 코드랩을 진행해 보면서 자세한 사항을 알아보려고 합니다.

****

코드랩을 진행하기 얼마전 AAC가 드디어 정식 발표가 되었는데요. ```Now 1.0 stable``` 업데이트 된사항에 deplicate 된 부분이 있습니다.  ( 코드로 본다 )

LifeCycleActity -> AppCompatActivity

LifeCycleRegistryOwner -> LifeCycleOwner 로 변경되어 적용되었습니다.

 `AppCompatActivity` 코드를 타고가 보시면 FragmentActivity를 extends 한걸 보실 수 있는데요! 다시 FragmentActivity로 타고들어가서 import 코드를 살펴보시면 `import android.arch.lifecycle.Lifecycle;`가 임포트 되신거를 보실 수 있습니다!  

```Java
1- public class LocationActivity extends AppCompatActivity {
```

```Java
2- public class AppCompatActivity extends FragmentActivity 
```

```java
3- import android.arch.lifecycle.Lifecycle;
```

> step1에서 가단한 프로젝트 세팅을 마치신후 step2로 넘어가 본격적으로 코드를 볼텐데요. 가이드에 나와있는데로  ViewModel 을 add 해보도록 하겠습니다.
>
> ViewModel을 사용하여 액티비티 또는 프래그먼트의 전체 라이프 사이클에 걸쳐 데이터를 유지할 수 있습니다.
>
> 이번 코드랩을 진행할 예제는 타이머 인데요 . 

안드로이드에서 라이프사이클은 사용자의 액션에 따라 자주 변경되는데, 대표적인 예로 화면 전환을 예로 들 수 있습니다. 화면전환을 하게되면 configuration이 바뀌게 되는데 이유는 액티비티가 완전히 새로 재시작, reCreate 되기 때문인데요. 액티비티가 재시작 될때 데이터를 제대로 저장하고 데이터를 복원하지 않았을때 , 여러분은 생명주기를 고려하지않은 코드로인해 버그를 마주보게 될지도 모릅니다 ㅠ.

> 이렇게 step2에서는 ViewModel로 해결방안을 제시합니다.  ViewModel을 사용하여 화면 회전 전체에서 상태를 유지하고 이전 단계에서 관찰한동작을 해결합니다. 이전단계에서는 이미지로 보셨다시피 타이머가 실행되어 있죠! 
>
> 이 타이머는 화면 회전과 같이 작업이 삭제되었을때  재설정 됩니다. viewModel을 사용하면 액티비티 또는 프래그 먼트의 전체 라이프 사이클에 걸쳐서 데이터를 유지할 수 있는데요,액티비티에 관련된 UI 데이터 접근을 제공하고 LiveData, LifeCycle컴포넌트와 결합하도록 설계되었습니다. ViewModle은 액티비티가 정리 될 때까지 살아있게 됩니다.

> 이제 ViewModel을 사용하여 타이머의 상태 유지를 할 수 있는 코드를 함께 짜보려고합니다.
>
> `chronoActivity2` 를 열어볼까요? 클래스가 ViewModel을 검색하여 사용하는 방법을 확인해 보려고합니다.
>
> `chronoActivity2`의  사이트에 나온 this는  LifeCycleOwner의 인스턴스를 넘김니다.

```Java
ChronometerViewModel chronometerViewModel
        = ViewModelProviders.of(this).get(ChronometerViewModel.class);
```

위의 코드의 ViewModelProviders는 액티비티의 인스턴스를 제공합니다. 이것이 디바이스를 회전하더라도 구성이 변경되지 않도록 할 수 있는 메커니즘이라고 할 수 있는데요. 기술적으로 새로운 인스턴스를 제공하고, 인스턴스가 항상 연결되어 있는지 보장하도록 합니다.

ViewModel를 통해 어떻게 데이터를 유지할 수 있냐하면은,  코드의 분리로 데이터를 유지할 수 잇는 것인데요, 액티비티의 활동을 위한 모든 UI데이터를 가지고 있도록합니다. 그렇게되면 액티비티에서는 UI화면에 데이터를 어떻게 나타낼지와 사용자의 인터랙션만을 관리하도록 하게 합니다.

> 가이드에서는 액티비티와 프래그먼트는 사용자가 앱을 사용할때 자주 생성되고 파괴되는 수명이 짧은 개체이기 때문에 타이머와 같은 지속되어야 하는 작업을 하기에는 적합하지 않습니다.
>
> 그래서 사용하게 되는 Viewmodel은 네트워크 통신 뿐만 아니라 데이터의 지속성과 조작에 관련한 작업을 하는데 아주 적합하다고 할 수있습니다.

**한가지더 주의할 점은 ViewModel은 onSaveInstanceState를 대체하지 않습니다. 뷰모델은 configuration이 바뀔때 데이터가 유지되지만 핸드폰자원의 제약으로 인한 프로세스관련  종료가 일어나게 되면 그것이 앱에 계속 살아남는다는 의미는 아닙니다.**

**하지만 onSaveInstanceState는  UI상태 프로세스가 프레임워크에의해 종료된 경우 실제로 복원할 필요가 있는 최소량의 데이터를 저장하기 위한 것으로, 예를 들면 객체에 대한 데이터베이스 아이디를 저장하기 위해서인데요 ,완전한 객체 말고 말이죠.**

주의하실 점이 있다면 액티비티 또는 프래그먼트의 범위는 생성 된 상태에서 완료된 상태 (또는 종료 된 상태)로 진행되며, 파괴 된 상태로 혼동해서는 안됩니다. 장치가 회전 할 때, Activity가 파괴되지만 그와 관련된 뷰 모델의 인스턴스가 파괴되는것은 아니다. *the activity is destroyed but any instances of `ViewModel` (associated with it )are not.* #??



> 프레임 워크에서 LifeCycleOwner의 범위, 생명주기가 살아있는한은 ViewModel을 활성 상태로 유지합니다. 그럼 이 코드랩의 코드에서 보면 ViewModel은 화면 회전과 같이 소유자가 삭제된 경우에도 데이터가 삭제되지 않고  새인스턴스가 기존 ViewModel에 다시 연결 됩니다.
>
> 그림을 보시면 액티비티가 삭제되고 다시 실행 되는 플로우에도 뷰모델은 그대로 스코프를 유지하는 것을 보실 수 있습니다.
>
> step2를 실행해보시고 앱을 나왔다 다시들어가거나 가로세로를 바꿔보시고 제대로 작동하는지 확인해보세요!

**ViewModel**

액티비티에 관련된 UI 데이터 접근을 제공하고 LiveData, LifeCycle컴포넌트와 결합하도록 설계되었습니다. ViewModle은 액티비티가 정리 될 때까지 살아있게 됩니다.

ViewModel은 Activity또는 Fragment에 대한 UI데이터를 포함하는 도우미 클래스 입니다. UI컨트롤러 로직과 데이터를 보기위한 내용을 분리하는 역할을 하는거죠.

ViewModel은 구성변경으로 인해 Activity/Fragment가 제거되고, 다시 생성되는 경우를 포함하여 해당 Activity/Fragment의 범위가 활성 상태인 동안만 유지됩니다.이를 통해 ViewModel은 다시 생성된 활동 또는 프래그먼트 인스턴스ㅇ에서 UI데이터를 사용 가능하도록 할 수 있습니다. 

****



> step3에서는  LiveData를 사용하여 데이터를 감싸볼껍니다. 3단계에서는 이전단계에서 사용된 메트로놈 타이머를 사용자 정의 카메라고 바꾸어 1초바다 UI작업을 업데이트하게 합니다. 
>
> LiveData를 사용하여 Viewmodel내에 저장된 UI데이터를 래핑하면 데이터가 인식 가능한 생명주기 코드에(홈에) 제공됩니다. LiveData는 알림과 관련된 사항을 처리하는 반면, ViewModel은 데이터가 적절하게 유지되도록 합니다.
>
> timer 를 LiveDataTimerViewModel클래스에 추가하고 사용자와 UI간의 인터렉트을 관리하도록 남겨두고요!
>
> 타이머가 알림을 보내면 UI를 업데이트하도록하고, 메모리 누수를 피하기위해 ViewModel에는 액티비티에 대한 참조가 포함되지 않도록 합니다.  예를들면 화면의 로테이션이 바뀌는 사황이 왔을때 뷰모델이 없어져야할 작업이 참조될 수 도 있기때문이죠.  뷰모델은 해당 액티비티 또는 라이프 사이클오너가 더이상 존재하지 않을때까지 인스턴스를 보유한다고 했으니까요!
>
> **메모리 릭을 불러올 수 있기때문에 조심해야할 것은 뷰모델이 컨텍스트나 뷰에 강한 참조를 저장하면 안됩니다! (만약 뷰모델에서 컨텍스트를 사용하고 싶으시다면 context - android ViewModel 을 사용하 실 수 있습니다.) **
>
> 이렇게 지금까지 보신 예제를 보시면 뷰모델에서 직접 뷰를 수정하지 않고 데이터 코드만 observe관찰 하도록 해서 액티비티/프래그먼트의 구성 변경된 데이터를 수신하는것을알수 있고,
>
> 이는  옵저버 패턴이라고 하죠.-
>
> 여러분이 현업이나 프로젝트에서 RXJava를 사용해보셨다면 옵저버패턴을 사용해 보셨을 텐데요, 그렇다면 이내용이 이해가 더 잘 가실 수 있을 것 같습니다.

LifecycleOwner가 상태를 변경하면 알림을받을 LifecycleObserver를 추가합니다.
주어진 관찰자는 LifecycleOwner의 현재 상태로 이동합니다. 예를 들어 LifecycleOwner가 Lifecycle.State.STARTED 상태에 있으면 주어진 관찰자는 Lifecycle.Event.ON_CREATE, Lifecycle.Event.ON_START 이벤트를받습니다.

****

//라이브데이터는 뷰모델 내부에 저장될 수 있고 자동으로 연결하여 통지할 프래그먼트

# LiveData

>LiveData는 생명주기를 인식하는 특수한 관찰가능 클래스이며 active(포그라운드에 있는 액티비티의 상태) 한 observer(Owner)에게만 변경된 데이터를 알립니다.
>
>먼저 사이트에 제가 전에 언급한 LifeCycleOwner가 나오는데요, 내용을 보시면.
>
>`ChronoActivity3`는 수명주기 상태를 제공 할 수있는 LifecycleActivity의 인스턴스입니다. 아래의 코드선언을 보시죠.
>
>LifecycleOwner는 ViewModel 및 LiveData 인스턴스의 생명주기를 액티비티 / 프래그먼트에 바인딩하는 데 사용됩니다. 프래그먼트의 해당하는 클래스는 LifecycleFragment입니다.
>
>chronoActivity를 업데이트 해볼가요.ChronoActivity3에 아래 코드를 따라해보세요 . 다운로드 받으신 프로젝트에 액티비티 3를 들어가셔서 subscrivbe()메서드 안에 투두에 작업을 하시면 됩니다. 그리고 LiveDataTimerViewModel 클래스에서 새 경과 시간 값을 설정할건데요. 다음 주석을 찾아보시면 됩니다.
>
>ㄱ리고 앱을 실행해 보시게 되면  다른 앱으로 이동하지 않는 한 로그가 1 초마다 업데이트됩니다. 화면을 회전해도 앱 작동 방식에는 영향을 미치지 않을것입니다.!

LiveData 개체는 액티비티또는 LifecycleOwner가 활성 상태 일 때만 업데이트를 보냅니다. 다른 앱으로 이동하면 돌아올 때까지 로그 메시지가 일시 중지됩니다. LiveData 개체는 해당  LifeCycleOwner가 STARTED 또는 RESUMED 일 때만 구독을 활성으로 간주합니다.

LIveData는 Activity가 ViewModel의 변경을 폴링할 필요 없이 Stream data를 ViewModel로 부터 Activity나 UI로 보내줍니다. Activity는 LiveData에 연동하여 데이터 변경에 대처합니다. LiveData로 모든 데이터를 나타낼 수 있습니다. 생명주기를 인식하도록 컴포넌트를 만들 수 있고 앱의 요구사항에 맞게 ViewModel도 만들 수 있습니다.

[LiveData](https://developer.android.com/topic/libraries/architecture/livedata.html)는 Fragment 또는 Activity와 같은 라이프사이클 오너와 연결된 많은 리스너에 의해 관찰될 수 있습니다.

> step4에서는 라이브사이클이 이벤트를 구독하는 것을 해볼건데요. 많은 안드로이드는 컴포넌트 또는 라이브러리를 구독하거나 초기화하고 ,구독을 취소하고 중지해야 하는 작업을 개발자가 생명주기를 분석하고 맞추어야했습니다. 그렇지 않으면 메모리 누수 및 미묘한 버그가 발생할 수 있습니다.
>
> 이번 step에서 사용하게될 LifeCycleOwner는 lifecycle-aware  구성 요소의 새 인스턴스로 전달되어 라이프 사이클의 현재 상태를 알 수 있도록합니다.
>
> 사이트의 아래 코드를 사용하여 라이프 사이클의 현재 상태를 쿼리 할 수 있습니다. 현재의 state를 알수있는거죠.

```java
lifecycleOwner.getLifecycle().getCurrentState()
```

위의 코드는 Lifecycle.State.RESUMED 또는 Lifecycle.State.DESTROYED와 같은 상태를 반환합니다. 

LifecycleObserver를 구현하는 lifecycle-aware 객체는 LifeCycleOwner의 상태 변화를 관찰 할 수 있습니다.

```java
lifecycleOwner.getLifecycle().addObserver(this);
```

위의 코드처럼 말죠 ! 코드를 같이 작성하면서 보겠습니다.

 다음 단계에서는 Activity에 반응하는 lifeCycle - aware  컴포넌트를 만들껀데요. 프레그먼트에서 LifeCycleOwner를 사용할때도 이와같은 과정이 적용됩니다.

프로젝트 예제에서 보시는것과 같이 LocationManager를 사용하여 현재 위치를 가져와 사용자에게 표시하도록하는 코드인데요. 아래의 두가지 방법으로 사용이 가능하다.

1. 변경사항을 subsctibe(구독)하고 LiveData를 사용하여 UI자동으로 업데이트 하도록 !


2. 액티비티의 상태 변경에 따라 register그리고unregister하는 LocationManager의 래퍼를 만들어

추가하여 예제를 실행할 수 있습니다. 지금 단계에서는 두번째의 방법으로 예제를 진행한것을 보실 수 있습니다.

코드랩 사이트에서 보실 수 있듯이 액티비티에서 생명주기 메서드를 활용하는 전형적인 코드가 나오는데요.

> step4 에서는 BoundLocationManager라는 클래스에서 LifecycleOwner 구현을 사용합니다.
>
> 클래스가 액티비티의 라이프 사이클을 관찰하려면 observer를 추가해야하고, observer를 수행하기 위해 아래 코드를 생성자에 추가하여 액티비티의 라이프 사이클을 관찰하도록 BoundLocationManager객체에 지시하도록 하는 코드입니다.

```java
lifecycleOwner.getLifecycle().addObserver(this);
```

> 그리고 라이프 사이클을 변경될 때 동작해야하는 메서드를 호출하려면 ```@OnLufeCycleEvent``` 어노테이션을 사용하게 됩니다.BoundLocationListener 클래스의 다음 주석을 사용하여 addLocationListener () 및 removeLocationListener () 메서드를 업데이트합니다.
>
> 코드에 주석과 함꼐 설명합니다.

 

> Android 에뮬레이터를 사용하여 기기의 위치 변경을 시뮬레이션합니다 (확장 된 컨트롤을 표시하려면 세 개의 점을 클릭하십시오). 텍스트 뷰가 변경되면 업데이트됩니다.

# LifeCycle

일반적인 Android 관찰 모델은 [onStart()](https://developer.android.com/reference/android/app/Activity.html#onStart())에서 관찰을 시작하고 [onStop()](https://developer.android.com/reference/android/app/Activity.html#onStop())에서 관찰을 중지합니다.  이는 아주 간단한 얘기로 들리지만, 한 번에 여러 개의 비동기 호출이 수행되는 경우가 종종 있고,  이러한 비동기 호출은 모두 구성 요소의 수명 주기를 관리하는 작업을 담당합니다.생명주기를 인지하는 컴포넌트를 쉽게 만들수 있게 여러 클래스와 인터패이스를 Lifecycle 패키지가 제공합니다.

[LifecycleOwner](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html)는 [getLifecycle()](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html#getLifecycle()) 메서드에서 Lifecycle 객체를 반환하는 인터페이스인 반면,

>  [LifecycleObserver](https://developer.android.com/reference/android/arch/lifecycle/LifecycleObserver.html)는 해당 메서드에 어노테이션을 추가함으로써 구성 요소의 수명 주기 이벤트를 모니터링할 수 있는 클래스입니다. 이를 모두 결합하면 수명 주기 이벤트를 모니터링하고 현재 수명 주기 상태를 쿼리할 수 있는 Lifecycle-aware 컴포넌트를 생성할 수 있습니다.

```@OnLifecycleEvent(Lifecycle.Event.ON_RESUME)```

![스크린샷 2017-11-16 오후 8.42.40](/Users/jeonghyeonji/Desktop/스크린샷 2017-11-16 오후 8.42.40.png)



> 마지막으로 보실 step5는 프래그먼트와 아래 나오는 사항의 통신을 가능하게 하기 위해 viewModel을 사용하여 단계를 진행해 보려고합니다.
>
> 진행 할 사항들을 보면
>
> - 싱글 액티비티
> - 프래그먼트의 두개의 인스턴스 (각 인스턴스는 SeekBar가 있습니다.)
> - LivaData가 있는 단일 ViewModel

사이트에 나오는 앱을 실행해서  서로 독립적 인 SeekBar의 두 인스턴스를 확인합니다.

그럼 코드에서 하나의 SeekBar가 변경 될 때 다른 SeekBar가 업데이트되도록 ViewModel에 프래그먼트을 연결하는 과정을 보도록 하겠습니다.

NOTE : 각 프래그먼트의 라이프 사이클은 독립적이므로 LifeCycleOwner로 액티비티를 사용해야합니다.

****





# Room / 데이터 지속성(Persistence)

Room은 SQLite를 완벽히 활용하면서 원할한 데이터베이스 엑세스를 지원하는 객체 매핑 추상화 계층을 제공합니다. (Object-mapping abstraction layer) 

Room은 개발자가 자료를 질의 하기 위한  SQL을 직접 작성해야합니다. 그리고 반환된 객체에 자식에 대한 lazy loading을 지원하지 않습니다.

이것은 개발자에게 보이지 않게 SQL을 생성하는 많은 ORM에 비해 실질적인 강점이기는 합니다. 비효율적인 질의를 하지 않게 되기때문이죠.

### 데이터베이스, 엔터티 및 DAO

Room에는 다음과 같은 세 가지 주요 구성 요소가 있습니다.

- [엔터티](https://developer.android.com/reference/android/arch/persistence/room/Entity.html) - 단일 데이터베이스 행에 대한 데이터를 나타내며, 어노테이션이 추가된 자바 데이터 객체를 사용하여 생성됩니다. 엔티티는 각각 자체적인 테이블에 유지됩니다. 

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



- [데이터베이스](https://developer.android.com/reference/android/arch/persistence/room/Database.html) - 어노테이션을 사용하여 엔티티 밎 데이터베이스 버전의 목록을 정의하는 홀더 클래스. 이 클래스에서 컨텐츠는 DAO의 목록을 정의합니다. 또한, 기본 데이터베이스 연결을 위한 기본 엑세스 지점이기도 합니다.

```java
// 이러한 엔티티를 포함한 데이터 베이스를 생성
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```



![Screen Shot 2017-05-17 at 5.08.05 PM](/Users/jeonghyeonji/Downloads/Screen Shot 2017-05-17 at 5.08.05 PM.png)

####**Room을 사용하려면 엔티티로 유지할 자바 데이터 객체에 어노테이션을 지정하고 이러한 엔티티를 포함한 데이터 베이스를 생성한후, 데이터베이스를 엑세스 및 수정하기 위한 SQL 로 DAO클래스를 정의합니다.**





