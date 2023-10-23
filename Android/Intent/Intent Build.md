## 인텐트 빌드

`Intent` 객체에는 Android 시스템이 어느 구성 요소를 시작할지 판별하는데 사용하는 정보가 
담겨 있다.(예를 들어 정확한 구성 요소 이름 또는 인텐트를 수신해야 하는 구성 요소 카테고리 등).
또한 수신자 구성 요소가 작업을 적절히 수행하기 위해 사용할 정보(예 : 수행할 작업 및 조치를 취할
데이터 위치 등)도 이 안에 포함되어 있다.

`Intent`의 필드는 `ComponentName` 객체로, 이것을 지정하려면 대상 구성 요소의 완전히 
정규화 된 클래스 명(앱 패키지 포함)을 사용해야 합니다. 예를 들어 `com.example.ExampleActivity`
를 쓰세요. 구성 요소 이름을 설정 하려면 `setComponent()`, `setClass()`, `setClassName`을 
사용 하거나 `Intent` 생성자를 사용합니다.

### 작업

---

수행할 일반적인 작업을 나타내는 문자열 입니다.
`BroadCast Intent`의 경우 이미 실행 되어 보고 되는 작업입니다. 이 작업은 대체로 
나머지 Intent 구조를 결정합니다. 특히 `data`와 `extra`에 포함 되는 정보가 결정됩니다.

본인의 앱 내의 Intent가 사용할 작업(또는 다른 앱이 사용하여 본인 앱 안의 구성 요소를 호출하게 
할 작업)을 직접 지정할 수 있지만, 보통은 `Intent`클래스나 다른 프레임워크 클래스가 
정의한 작업 상수를 지정해야 한다.

- `ACTIVITY_VIEW`  
이 작업은 Activity가 사용자에게 표시할 수 있는 어떤 정보를 가지고 있을 때 `startActivity()`가 
있는 Intent에서 사용합니다.
- `ACTIVITY_SEND`  
공유 인텐트라고 하며, 사용자가 다른 앱을 통해 공유할 수 있는 데이터를 가지고 있을 때 
`startActivity()`가 있는 인텐트에서 사용해야 합니다. 

Intent에 대한 작업을 지정하려면 `setAction()` 또는 `Intent`생성자를 사용하면 된다.
나름의 작업을 직접 정의하는 경우, 앱의 패키지 이름을 접두어로 포함 시켜야 된다.
```kotlin
const val ACTION_TIMETRAVEL = "com.example.action.TIMETRAVEL"
```

### 데이터

---

작업을 수행할 데이터 및/또는 해당 데이터의 MIME 유형을 참조하는 URI(`Uri` 객체)입니다. 
제공된 데이터의 유형을 나타내는 것은 일반적으로 인텐트의 작업이다. 

데이터의 MIME 유형을 지정하면 Android 시스템이 인텐트를 수신하기 가장 좋은 구성 요소를 
찾는데 도움이 된다. 다만 때로는 MIME 유형을 URI를 통해 추론할 수 있다. 
특히 데이터가 `content: `URI 일 때 추론하기 쉽다. `content: `URI는 데이터가 기기에 
위치하고 `ContentProvider`가 제어한다는 것을 의미하며, 따라서 MIME 유형이 시스템에 
표시 된다.

- 데이터 URI만 설정하려면 `setData()`를 호출
- MIME 유형만 설정하려면 `setType()`을 호출
필요한 경우, `setDataAndType()`을 사용하여 두 가지를 모두 명시적으로 설정할 수 있다.

### 카테고리

---

인텐트를 처리해야 하는 구성 요소이 종류에 관한 추가 정보를 담은 문자열이다. 
인텐트 안에는 카테고리 설명이 얼마든지 있어도 되지만 대부분 인텐트에는 카테고리가 
없어도 된다.

- `CATEGORY_BROWSABLE`  
대상 액티비티가 웹 브라우저를 통해 시작되도록 허용하고 이미지, 이메일 등의 링크로 참조된 
데이터를 표시하게 한다.
- `CATEGORY_LAUNCHER`  
이 액티비티가 작업의 최초 액티비티이며, 시스템의 애플리케이션 시작 관리자에 목록으로 게재된다.

카테고리는 `addCategory()`로 지정한다.

### 엑스트라

---

요청된 작업을 수행하는데 필요한 정보가 담김 키-값 쌍이다.
몇몇 작업이 특정한 데이터 URI를 사용하는 것과 마찬가지로, 몇몇 작업은 특정한 
엑스트라도 사용한다.

- 댜양한 `putExtra()`메서드로 extra 데이터를 사용할 수 있다.
- 각 메서드는 키 이름과 값 두가지 매개변수를 가진다.
- 모든 extra 데이터를 가진 `Bundle` 생성 -> `Bundle`을 `putExtras()`로 삽입 가능  
ex) `ACTION_SEND`로 이메일을 전송할 인텐트를 생성하여 수신자를 지정 -> `EXTRA_EMAIL` 사용 
-> 제목을 `EXTRA_SUBJECT`로 지정

```kotlin
const val EXTRA_GIGAWATTS = "com.example.EXTRA_GIGAWATTS"
```



