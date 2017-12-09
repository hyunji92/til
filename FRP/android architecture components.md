# android architecture components

- The most important thing you should focus on is the **separation of concerns** in your app.
- The second -important principle is that you should **drive your UI from a model**, preferably a persistent model. ( ` Models are components that are responsible for handling the data for the app. They are independent from the Views and app components in your app, hence they are isolated from the lifecycle issues of those components. `)


UI 컨트롤러 (액티비티 및 프래그먼트)는 최대한 가볍게 유지하십시오. 그들은 자신의 데이터를 수집하려고해서는 안됩니다. 대신, ViewModel을 사용하여이를 수행하고 LiveData를 관찰하여 변경 사항을 다시보기에 반영하십시오.


UI가 데이터 변경에 따라보기를 업데이트하거나 사용자 동작을 다시 ViewModel에 알리는 것이 데이터 중심 UI를 작성하도록하십시오.


ViewModel 클래스에 데이터 로직을 넣으십시오. ViewModel은 UI 컨트롤러와 나머지 응용 프로그램 사이의 커넥터 역할을해야합니다. 주의 할 점은 ViewModel이 데이터를 가져 오는 것 (예를 들어 네트워크에서)이 아니라는 것입니다. 대신 ViewModel은 해당 작업을 수행 할 적절한 구성 요소를 호출 한 다음 그 결과를 다시 UI 컨트롤러에 제공해야합니다.


데이터 바인딩을 사용하면 뷰와 UI 컨트롤러간에 깨끗한 인터페이스를 유지할 수 있습니다. 이를 통해보다 많은 선언문을 만들고 활동 및 단편에 작성해야하는 업데이트 코드를 최소화 할 수 있습니다. Java에서이 작업을 수행하려면 버터 나이프와 같은 라이브러리를 사용하여 상용구 코드를 피하고 더 나은 추상화를 수행하십시오.

UI가 복잡한 경우 UI 수정을 처리하기 위해 Presenter 클래스를 만드는 것이 좋습니다. 이것은 일반적으로 잔인한 일이지만 UI를 더 쉽게 테스트 할 수 있습니다.
ViewModel에서보기 또는 활동 컨텍스트를 참조하지 마십시오. ViewModel이 활동을 초과하면 (구성이 변경된 경우) 활동이 유출되고 제대로 가비지 수집되지 않습니다.





LiveData 개체로 작업하려면 다음 단계를 수행하십시오.

1. 특정 유형의 데이터를 보유 할 LiveData의 인스턴스를 만듭니다. 이것은 대개 ViewModel 클래스 내에서 수행됩니다.
2. onChanged () 메서드를 정의하는 Observer 객체를 만듭니다.이 메서드는 LiveData 객체의 보유 데이터가 변경 될 때 수행되는 작업을 제어합니다. 일반적으로 활동 또는 단편과 같은 UI 컨트롤러에서 Observer 객체를 만듭니다.
3. observe () 메서드를 사용하여 Observer 객체를 LiveData 객체에 연결합니다. observe () 메서드는 LifecycleOwner 객체를 사용합니다. Observer 객체를 LiveData 객체에 구독하여 변경 사항을 알립니다. 일반적으로 Observer 객체는 액티비티 또는 프래그먼트와 같은 UI 컨트롤러에 연결합니다.

참고 : observeForever (Observer) 메소드를 사용하여 연관된 LifecycleOwner 객체없이 관찰자를 등록 할 수 있습니다. 이 경우 관찰자는 항상 활성화 된 것으로 간주되므로 수정 사항에 대해 항상 통보됩니다. removeObserver (Observer) 메소드를 호출하는 이들 옵저버를 제거 할 수 있습니다.
LiveData 개체에 저장된 값을 업데이트하면 연결된 LifecycleOwner가 활성 상태에있는 한 등록 된 모든 관찰자가 트리거됩니다.

LiveData를 사용하면 UI 컨트롤러 옵저버가 업데이트를 구독 할 수 있습니다. LiveData 객체가 보유한 데이터가 변경되면 UI가 자동으로 업데이트됩니다.



Room에 LiveData 사용.
Room 지속성 라이브러리는 LiveData 객체를 반환하는 관찰 가능한 쿼리를 지원합니다. 관찰 가능한 쿼리는 DAO (Database Access Object)의 일부로 작성됩니다.

Room은 데이터베이스가 업데이트 될 때 LiveData 객체를 업데이트하는 데 필요한 모든 코드를 생성합니다. 생성 된 코드는 필요할 때 백그라운드 스레드에서 비동기 적으로 쿼리를 실행합니다. 이 패턴은 데이터베이스에 저장된 데이터와 동기화 된 UI에 표시된 데이터를 유지하는 데 유용합니다. 룸 및 DAO에 대한 자세한 내용은 방 영구 라이브러리 안내서를 참조하십시오.



안드로이드 프레임 워크는 액티비티와 프래그먼트와 같은 UI 컨트롤러의 라이프 사이클을 관리합니다. 프레임 워크는 특정 사용자 동작 또는 사용자 제어에서 완전히 벗어난 장치 이벤트에 대한 응답으로 UI 컨트롤러를 파괴하거나 다시 작성하기로 결정할 수 있습니다.

시스템이 UI 컨트롤러를 파괴하거나 다시 만들면 임시 UI 관련 데이터가 손실됩니다. 예를 들어 앱에 사용자의 활동 중 하나에 대한 사용자 목록이 포함될 수 있습니다. 구성 변경을 위해 활동이 다시 작성되면 새 활동이 사용자 목록을 다시 가져와야합니다. 단순한 데이터의 경우 onCreate ()에서 onSaveInstanceState () 메서드를 사용하고 번들에서 해당 데이터를 복원 할 수 있지만이 방법은 직렬화 및 직렬화 될 수있는 소량의 데이터에만 적합하며 잠재적으로 많은 양의 데이터에는 적합하지 않습니다 사용자 또는 비트 맵 목록과 같습니다.

또 다른 문제점은 UI 컨트롤러가 비동기 호출을 자주 만들어서 반환하는 데 시간이 걸릴 수 있다는 것입니다. UI 컨트롤러는 이러한 호출을 관리하고 잠재적 인 메모리 누수를 방지하기 위해 시스템이 손상된 후에 시스템을 정리해야합니다. 이 관리에는 많은 유지 관리가 필요하며 구성 변경을 위해 개체가 다시 만들어지는 경우 개체가 이미 만든 호출을 다시 발행해야하기 때문에 리소스가 낭비됩니다.

활동 및 단편과 같은 UI 컨트롤러는 주로 UI 데이터를 표시하거나, 사용자 작업에 반응하거나, 권한 요청과 같은 운영 체제 통신을 처리하기위한 것입니다. UI 컨트롤러가 데이터베이스 또는 네트워크에서 데이터를로드하는 작업을 수행하도록 요구하면 클래스에 부 풀림이 추가됩니다. UI 컨트롤러에 과도한 책임을 할당하면 작업을 다른 클래스에 위임하는 대신 응용 프로그램의 모든 작업을 자체적으로 처리하려고하는 단일 클래스가 발생할 수 있습니다. 이런 식으로 UI 컨트롤러에 과도한 책임을 할당하면 테스트가 훨씬 더 어려워집니다.

UI 컨트롤러 로직에서 뷰 데이터 소유권을 분리하는 것이 더 쉽고 효율적입니다.