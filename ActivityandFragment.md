Activity

onCreate()

무조건 Activity가 처음 실행될 때 호출된다. 시스템에 의해 호출되면 인자로 받은 Bundle은 null이다.

Activity가 실행된 적이 있는데, 어떤 이유로 종료된 후 재시작되면, 종료될 때 호출된 onSaveInstanceState()에서 저장한 내용과 동일한 Bundle을 넘겨준다. 디바이스가 회전되어 가로/세로 전환 등 리소스를 새롭게 갱신되어야 할 때 호출된다.

onDestroy()

Actiivity가 종료되기 전 호출된다. Activity내부에서 finish()를 실행하면 호출된다.

시스템 메모리가 부족하면, 안드로이드가 강제로 TestApp을 죽일 때도 호출이 되는데, 메모리 확보가 매우 시급할 때는 호출조차 되지 않는 경우도 있다.

onCreate()와 짝을 이뤄 사용했던 리소스는 이 곳에서 싹~ 치워준다.

onStart()

Activity가 초기 실행 후, 화면의 전면으로 나타날 때 onCreate(), onRestart() 이후에 호출된다.

전화수신, SMS수신 등으로 Background로 갔다가 다시 전면으로 나올때도 호출된다.

onRestart()

Activity가 정지되었다가 다시 실행될 때 호출되는데, onDestroy()가 호출된 이후가 아닌, onStop()으로 정지된 상태에만 해당된다. 즉, onStop()과 onRestart()는 한 쌍으로 생각하면 된다.

onStop()

하드웨어 HOME버튼을 눌렀을 때와 SMS수신, 전화수신, 다른 App실행할 때 호출된다.

onResume()

Activity가 전면에 나타날 때 대부분의 상황에서 호출된다. 처음 실행했을 때, onCreate() 이후에도 호출된다. (책에서는 팝업 대화상자가 떳다가 닫히는 경우에도 호출된다고 하지만, AlertDialog로 테스트 했을 때에는 호출되지 않았다.)

onPause()

거의 모든 경우에 onStop(), onDestroy()가 호출되기 이전에 호출된다.

Activity가 사용자의 시선에서 가려지는 경우에 호출된다고 생각하면 된다.

대부분의 상황에서 onStop() 발생하기 이전에 불린다.

*일반적으로 onResume()과 쌍으로 보고, onResume()에서 했던 작업을 onPause()에서 정리, 멈추는 것이 좋다.

예를 들면, onResume()에서 쓰레드를 실행시켰으면, onPause()가 호출될 때, 아직 쓰레드가 실행중이면 정리를 해주면 된다.

*onPause()가 호출되서 App(또는 Activity)이 일시정지된 상태라면 안드로이드 시스템에서 필요에 따라 완전이 죽일 수 있기 때문에 그 이후의 작업을 못할 수도 있다는 점을 유의해야한다.

onSaveInstanceState()

Activity가 전면에서 Background로 숨는 경우 호출된다.

현재의 Activity 상태를 저장하려면 이 함수를 구현한다.

호출될 때, Bundle 인스턴스를 넘겨주는데 이를 이용해서 저장하면 되는데, 예를 들어 에디트박스에 입력된 문자열 등을 저장해 두면 된다.

onRestoreInstanceState()

onSaveInstanceState() 함수에서 저장했던 내용은 onCreate()에서 Bundle 인스턴스로 넘겨받는데, onRestoreInstanceState()에서도 같은 내용을 받을 수 있다.

주의할 점은 onRestoreInstanceState()는 정상적인 상황에서는 호출되지 않는다.

테스트 결과 일반적인 상황이 아닌, 디바이스의 화면회전이 발생할 때, 강제종료 후 제시작 할 때 발생했다.

onPostCreate() 와 onPostResume()

이 두 함수는 시스템 상에서 마지막 초기화 작업을 목적으로 만들어진 것으로 일반적으로 어플리케이션 작성시에는 구현할 필요가 없다고 한다.

onLowMemory()

시스템 메모리가 부족할 때 호출된다고 하나, 테스트로 발생시키기 어려워 생략했다.

안드로이드 Dev 사이트를 참고하여 설명하면, 이 함수가 정확히 호출되는 시점은 명확하지 않고, 다만, Background에서 실행하는 프로세스가 죽임을 했을 때, 호출된다고 한다. 시스템은 현재 Foreground에 있는 App에게 메모리를 좀 확보해 주십사~하고 호출해 주는 것이다. 즉, 이 함수에서는 필요없는 리소스를 최대한 확보하는 코드를 넣어주면 시스템에서 매우 고마워한다는 것이다.

이 함수가 return되는 순간 시스템은 GC를 수행한다고 한다.



---

Fragment

- 최초 생성 Lifecycle

 1. onAttach()

Fragment가 Activity에 붙을때 호출 된다.

 2. onCreate()

Activity에서의 onCreate()와 비슷하나, ui관련 작업은 할 수 없다.

 3. onCreateView()

Layout을 inflater을하여 View작업을 하는곳이다.

 4. onActivityCreated()

Activity에서 Fragment를 모두 생성하고 난다음 호출 된다. Activity의 onCreate()에서 setContentView()한 다음이라고 생각 하면 쉽게 이해 될것 같다. 여기서 부터는 ui변경작업이 가능하다.

 5. onStart()

Fragment가 화면에 표시될때 호출된다. 사용자의 Action과 상호 작용 할 수 없다.

 6. onResume()

Fragment가 화면에 완전히 그렸으며, 사용자의 Action과 상호 작용이 가능하다.

- 다른 Fragment가 add

 1. onPause()

Fragment가 사용자의 Action과 상호 작용을 중지한다.

 2. onStop()

Fragment가 화면에서 더이상 보여지지 않게 되며, Fragment기능이 중지 되었을때 호출 된다.

 3. onDestoryView()

View 리소스를 해제 할수 있도록 호출된다.

backstack을 사용 했다면 Fragment를 다시 돌아 갈때 onCreateView()가 호출 된다.

 - replace or backward로 removed되는 경우

 4. onDestory()

Fragment상태를 완전히 종료 할 수 있도록 호출 한다.

 5. onDetach()

Fragment가 Activity와 연결이 완전히 끊기기 직전에 호출 된다.

- 그외 Callbacks Method

onSaveInstanceState()

Activity와 동일하게 Fragment가 사라질떄 호출되며 상태를 Bundle로 저장할수 있도록 호출 된다.