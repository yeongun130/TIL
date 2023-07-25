# **Hilt**
Hilt는 Android 클래스에 의존성 주입을 지원하고 수명 주기를 자동으로 관리해주기 때문에 Android에서 
DI를 사용하기에 적합한 라이브러리이다.

## **Hilt 공식 문서 정리**

### **build.gradle(Project)에 추가한다.**
```kotlin
buildscript {
    dependencies {
        classpath = "com.google.dagger:hilt-android-gradle-plugin:$version"
    }
}
```

### **build.gradle(Module: app)에 추가한다.**
```kotlin
plugins {
  id = "kotlin-kapt"
  id = "dagger.hilt.android.plugin"
}

android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }    
}

dependencies {
    implementation = "com.google.dagger:hilt-android:2.38.1"
    kapt = "com.google.dagger:hilt-compiler:2.38.1"
}
```

### **Application 클래스 만들어 준다.**
```kotlin
@HiltAndroidApp
class ExampleApplication: Application() {  }
```

### **Android 클래스에 종속성 주입**
```kotlin
@AndroidEntryPoint
class ExampleActivity: AppCompatActivity() {  }
```

`Activity`외에도 Android 클래스를 지원한다.

- Application(@HiltAndroidApp)
- ViewModel(@HiltViewModel)
- Activity
- Fragment
- View
- Service
- BroadCastReceiver

### **의존성 주입 정의**
```kotlin
class AnalyticsAdapter @Inject constructor(
    private val service: AnalyticsService
) {  }
```
`@Inject`주석을 사용하여 인스턴스를 제공하는 방법을 Hilt에게 알려준다.

### **Hilt에서 제공하는 Component hierarchy**
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcl8gyc%2Fbtq1UNfDwtS%2FKUMlMdk66c486inLi9gQDK%2Ftfile.svg)

### **Hilt에서 제공하는 Component, Lifecycle, Scope**
| Hilt component            | Injector for                   | Created at             | Destroyed at            | Scope                   | 
|---------------------------|--------------------------------|------------------------|-------------------------|-------------------------|
| SingletonComponent        | Application                    | Application#onCreate() | Application#onDestroy() | @Singleton              |
| ActivityRetainedComponent | 해당없음                           | Activity#onCreate()    | Activity#onDestroy()    | @ActivityRetainedScoped |   
| ViewModelComponent        | ViewModel                      | ViewModel created      | ViewModel destroyed     | @ViewModelScoped        |   
| ActivityComponent         | Activity                       | Activity#onCreate()    | Activity#onDestroy()    | @ActivityScoped         |   
| FragmentComponent         | Fragment                       | Fragment#onAttach()    | Fragment#onDestroy()    | @FragmentScoped         |   
| ViewComponent             | View                           | View#super()           | View destroyed          | @ViewScoped             |   
| ViewWithFragmentComponent | @WithFragmentBindings가 붙은 View | View#super()           | View destroyed          | @ViewScoped             |   
| ServiceComponent          | Service                        | Service#onCreate()     | Service#onDestroy()     | @ServiceScoped          |  

- SingletonComponent : Application의 생명주기를 갖는다. Application이 생성 되는 시점에 같이 생성되고, 
파괴 되는 시점에 같이 파괴된다.
- ActivityRetainedComponent : Activity의 생명주기를 갖는다. 다만, Activity의 Configuration 
Change(디바이스 화면 전환 등..)시에는 파괴 되지 않고 유지된다.
- ViewModelComponent : Jetpack ViewModel의 생명주기를 갖는다.
- ActivityComponent : Activity의 생명주기를 갖는다. Activity가 생성 되는 시점에 같이 생성 되고, 
파괴 되는 시점에 같이 파괴된다.
- FragmentComponent : Fragment의 생명주기를 갖는다. Fragment가 Activity에 붙는 시점에 
생성 되고, 파괴 되는 시점에 같이 파괴된다.
- ViewComponent : View의 생명주기를 갖는다. View가 생성 되는 시점에 같이 생성 되고, 파괴 되는 시점에 
파괴 되는 시점에 같이 파괴된다.
- ViewWithFragmentComponent : Fragment의 View 생명주기를 갖는다. View가 생성 되는 시점에 
같이 생성 되고, 파괴는 시점에 같이 파괴된다.
- ServiceComponent : Service의  생명주기를 갖는다. Service가 생성 되는 시점에 같이 생성 되고, 파괴 되는 시점에
  파괴 되는 시점에 같이 파괴된다.


