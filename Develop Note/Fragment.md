# Fragment

> Fragment는 자체 생명주기를 가지고, Activity실행 중에 추가 및 제거가 가능한 액티비티의 모듈식 섹션이다.
>
> 항상 Activity내에 포함되어 있어야 하고 해당 Fragment 생명주기는 호스트Activity 의 수명주기에 직접적으로 영향을 받는다.

Fragment를 Activity layout의 일부로 추가하는 경우, 이는 Activity view계층 내부의 ViewGroup 안에 살고, 해당 Fragment가 자신의 view layout을 정의 한다.