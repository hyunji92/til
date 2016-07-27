# RecyclerView
## 알아보기

5.0 롤리팝과 함께 나타난 RecyclerView. 나는 넘나 뒤늦게 안것.
ListView 를 이용할 때 아주 기초적이고 정석적인 개념으로 사용되던 ViewHolder pattern 을 반 강제화? 
하면서 동시에 성능까지 개선한ListView 의 개량버전.

기존의 ListView 대부분의 겻을 custom해서 써야했고 또 기존의 ListView는 커스텀하기에는 구조적인 문제로 많은 제약이 있었다.
구조적인 문제로 인해 성능문제가 있었다. RecyclerView는 이런 문제를 해결하기위해 좀 
더 다양한 형태로 개발자가 커스텀할 수 있도록 유연하며 성능에 중점을 두어 만들어졌다고 한다.



### **RecyclerView 사용하기**

1. 제일먼저 배우면서 신기했던것.
 - RecyclerView의 가장큰 변경 사항은 LayoutManager과 ViewHolder, Item에 대한 뷰의 변형이나 애니메이션할 수 있는 개념이 추가 되었다. 
 - 이것들은 ( 앞의 LayoutManager, ViewHolder ,item ...) 리스트에 표시될 재활용 뷰를 관리하는데 사용되며 아이템의 포지션에 따른 레이아웃들의 위치를 결정한다고한다. 그리고 불필요한 뷰의 생성을 피하기위해 레이아웃을 관리는 역할을 하게 된다.


2. 주요 클래스

  
  - `Adapter` – 기존 ListView 쓸때 사용하던 Adapter를 생각하세여.
              데이터와 아이템에 대한 view 를 생성합니다.
  - `ViewHolder` – 재활용하는 View에 대한 서브뷰들 ?을 가지고 있는다.
  - `LayoutManager` – 아이템들을 배치한다.
  - `ItemDecoration` – 배치된 아이템 항목에서 서브뷰에 대한 처리하는 곳
  - `ItemAnimation` – 아이템들이 추가, 제거 될때 애니메이셔 처리를 하는것.


3. 내가 알것들.

    1. `ViewHolder`  : 
        `ViewHolder`는 기존의 `ListView`에서 많이 사용하고 구글에서도 추천하는 패턴이다.
        하지만 이를 강제적으로? 제한 하지는 않았지만 `RecyclerView`에서 Adapter와 ViewHolder를 반듯이
        같이 사용할 수 밖에 없는 구조로 만들어져있다!
        `ViewHolder`를 많이 사용해 보지 않았다면 연습이 필요!!!!
    2. `LayoutManager` :
         `LayoutManager` 는 RecyclerView 에서 가장 신기했던 부분
         RecyclerView를 만들때 반드시 생성해야하는 것.이를 통해 
         
    3. `Adapter` : 
        `ListView`에서 `adapter`와 동일한 형태의 구조로 해당 아이템의 데이터와 뷰간의 처리를 한다.
        






