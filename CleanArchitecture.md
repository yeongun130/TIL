### Clean Architecture란?
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcnQnoy%2Fbtq8HdLkG5Q%2FH05npeCZzqfmZqjYrVxAK0%2Fimg.jpg)

Clean Architecture는 총 4가지의 계층으로 이루어져 있다.

### 1. Entities
엔티티는 비즈니스 규칙을 캡슐화합니다. 엔티티는 메소드를 갖는 객체일 수도 있지만
데이터 구조와 함수의 집합일 수도 있다. 가장 일반적이고 고수준의 규칙을 캡슐화 하게 된다.
외부가 변경되더라도 이러한 규칙이 변경될 가능성이 적다.

### 2. Uses cases
유스케이스는 애플리케이션의 고유 규칙을 캡슐화 하며 엔티티로부터 데이터 흐름을 조합합니다.
유스케이스 계층의 변경이 엔티티에게 영향을 줘서는 안되며 데이터베이스, 공통 프레임워크 및
UI에 대한 변경으로부터 격리된다.

### 3. Interface Adapters (Presenters)
인터페이스 어댑터는 Entities 및 Uses cases의 편리한 형식에서 데이터베이스 및 웹에
적용할 수 있는 형식으로 변환한다. 이 계층에는 MVP 패턴의 Presenter, MVVM 패턴의
ViewModel이 포함된다. 즉, 순수한 비즈니스 로직만을 담당하는 역할을 한다.

### 4. Frameworks, Drivers(Web, DB)
프레임 워크와 드라이버는 상세한 정보들을 두게 된다. 웹 프레임 워크, 데이터베이스, UI,
HTTP client 등으로 구성된 가장 바깥쪽 계층이다.

---
클린 아키텍처가 동작하기 위해서는 의존성 규칙을 잘 지켜야 한다.
각각의 클래스는 한가지 역할만 수행하고, 서로 의존 관계를 어떻게 할지 규칙이 정해져 있고
이를 지켜야 한다.

의존성 규칙은 반드시 외부에서 내부로 저수준 정책에서 고수준 정책으로 향해야 한다.
예를 들면, 안드로이드 비즈니스 로직을 담당하는 ViewModel은 로컬 DB나 Web과 같은
세부적인 사항에 의존하지 않아야 한다. 이를 통해 비즈니스 로직은 세부사항의 변경에 영향
받지 않도록 할 수 있다.

이렇게 나누면 다음과 같은 이점을 얻을 수 있다.

- 새로운 기능을 빠르게 적용할 수 있다.
- 집중화된 클래스에 따른 프로젝트 유지 관리가 용이하다.
- 패키지 구조 탐색이 쉬워진다.
- 테스트 코드 작성에 용이하다.

### Clean Architecture in Android
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPJTRL%2Fbtq8IgnyP7P%2FUiDj9o89fYDn0kJKUkleHk%2Fimg.jpg)

클린 아키택처를 안드로이드에 접목 시키려면 일반적으로 Presentation, Domain, Data
총 3개의 계층으로 나눠지게 된다. Presentation -> Domain, Data -> Domain 순서로
의존성을 갖고 있다.

### 1. Presentation
화면과 입력에 대한 처리 등 UI와 관련된 부분을 담당한다. Activity, Fragment, View,
Presenter 및 ViewModel을 포함한다. Presentation 계층은 Domain 계층에 대한
의존성을 가지고 있다.

### 2. Domain
애플리케이션의 비즈니스 로직에서 필요한 UseCase와 Model을 포함하고 있다. UseCase는
각 개별 기능 또는 비즈니스 논리 단위이며, Presentation, Data 계층에 의존하지 않고
독립적으로 분리되어 있다. 안드로이드 의존성을 갖지 않고 java 및 kotlin 코드로만 구성
하며 다른 애플리케이션에도 사용할 수 있다. Repository 인터페이스도 포함되어 있다.

### 3. Data
Domain 계층의 의존성을 가지고 있다. Domain 계층의 Repository 구현체를 포함하고 있으며,
데이터베이스, 서버와의 통신도 Data 계층에서 이루어진다. mapper 클래스를 통해 Data 계층의
모델을 Domain 계층의 모델로 변환해주는 역할도 한다.

---
클린 아키텍처는 구조에서는 Realm에서 Room으로 데이터베이스를 변경할 때 수월하게 변경할 수
있다. Domain 계층에서 Repository 인터페이스를 작성하고 Data 계층에서 이를 구현한다.
데이터베이스는 Data 계층에서만 존재하기 때문에 Realm에서 Room으로 데이터베이스를 변경한다고
하면 Date 계층의 Repository 구현체만 Room으로 변경하면 된다.

Presentation 계층과 Domain 계층은 데이터베이스를 어떤 것을 사용하는지 전혀 알지 못한다.
때문에 Data 계층의 데이터베이스 관련 로직만 변경해주면 보다 쉽게 데이터베이스를 변경할 수 있다.


