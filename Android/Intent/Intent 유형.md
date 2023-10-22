## 인텐트 및 인텐트 필터

`Intent`는 메시징 객체로, 다른 앱 구성 요소로부터 작업을 요쳥하는데 사용할 수 있다.

### Intent 유형

1. 명시적 인텐트(Explicit Intent)
- 실행하고자 하는 컴포넌트의 클래스명을 인텐트에 담는 방법
- 호출된 대상을 명시하는 경우
- 주로 같은 앱의 컴포넌트를 실행할 때 사용   
-> ex) 앱의 화면 전환

```kotlin
val intent = Intent(this, SubActivity::class.java)
strartActivity(intent)
```
- 현재 Activity에서 다른 Activity를 호출할 때 사용

2. 암시적 인텐트(Implicit Intent)
- 클래스명이 아닌 Intent Filter 정보를 활용
- 호출된 대상을 명시하지 않은 경우
- 주로 클래스명을 알 수 없는 외부 앱의 컴포넌트를 실행할 때 사용

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Url.parse("https://www.naver.com"))
// ACTION_VIEW -> 뒤에 있는 것을 보여주라는 것
startActivity(intent)
```
- 작업을 할 수 있는 모든 대상에게 요청  
-> ex) 링크 클릭 시, 링크를 열 수 있는 앱이 많은 경우. 크롬, 웨일, 인스타 등에 요청

---

![screensh](https://developer.android.com/static/images/components/intent-filters_2x.png?hl=ko)
- 암시적 인텐트가 시스템을 통해 전달 되어 다른 액티비티를 시작한느 방법

1. Activity A가 작업 설명이 있는 `Intent`를 생성하여 이를 `startActivity()`에
전달합니다.
2. Android System이 해당 `Intent`와 일치하는 `Intent Filter`를 검색합니다.
3. 일치한느 항목을 찾으면, System이 해당 Activity의 `onCreate()` 메서드를 
호출하여 이를 `Intent`에 전달하고, 일치하는 Activity(Activity B)를 시작한다.

