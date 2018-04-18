# *Next Step Architecture*

---

###*Agile조직이<br> 앱의 구조를 바꾸는 과정은 <br>어떻게 진행될까?*

---
1. 왜 MVP를 중요하게 생각할까?
2. 어떻게 조직에 적용할까?
3. 실제 코드에서 어떻게 적용할까?

---
##우리는 왜 mvp를 <br> 중요하게 생각할까?
---
### 생각하시는 <br> 그 MVP는 아닙니다..ㅎ

![small](file:/Users/jeonghyeonji/til/nba-mvp.jpg) 

---
#MVP:<br> 
##Minimum Viable Product
---
 최소로 <br>실행가능한 제품
---
---
![](file:/Users/jeonghyeonji/til/mvp.png)

---
최소의 기준 및 실행가능 기준 
조직, 서비스, 제품별 MVP의 해석이 다르다.
---
---
![](file:/Users/jeonghyeonji/til/lean.png)

---
`(발표자노트)`

##(발표자노트)현재의 상태에서 <br>사용자가 최소한의 불편함 없이 <br>사용할 수 있는지를 확인하는 것
---
#####왜 중요한가.
###리소스는 한정적이지만,<br> 사용자들의 니즈는<br> 빠르게 변한다.
---
`(발표자노트)`

##최소한의 리소스로 <br>고객의 피드백을 얻고, <br>쉽게 제품 개발에 반영할 수 있도록  <br>설계된 서비스를 출시하고 싶다.
---
mvp를 어떻게 만들까? 
---
---
1. 2주안에  기능을 확인할 수 있는 상황을 공유.
2. 앞으로의 더 변경될 사항에 대해 이야기한다.
3. 변경될것과 변하지않을 것을 구분한다.
4. 기존 구성요소에는 수정이 일어나는것을 지양.
5. 확장 / 재사용이 가능한 포인트를 찾는다.

---
##어떻게 조직에 적용할까?
---

![center](file:/Users/jeonghyeonji/til/lin.jpeg)

---
###과제 : <br>로그인 화면의 통일성 <br>== 통합 로그인 
---
Minimum Viable : 
---
---
![fit-right](file:/Users/jeonghyeonji/til/cash.png)
###사용자가 ID/PW를 입력하고<br> 입력한 내용이 일치할 경우,<br> 로그인이 성공되어 <br>다음 프로세스로 진행되는 것
---

Action Item :
---
---
1. email / display name 적용된 통합로그인
2.  소셜 로그인 기능
3. 다른 for캐시슬라이드 서비스에 적용 ( 모듈화 )

---
`(발표자노트)`
기술 설계 과정 토론… 
사용자가 사용할 수 있도록 <br>*최소반응을 이끌어낼 수 있는 기능단위로 MVP를 잡고,*
이후 개발 사항의 변경에 용이한 구조로<br> 변경 방향을 잡도록 한다.

---
###실제 코드에서<br>어떻게 실행할까?
---
여기서 등장하는<br> MVP
---
---
![f](file:/Users/jeonghyeonji/til/mvp_pattern.png)
여기서는<br>그 MVP가 맞습니다.
---
---
현재 : <br>MVC 
---
---
캐슬 - 현재 mvc 보여주기
---
---
###새로 들어가는 과제를 통해 <br>new Architecture 끼워 넣기
---
1. AAC?!
2. MVVM같은 MVVM같은 MVP

---
AAC :<br> Android Architecture Component
---
---
- LifeCycle
- LiveData
- ViewModel
- Room

---
현재 바로 적용 가능한 것 :
---
---
New Language : Kotlin
---
---
왜 Kotlin을 써야하는가?
---
---
`#린스타트업`
#MVP에 따른  기능개발 Task
---
1. 화면 설계 - layout 코드 작성
2. 화면에 따른 기능 구현
3. 서버와 데이터 연결
4. 서버 협업 테스트 , response확인
5. 테스트
 ....
 
---
new architecture 도입?
---
---
기존 서비스의 코드를<br> 수정하기 힘들다.
---
---
(말)수 많은 History..

---
##추후 적용될<br> 모듈화의 방향으로 <br>New Tech를 적용하자!
---
![](file:/Users/jeonghyeonji/til/bestor.jpeg)

---
android Studio<br>패키지 캡쳐? (예시)
---
---
Realize :<br>실현
---
---
##구조를 바꾸기 위한 과정은<br> 어떻게 진행될까?
---

1. 기존에 있던 것을 다시 적용하는 경우 
2. 새로운것을 시도하여 적용하는 경우
3. 모듈을 진행하면서 더 다르게 적용하고 싶었던 경우 

---
`#기존에 있던 것을 다시 적용하는 경우 ` 

-  기존에 사용했었던 오픈소스?
-  업데이트 / 안정화 된 라이브러리들을 다시 적용해본다.

---
![fit-center](file:/Users/jeonghyeonji/til/open.png)
##OPEN SOURCE
---
![right](file:/Users/jeonghyeonji/til/hqdefault.jpg)

말도많고<br>탈도많은<br>우리사이<br>그래도 돌아서면<br>  생각나.
---
---
`#새로운 것을 시도하여 적용하는 경우` 

-  Kotlin
-  MVP pattern
-  retrofit

---
`#모듈화를 진행하면서 더 다르게 적용하고 싶었던 경우 `
<br>

-  서비스들의 공통적인 Style적용

---
Critical pass
---
---
###실제 실행할때 어떻게 하고있고 ,<br> 하는것이 맞다고 생각하는지 얘기한다.
---
예 )  room을 solid pattern으로<br>어떻게 적용할 수 있을까 ..( need develop.. )
---
---
Talking about<br>Real Architecture :
---
---
1. 캐시슬라이드의 구조
2. 실제 캐시슬라이드에 NEW를 도입할 때 어려웠던 점 

----
`#캐시슬라이드의 구조`

---
`#새로운 것은 언제가 어렵다`

- 다른 팀원들의 러닝커브
- to 자바 개발시간 < to 코틀린 개발시간 

---
##도입 과정
---

1 .  최신 기술 반영을 위한 공감을 이끌어 내기위한<br> 최신 기술의 장단점과 앱의 적용시 고려되어야 할<br>점들을 토론.

---

2 .  개발자로서의 성장을 위한 최신 기술 study <br>-> 구조에 반영

---

3 .  제품 타겟층에 따른 최신 기술 반영

---
예시?) 현재 들어가 통합로그인 과제에 따른 구조 개선
현재 로그인 서비스는 서버의 스택? 에 맞추어 짜여져있다 - 빠른 적용과 반응을 보기위해 서버에 맞추어 구조를 짜고 진행한다.
새로운 서비스 - kotlin 도입 , MVP패턴 도입
새로운 기술 도입을 위한 팀원들의 설득과정 , 협업을 위한 구조 설계 과정

---