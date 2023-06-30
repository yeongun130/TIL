# Fragment LifeCycle
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDyVCU%2Fbtq9CtTEtoA%2FkpOuUqYRAw8aVmbyKT7jpk%2Fimg.png)

액티비티의 생명주기에 따른 콜백 함수와 비교 해봤을 때 생성 시 onCreateView() - 
onViewStateRestored()가 추가로 있고, 종료 시 onSaveInstanceState() - 
onDestroyView()가 추가로 있다.

액티비티도 마찬가지이지만 기본적으로 LifeCycle은 위에서 아래 방향으로 진행된다. 
Fragment가 백스택에 최상단으로 올라왔을 경우 생명주기가 CREATED - STARTED - 
RESUMED 순으로 진행되고, 반대로 백스택에서 pop으로 시작했을 경우 RESUMED - 
STARTED - CREATED - DESTROYED 순으로 진행된다.

Fragment의 LifeCycle이 변화되는 순간 Fragment CallBack 함수를 호출하게 되고, 
해당 콜백 함수가 종료되는 시점에 View의 LifeCycle에 이벤트를 전달하게 된다.

## onCreate()
Fragment만 CREATED 된 상황이다.
FragmentManager가 add 됐을 때 도달하며 onCreate() 콜백 함수를 호출한다. 
주의할 점은 onCreate() 이전에 onAttach()가 먼저 호출된다는 것이다.
이 시점에는 아직 Fragment View가 생성되지 않았기 때문에 Fragment의 View와 
관련된 작업을 두기에는 적절하지 않다.
onCreate() 콜백 시점에는 Bundle 타입으로 savedInstanceState 파라미터가 함께 
제공되는데, 이는 onSaveInstanceState() 콜백 함수에 의해 저장된 Bundle 값이다. 
savedInstanceState 파라미터는 Fragment가 처음 생성 됐을 때만 null 값으로 넘어오며, 
onSaveInstanceState() 함수를 재정의 하지 않았더라도 그 이후 재생성부터는 non-null 
값으로 넘어오게 된다.

## onCreateView(), onViewCreated()
onCreate() 이후에는 onCreateView()와 onViewCreated() 콜백 함수가 이어서 호출된다. 
onCreateView()의 값으로 정상적인 FragmentView 객체를 제공했을 때만 FragmentView의 
LifeCycle이 생성된다.
onCreateView()를 재정의 하여 FragmentView를 직접 생성하고 Inflate 할 수 있지만 
LayoutId를 받는 Fragment의 생성자를 사용하여 해당 리소스 아이디 값을 통해 onCreateView() 
재정의 없이도 FragmentView를 생성할 수 있다. onCreateView()를 통해 반한된 View 객체는 
onViewCreated()의 파라미터로 전달 되는데, 이 시점부터는 FragmentView의 LifeCycle이 
INITIALIZED 상태로 업데이트 되기 때문에 View의 초기값을 설정해주거나 LiveData 옵저빙, 
RecyclerView 또는 ViewPager2에 사용될 Adapter 세팅 등은 onViewCreated()에서 
해주는 것이 적절하다.

## onViewStateRestored()
onViewStateRestored() 함수는 저장해둔 모든 state 값이 Fragment의 View 계층구조에 
복원 됐을 때 호출된다. 따라서 이 시점부터 각 뷰의 상태값을 체크할 수 있다.
View lifecycler owner는 이 때 INITIALIZED 상태에서 CREATED 상태로 변경됐음을 알린다.

## onStart()
Fragment가 사용자에게 보여질 수 있을 때 호출된다. 이는 주로 Fragment가 attach 되어있는 
Activity의 onStart()와 유사하다. 이 시점부터는 Fragment의 child FragmentManager를 
통해 FragmentTransaction을 안전하게 수행할 수 있다. 

## onResume()
Fragment가 보이는 상태에서 모든 Animator와 Transition 효과가 종료되고, Fragment가 
사용자와 상호작용할 수 있을 때 onResume() 콜백이 호출된다. 
Resumed 상태가 됐다는 것은 사용자가 Fragment와 상호작용 하기에 적절한 상태가 됐다고 했는데, 
이는 반대로 onResume()이 호출되지 않은 시점에서는 입력을 시도하거나 포커스를 설정하는 등의 작업을 
임의로 하면 안된다는 것을 의미한다.

## onPause()
사용자가 Fragment를 떠나기 시작했지만 Fragment는 여전히 visible 일 때 onPause()가 호출된다. 
Fragment와 View의 LifeCycle이 PAUSED가 아니라 STARED가 된다. 

## onStop()
Fragment가 더이상 화면에 보여지지 않게 되면 Fragment와 View의 LifeCycle은 CREATED 상태가 되고 
onStop() 콜백 함수가 호출된다. 이 상태는 부모 액티비티나 프래그먼트가 중단 됐을 뿐만 아니라 
부모 액티비티나 프래그먼트의 상태가 저장 되었을 때도 호출된다. 
API 28 버전을 기점으로 onSaveInstanceState() 함수와 onStop() 함수 호출 순서가 달라졌다. 
onStop()이 onSaveInstanceState() 함수보다 먼저 호출됨으로써 onStop()이 
FragmentTransaction을 안전하게 수행할 수 있는 마지막 시점이 됐다.
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbC4Zkm%2Fbtq9DwbxrgQ%2FIl287fhextuJbiCRZtZde1%2Fimg.png)

## onDestroyView()
모든 exit animation과 transition이 완료되고, Fragment가 화면으로부터 벗어났을 경우 
FragmentView의 LifeCycle은 DESTROYED가 되고 onDestroy()를 호출한다.
이 시점부터는 getViewLifecycleOwnerLiveData()의 리턴값으로 null이 반환된다. 
그리고 해당 시점에는 가비지 컬렉터에 의해 수거될 수 있도록 Fragment View에 대한 
모든 참조가 제거되어야 한다.

## onDestroy()
Fragment가 제거되거나 FragmentManager가 destroy 됐을 경우, Fragment의 LifeCycle은 
DESTROYED 상태가 되고 onDestroy() 콜백 함수가 호출된다. 해당 지점은 Fragment LifeCycle의 
끝을 알린다. 그리고 onAttach()가 onCreate() 전에 호출 됐던 것처럼 onDetach() 또한 
onDestroy() 이후에 호출된다.

