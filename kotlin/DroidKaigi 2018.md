# DroidKaigi 2018

## DAY1

### - kotlin basic 

`lateinit`  

```kotlin
lateinit var profile: Profile
var profile: Profile? = null //차이?

fun init(){
  profile.name
}


val r = str?.apply{
  
}
```

`with/apply/also`

```kotlin
  
}
val button = Button(context).apply{
  this.text = "hello"
  setText("hello")
}
val button = Button(context).also{
  it.text = "hello"
}

return team?.user?.name? = name

```

`data class`

```kotlin
data class User (val name: Stirng, val age: Int)

class UserModel(): DefaultImpl by Steat() {
  override fun abstract(): String{
 
  }
}
```

```kotlin
fun String.isInteger() = this.all{}
```

- delegate Property

```kotlin
val user: User 
	get() = User(userId)
```

```kotlin
//custom getter
val isAdmin: String
		get() = userId == ADMIN_ID
```

`Custom setter`

```kotlin
val text: String = ""
set(value){
  owner.name = value
}

set(value){
  field = value
  xxx = value.name
}

```

### Deep Dive Into LayoutManaget for RecyclerView

- onLayoutchildren
  - initial layout
  - adapter changes
- layout all relevant 
- only visible view should be on memory



LayoutState *

Why fill toward both direction?

onLayoutChildren

1. Find the anchor position - Anchor Posotion 
   1. remenber the first visibe view to resotre the state
2. Calculate how many items should be in visible portion
3. Fill toward end
4. Fill toward start



ScrollVericallyBy

- canScrollVertically: Boolean
- ScrollVericallyBy(int dy, RecyclerView.Recycler recycler, RecyclerView.State state)

1. Small dy
2. Large dy 
   1. Recycle views no longer visible
   2. Fill appearing Veiws



- RecyclerVeiw

  - Recycler
    - RecycledViewPool(can be shared across multiple view) / LayoutManage called RecyclerVeiw(View)
  - Scrap
    - detachAndScrapAttachedVeiws
    - Scrap heap (share view pool)

- layoutmanager -> recycler ( recycler.getViewForPosition) -> adapter ( adapter.bindViewHolder)

- case 1.Found in scrap

  - layoutmanager -> recycler -> adapter ( adapter.bindViewHolder)

- Found In Recycler pool

- Not found in cache

- Use scrap if you re- attach in a same layout pass

- use recycler for views no longer needed

- scrollHorizontallyBy

- Predictive Animations on recyclerVeiw

  - SupportPredictiveOnAnimation

- In pre- layout

  - visible area / remaining/ appearing / disappearing

  ​

### How to Kontribute ( v2)

setup - Ant enviernment

ant-g updateㅋ

YouTrack 사용

Intention

Use PSI viewer!

 - identify PsI ( program structure interface  )

KtReturnExpression

Create Intention file

abstact class SelfTargetingIntention

Add intentionAction Tag

Konclusion



### How to love developers like your customer

1. debug features
2. Leak canary lib
3. Suspend



### say bye to Fragment

- Router - stack manage
- 액티비티 생명주기에 따라서 controller 관리
- KTX ?????zz



### MVVM

- Domain Presentation Separation



###Faster Builds with android plugin for gradle 3.0.0

> Basic Optimizations

- min SDK > 21

- no Dynamic version numbers

- Filter resources

- ```kotlin
  default { 
    resConfigs "en","xxhdpi"
  }
  ```

- Disable Multi -APKs

  - ```splits.abi.enable = false```

- Configuration - on - Demand

  -  ``` org.gradle.configureondemand =true```

- Parallel Execution

  - ```org.gradle.parallel= true``` 

- Disable PNG Crunching

- ```kotlin
  apptOption{
    cruncherEnable false
  }
  ```

- WebP

  - ~25% reduced file size
  - PNGs -> WebP
  - Supported on API 18+

> Use Plugin 3.0.0 +

