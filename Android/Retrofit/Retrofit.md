# Retrofit
> - HTTP 통신을 간편하게 처리할 수 있는 라이브러리

## Retrofit 구성요소

---
### Data class

- Kotlin에서 데이터를 모델링하기 위해 사용되는 특별한 클래스
- 간단하게 데이터를 저장하고 접근할 수 있다.
- API 요청 및 응답 데이터를 나타내는 클래스로 데이터 클래스를 사용할 수 있다.
- 데이터 클래스는 주로 속성(프로퍼티)를 가지고 있으며, 필요한 경우 equals(), hashcode(), 
toString() 등의 메서드를 자동으로 생성해준다.

### HTTP 작업을 정의하는 Interface

- Retrofit을 통해 서버와 통신하기 위한 메서드를 선언하는 역할
- 인터페이스의 각 메서드는 HTTP 메서드 어노테이션(@GET, @POST, @PUT 등)과 함께 엔드포인트 경로, 
매개변수, 응답 형식 등을 정의한다.

### Retrofit.Builder를 선언한 Object

- Retrofit 객체를 생성하기 위해 사용되는 빌더 클래스
- Retrofit.Builder를 사용하여 Retrofit 객체를 구성하고 필요한 설정을 지정한다.
- Retrofit.Builder()로 생성되며, baseUrl(), addConverterFactory(), addCallAdapterFactory() 
등의 메서드를 사용하여 필요한 구성을 실행할 수 있다. 

## Retrofit 사전 준비

---
### builder.gradle(Model:app)에서 의존성 추가
```
dependencies() {
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0' // JSON 파싱을 위한 Gson 컨버터
}
```
- 최신버전은 [*Retrofit Github*](https://github.com/square/retrofit)에서 확인 가능

### Manifest.xml 파일에 인터넷 권한 추가
```xml
<uses-permission android:name = "android.permission.INTERNET"/>
```

## Retrofit 사용하기

---
### 1. 데이터 클래스 정의
```kotlin
data class User(
    val id: Int,
    val name: String,
    val email: String
)
```
- 사용자 정보를 담는 데이터 클래스를 정의한다.
- 서버에서 받은 JSON 응답을 파싱하여 객체로 표현하는데 사용 

### 2. Retrofit을 사용하여 HTTP 작업을 정의하는 인터페이스 선언
```kotlin
interface ApiService {
    @GET("users")
    suspend fun getUsers(): List<User>
}
```
- `ApiService`인터페이스는 Retrofit을 통해 수행할 HTTP 작업을 정의하는 역할
- `suspend`키워드를 사용하여 코루틴을 지원하도록 설정했다.

### 3. Retrofit.Builder를 사용하여 Retrofit 인스턴스를 생성하는 객체 선언
```kotlin
object Retrofit {
    private const val BASE_URL = "https://api.example.com/"
    
    private val retrofit: Retrofit by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .buuild()
    }
    
    val apiService: ApiService by lazy {
        retrofit.create(ApiService::class.java)
    }
}
```
- `RetrofitClient`는 싱글톤 객체로, Retrofit 인스턴스를 생성하고 관리하는 역할
- `BASE_URL`은 API의 기본 URL을 나타낸다.
- `Retrofit.Builder`를 사용하여 Retrofit 인스턴스를 생성하고, 필요한 구성을 설정한다.
- `addConverterFactory(GsonConverterFactory.create())`를 통해 Gson 컨버터를 
추가하여 JSON 데이터를 파싱할 수 있도록 설정한다.
- `apiService`는 Retrofit을 통해 생성된 인스턴스를 반환하는 프로퍼티이다.

### 4. ViewModel을 사용하여 Retrofit과 통합
```kotlin
class MyViewModel: ViewModel() {
    private val apiService = RetrofitClient.apiService
    
    private val _users = MutableLiveData<List<User>>()
    val users: LiveData<List<User>> get() = _users
    
    private val _error = MutableLiveData<String>()
    val error: LiveData<String> get() = _error
    
    fun fetchUsers() {
        viewModelScope.launch {
            try {
                val userList = apiService.getUsers()
                _users.value = userList
            } catch (e: Exception) {
                _error.value = "Failed to fetch users: ${e.message}"
            }
        }
    }
}
```
- `MyViewModel`클래스는 ViewModel을 확장하며, Retrofit을 사용하여 데이터를 가져온다.
- `RetrofitClient.apiService`를 통해 Retrofit 인스턴스에 엑세스하고, 
`fetchUsers()`를 호출하여 사용자 데이터를 가져온다.
- `viewModelScope.launch`블록 내에서 `apiService.getUsers()`를 호출하여 사용자 목록을 
가져온 후, `_users`MutableLiveData를 업데이트 한다.
- API 요청이 성공하면 `_users`를 업데이트 하고 실패하면 `_error`에 에러 메시지를 설정한다.

### 5. Activity 또는 Fragment에서 ViewModel을 사용하여 데이터 가져오기
```kotlin
class MyActivity: AppCompatActivity() {
    private lateinit var viewModel: MyViewModel
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        viewModel = ViewModelProvider(this).get(MyViewModel::class.java)
        
        viewModel.users.observe(this, Observe { userList ->
            // 사용자 목록이 업데이트 될 때마다 호출된다.
            // userList를 사용하여 UI를 업데이트 하거나 처리한다.
        }) 
        
        viewModel.error.observe(this, Observe { errorMessage ->
            // 사용자에게 에러 메시지를 표시하거나 에러 처리 로직을 수행한다.
            Toast.makeText(this, errorMessage, Toast.LENGTH_SHORT).show()
        })
        
        viewModel.fetchUsers()
    }
}
```
- `MyActivity`클래스는 AppCompatActivity를 상속하며, ViewModelProvider를 사용하여 
ViewModel을 생성하고 가져온다.
- `ViewModelProvider(this).get(MyViewModel::class.java)`를 호출하여 ViewModel 
인스턴스를 얻을 수 있다.
- `viewModel.users.observe`를 사용하여 `users`LiveData를 관찰하고, 데이터가 업데이트 되면 
이를 수신하여 UI를 업데이트하거 처리한다.
- `viewModel.error.observe`를 사용하여 `error`LiveData를 관찰하고, 에러 메시지가 업데이트 되면 
이를 수신하여 사용자에게 에러 메시지를 표시하거나 에러 처리를 수행한다.
- `viewModel.fetchUsers()`를 호출하여 사용자 데이터를 가져온다.

