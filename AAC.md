# Room

Room은 개발자가 자료를 질의 하기 위한  SQL을 직접 작성해야합니다. 그리고 반환된 객체에 자식에 대한 lazy loading을 지원하지 않습니다.

이것은 개발자에게 보이지 않게 SQL을 생성하는 많은 ORM에 비해 실질적인 강점이기는 합니다. 비효율적인 질의를 하지 않게 되기때문이죠



# ViewModel

액티비티에 관련된 UI 데이터 접근을 제공하고 LiveData, LifeCycle컴포넌트와 결합하도록 설계되었습니다. ViewModle은 액티비티가 정리 될 때까지 살아있게 됩니다.



# LifeCycle

일반적인 Android 관찰 모델은 [onStart()](https://developer.android.com/reference/android/app/Activity.html#onStart())에서 관찰을 시작하고 [onStop()](https://developer.android.com/reference/android/app/Activity.html#onStop())에서 관찰을 중지합니다.  이는 아주 간단한 얘기로 들리지만, 한 번에 여러 개의 비동기 호출이 수행되는 경우가 종종 있고,  이러한 비동기 호출은 모두 구성 요소의 수명 주기를 관리하는 작업을 담당합니다.생명주기를 인지하는 컴포넌트를 쉽게 만들수 있게 여러 클래스와 인터패이스를 Lifecycle 패키지가 제공합니다.

[LifecycleOwner](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html)는 [getLifecycle()](https://developer.android.com/reference/android/arch/lifecycle/LifecycleOwner.html#getLifecycle()) 메서드에서 Lifecycle 객체를 반환하는 인터페이스인 반면, [LifecycleObserver](https://developer.android.com/reference/android/arch/lifecycle/LifecycleObserver.html)는 해당 메서드에 주석을 추가함으로써 구성 요소의 수명 주기 이벤트를 모니터링할 수 있는 클래스입니다. 이를 모두 결합하면 수명 주기 이벤트를 모니터링하고 현재 수명 주기 상태를 쿼리할 수 있는 Lifecycle-aware 컴포넌트를 생성할 수 있습니다.

```@OnLifecycleEvent(Lifecycle.Event.ON_RESUME)```



# LiveData

LIveData는 Activity가 ViewModel의 변경을 폴링할 필요 없이 Stream data를 ViewModel로 부터 Activity나 UI로 보내줍니다.Activity는 LiveData에 연동하여 데이터 변경에 대처합니다. LiveData로 모든 데이터를 나타낼 수 있습니다. 생명주기를 인식하도록 컴포넌트를 만들 수 있고 앱의 요구사항에 맞게 ViewModel도 만들 수 있습니다.



