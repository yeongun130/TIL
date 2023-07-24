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

## **Component hierarchy**

![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcl8gyc%2Fbtq1UNfDwtS%2FKUMlMdk66c486inLi9gQDK%2Ftfile.svg)
- Hilt에서 제공하는 Component hierarchy