- Add the google() repo

  - ```ko
    repositories{
      google()
      // All you need to download the plugin
    }
    ```



> Avoid Unnecessary Work

- Compilation Avoidance  - new Config

  RuntimeOnly -

  compileOnly -

  api

  Implementation - we dont have to recompile

- FreeBiee

- Improved parallelism



> Variant - aware

- Variant - aware - AGP automatically matches variants of dependencies to that of the consumer.

- ```kotlin
  dependencies{
    Implenentation ~
  }

  android{
    publicsh ~
  }
  ```

- all product flavor must now belong to a named flavors dimension

- missing build type

- matchingFallbacks

  - ``` 
    matchingFallbacks = [debug , release]
    ```

> Per-class Dexing

dexing is now performed at the class level

-  Cacheable tasks - org.gradle.caching = true
- More and more AGP tasks are taking advantage 



##DAY2

### Kotlin Backstage

- Primitive
- string Interpolation
- Nulls
- Extension Function 
- Inline Function 
- .asSeqence()
- Lambdas - 

```kotlin
private val lambda = {
  //lambda
}
```

- AnObject

```ko
val AnObject : INSTANCE
```

- Companion Object 



### android Things Google Assistance

Action on google - create google project 

- sync response - json object 로 정보가 온다
- Action on google device data <-> web OAuth site - only json
- Device Action
  - Sync
  - Execute (request ,"intent" : "action.device.EXECUTE"
  - Query ( response )
- How to create a web service ? - GDP Function
- cloud Function



### How Android Rendering Works to Provide Pixels on the Screen Faster than You Can Read this Sentence

>  How android renders th UI

- Rendering

- Choreographer  Vsync -> UI Thread (input, animation, measure, layout , draw ,  ) ->  render Thread (sync , GPU , execute , ) -> Graphics ( get better , swap  buffer, composite)

- Invalidation = the process of causing things to be redrawn.

- ViewRootImpl

- Traversal - Performing all phase 

- DisplayList - an encapsulation of the rendering commands for a view

- (sync )UI thread -> RenderThread + Damage Area + Upload non-HW bitmaps

- Render Thread - thread responsible for issuing display list for 

- cllpRect

- Optimizations ( GPU ) - layers,( HW / SW )  alpha, Reorderint / Batching , elevation

- Reordered - 하면 하나씩 생기던 이미지나 각각의ui들이 한번에 배경 그다음에 프로필 이미지 등 차례대로 로딩되고 렌더 되어 한번에 쫘르륵 순서대로 나타난다. 시간에따라 되는것부터 그리고 그다음 뷰를 그린다,

- clipReject

- Get buffer ->  glCommand( )

- Composite -> SurfaceFlinger 

- a super complicated Example

- OffsetTopAndBottom( ) {invalidateViewProperty () } -> RenderThread

- ```kotlin
   ViewRoolmpl {
  PerformTraversals () {
  performDraw }}

  ```

- Request Layout() - the process fos causing measure/ layout to be 

- ```kotlin
  requestLayout() {
    performTraversals()
  }
  ```

- Measure-  Asking Views their preferred size / multiple measure layout

- layout - telling views their size and position

- Composition. / Suface -> SurdaeFlinger - > Display

- BufferQueue -> buffer 0 … biffer N / Producer side and Consumer Side 로 나뉜다. Producer side ->. quereBuffer () / consumer side -> acquireBuffer() 

- window manager /. Window  / surface ( producer )

- surfaceFlinger/  layer /  Buffer Queue (consumer)

- SurfaceView **

- OpenGL / surfaeceTexture()

- RenderThread is the consumer

  - kind of like a fancy ImageView

- Query the SurfaceTexture

  - To crage the producer endpoint

- vulkan media codex 

- openGl

  - First draw : dequeueBuffer()
  - eglSwapBuffers() : queueBuffer ()

- Composition - HardWareComposer

  - Optimization to avoid using th GPU
    - Overlays
    - Save power
    - Save bandwidth

- composition protocol

  - Overlay limitation

- one time overlay -> FrameBuffer



###// Things CodeLabs



### ConstraintLayout

- Flexible layout system

- Encourage falt hierarchies

- Tool support

- Compatible API 9 

- Positioning

  - positioning with widget

- Centering

- Dimension

  - Fixed
  - Warp_content
  - Match Constraints (0dp)
    - takes available space

- Tooling

  - why should use it  zz
  - Component Tree
  - Inspector
  - Group Operations
  - Inference

- Performance

  - ViewGroup / mesure layer draw
  - Relative Layout 
    - meaures children twice

- 1.0

  - Gone Behavior
  - Inadapted margin
  - Guidelines
  - Ratio
  - chains
  - Different chain Styles
    - spread
    - spread inside
    - weighted

- 1.1.0

  - Barriers
  - Groups : apply the specific visibility
  - Placeholder - TransitionManager
  - Circular Constraints
  - 0dp = match_constraint
  - Baselin is constrained to another baseline
  - Horizontal constraint + no bias = Centered

- ConstraintSet

  - contains all your constraints
  - Programming API
  - clone from live layout or XML
  - Initalizing a constraintSet
    - layout resoure
    - live layout

- Applying a constraintSet 

  - ( .asConstraintSet) /  mConstraintSet .applyTo(mConstraintLyout)

- Build two layouts

- More Real- life Example

  - Converting existing layout
    - linear layout
      - Flowed behavior
      - weighted behavior
    - PercentRelative Layout
  - useing a Guideline
    - create a guideline
    - 가이드라인에 제약사항 걸어놓으면 match0dp줘도 가이드라인 안넘어감
    - constraint  = recyclerView + FlexedRelativeManager
    - ConstraintDimensionRaito
    - Toggle alternately by every click

- Tricky Layout

  - 긴 textview가 있으면 처리할 수 있는 constraint layout 
    - horizontal chain between two view( textview  + button)
    - Chain is a packed chain
    - Horizontal bias is set to 0
    - Two view s app:layout_constrainedWidth="true"
  - components are fine!
    - using nested constraintLayout can be a powerful layout tool
  - Don't Convert?
  - chains :don't overdo it

- Custom Components

  - 캘린더 위젯이나 , 너희 회사에서 만드는 custom component

  - custom view that extends constraintLayout

  - Define two constraintSet

    - toggle teh constraints set component

  - ```kotli
    class custonView: Constraint() {
     // 확장 사용가능  
    }
    ```

- What's Next

  - constraintLayput 2.0
  - virtual viewGroup
    - chain, layers, Table , Flex
    - Decorators
    - Additional tooling
    - Performances
  - ​

### Ready For AI? 

> ml on mobile
>
> machine learning & tensorFlow
>
> Build your own model and optimize it

- deep learning architecture 
- 구글 플라이트 - 항공사가 얼마나 늦나아닌가 확인하는거?
- AI detects a risk of my future kkkkk
- 왜 텐서플로? 
  -  andorid. - TensorFlow , library z
  - train - ir - inference
  - 텐서플로는 구글에 설명이 나와있어요
  - tensorFlow only for machineLearning
  - tensorFlow designed to run models in production
  - tensorFlow mobile life = java client -> c API -> core ( C ++)
  - tensorFlow mobile = tensorFlow java. + android 
  - tensorFlow 추가하기 겁나쉬움 ( 안드로이드 )
  - 디펜던시만 추가하면 된다. 그리고 계산 필요
  - Variable
  - Practically
  - Training
    - to enable the model to run on mobile
    - For better performance
      - Graph transformation
    - Freeze : variable to constant
      - train
      - Checkpoint
      - 성능을 좋게하려고 트레이닝을 시킨다?
  - Inference
  - Pre-trained models for Image classification
  - model comparison
  - mobilenet vs patchnet
  - ​

