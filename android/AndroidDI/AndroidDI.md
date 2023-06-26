## Dependency 란?
Dependency는 "의존성"이라는 뜻이다. 일반적으로 둘 중 하나가 다른 하나를 어떠한 용도를 위해 
사용하는 경우를 Dependency라 하는데, 여기서는 객체 지향의 중요한 요소 중 하나인 "Object 
Dependency" 즉 객체끼리의 의존성을 의미한다고 볼 수 있다.

## DI의 개념
DI란 Dependency Injection의 약자로 의존성 주입을 의미한다. 의존성 주입은 하나의 객체가 
다른 객체의 의존성을 제공하는 기술이다. 비유하자면 '의존성'은 서비스로 사용할 수 있는 객체이고 
'주입'은 의존성(서비스)을 사용하려는 객체로 전달하는것을 의미한다. DI는 프로그래밍에서 널리 사용되는 
기법으로, DI의 원칙을 따르면 훌륭한 앱 아키택처를 위한 토대를 마련할 수 있다.

## DI 에시
### 1) 클래스가 필요한 종속 항목을 구성한다.
~~~ kotlin
class Car {
    private val engine = Engine()
    
    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    var car = Car()
    car.start()
}
~~~

### 클래스들의 관계도
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbf7QiU%2FbtrdC5hrD8I%2Fk2mTuKSW437iCHB43q7En1%2Fimg.png)

이러한 방법으로 클래스가 필요한 객체를 얻는 경우 Car와 Engine이 밀접하게 연관되어 있기 때문에 
문제가 생길 수 있다. 우선 코드의 재사용이 여러워진다. Car 인스턴스는 자신이 생성한 한가지 
유형의 Engine만을 사용하기 때문에 Engine 유형이 Gas와 Electric 두 가지라면 하나의 Car 
인스턴스를 재사용하는 대신 두 가지 유형의 Car를 생성해야 한다. 또한 이처럼 종속 항목이 강하게 
의존하고 있는 경우 다양한 테스트를 하기 어려워진다. 

### 2) 다른 곳에서 객체를 가져온다.
Context getter와 getSystemService()와 같은 일부 안드로이드 API가 이러한 방식으로 작동한다.

### 3) 객체를 매개변수로 전달받는다.
앱은 클래스가 구성될 때 종속 항목을 제공하거나 각 종속 항목이 필요한 함수에 전달할 수 있다. 
Car 인스턴스는 초기화 시 Engine 객체를 구성하는 대신 Engine 객체를 생성자의 매개변수로 받는다.

~~~ kotlin
class Car(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val engine = Engine()
    var car = Car(engine)
    car.start()
}
~~~

### 클래스들의 관계도
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbhdIpH%2FbtrdEjTVXyf%2FQkT3uTaLwo5pcPY8ZCeLz0%2Fimg.png)

이러한 방법은 Car의 재사용이 가능해지며 다양한 시나리오를 테스트 할 수 있다.

## DI의 장점
- 코드의 재사용 가능 : 종속 항목의 객체를 쉽게 교체할 수 있다.
- 리펙토링 편의성 : 종속 항목은 구현 세부정보로 숨겨지지 않고 노출되어 있어 검증 가능한 
요소가 되므로 객체 생성 또는 컴파일 시 확인 할 수 있다.
- 테스트 편의성 : 클래스가 종속 항목을 관리하지 않으므로 다양한 방법을 테스트 할 수 있다.

## 안드로이드의 DI 라이브러리
- Dagger : 구글에서 유지 관리하며 자바, 코틀린 및 안드로이드용으로 널리 사용되는 라이브러리이다.
- Hilt : Dagger 기반으로 빌드된 Jetpack의 권장 라이브러리이다.

Dagger는 컴파일 타임 정확성, 런타임 성능, 확장성 및 안드로이드 스튜디오 지원 등의 장점이 있다.

Hilt는 프로젝트의 모든 안드로이드 클래스에 컨테이너를 제공하고 생명주기를 자동으로 관리해 앱에서 
DI를 실행하는 표준 방법을 정의한다.
