## 1. MVC
MVC 패턴은 Model + View + Controller를 합친 용어이다.
### 1) 구조
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7IE8f%2FbtqBRvw9sFF%2FAGLRdsOLuvNZ9okmGOlkx1%2Fimg.png)

- Model : 애플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View : 사용자에게 보여지는 UI 부분
- Controller : 사용자의 입력(Action)을 받고 처리하는 부분

### 2) 동작
1. 사용자의 Action은 Controller에 들어온다.
2. Controller는 사용자의 Action을 확인하고 Model을 업데이트한다.
3. Controller는 Model을 나타내줄 View를 선택한다.
4. View는 Model을 이용하여 화면을 나타낸다.

MVC에서 View가 업데이트 되는 방법
- View가 Model을 이용하여 직접 업데이트 하는 방법
- Model에서 View에게 Notify 하여 업데이트 하는 방법
- View가 Polling으로 주기적으로 Model의 변경을 감지하여 업데이트 하는 방법

### 3) 특징
Controller는 여러 개의 View를 선택할 수 있는 1:n 구조이다.
Controller는 View를 선택할 뿐 직접 업데이트 하지는 않는다. (View는 Controller를 알지 못한다.)

### 4) 장점
MVC 패턴은 널리 사용되고 있는 패턴이라는 점에 걸맞게 가장 단순하다. 단순하다 보니 보편적으로 
많이 사용되는 디자인 패턴이다.

### 5) 단점
MVC 패턴은 View와 Model 사이의 의존성이 높다. View와 Model의 높은 의존성은 애플리케이션이 
커질 수록 복잡해지고 유지보수가 어려워진다.

## 2. MVP
MVP 패턴은 Model + View + Presenter를 합친 용어이다. MVC와는 Model, View는 동일하고 
Controller 대신 Presenter이 존재한다.

### 1) 구조
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclZlsT%2FbtqBTLzeUCL%2FIDA8Ga6Yarndgr88g9Nkhk%2Fimg.png)

- Model : 애플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View : 사용자에게 보여지는 UI 부분
- Presenter : View에서 요청한 정보로 Model을 가공하여 View에 전달해주는 부분

### 2) 동작
1. 사용자의 Action들은 View를 통해 들어온다.
2. View는 데이터를 Presenter에게 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에서 요청받은 데이터를 응답한다.
5. Presenter는 View에게 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

### 3) 특징
Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 한다.
Presenter와 View는 1:1 관계이다.

### 4) 장점
MVP 패턴은 View와 Model의 의존성이 없다. MVP 패턴은 MVC 패턴의 단점이 였던 View와 
Model의 의존성을 해결하였다.

### 5) 단점
View와 Model 사이의 의존성은 해결 되었지만, View와 Presenter 사이의 의존성이 높은 
단점이 생겼다. 애플리케이션이 복잡해 질 수 록 View와 Presenter 사이의 의존성이 강해지는 
단점이 있다.

## 3. MVVM
MVVM 패턴은 Model + View + View Model을 합친 용어이다.

### 1) 구조
![screensh](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCiXz0%2FbtqBQ1iMiVT%2FstaXr7UO95opKgXEU01EY0%2Fimg.png)
- Model : 애플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View : 사용자에게 보여지는 UI 부분
- View Model : View를 표현하기 위해 만든 View를 위한 Model이다. View를 나타내기 위한 
Model이자 View를 나타내기 위한 데이터를 처리하는 부

### 2) 동작 
1. 사용자의 Action들은 View를 통해 들어온다.
2. View에 Action이 들어오면 Commend 패턴으로 View Model에 Action을 전달한다.
3. View Model은 Model에게 데이터를 요청한다.
4. Model은 View Model에게 요청 받은 데이터를 응답한다.
5. View Model은 응답 받은 데이터를 가공하여 저장한다.
6. View는 View Model과 Data Binding하여 화면을 나타낸다.

### 3) 특징
MVVM 패턴은 Commend 패턴과 Data Binding 두 가지 패턴을 사용하여 구현되었다.
Commend 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성을 없앴다.
View Model과 View는 1:n 관계이다.

### 4) 장점
MVVM 패턴은 View와 Model 사이의 의존성이 없다. 또한 Commend 패턴과 Data Binding을 
사용하여 View와 View Model 사이의 의존성도 없앴다. 각 부분은 독립적이기 때문에 모듈화 하여 
개발할 수 있다는 장점이 있다.

### 5) 단점
View Model 설계가 어려운 단점이 있다.