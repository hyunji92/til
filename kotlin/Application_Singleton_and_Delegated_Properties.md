# Application Singleton and Delegated Properties	

안드로이드의 문제점은 우리가 많은 클래스 구조체를 제어 할 수 없다는 것입니다.
예를 들어, 값을 생성자에서 정의해야하므로 nullable이 아닌 속성을 초기화 할 수 없습니다.
따라서 nullable 변수와 nullable 값을 반환하는 함수가 필요합니다. 우리는 항상 App 인스턴스를 가지고 있으며, onCreate 어플리케이션 전에는 제어 할 수있는 것이 없다는 것을 알고 있으므로 instance () 함수는 항상 null이 아닌 App 인스턴스를 반환 할 수 있다고 가정하여 안전합니다. 그러나이 해결책은 약간 부 자연스러운 것처럼 보입니다.
우리는 속성 (getter와 setter가 이미있는)과 그 속성을 반환하는 함수를 정의해야합니다. 비슷한 결과를 얻을 수있는 다른 방법이 있습니까? 예, 속성의 값을 다른 클래스에 위임 할 수 있습니다.  이는 일반적으로 위임 된 속성으로 알려져 있습니다.
