Composable 함수는 선언적이기 때문에 UI에 변화를 줄 수 있는 유일한 방법은 동일한
Composable 함수를 다른 파라미터와 함께 호출하는 것 뿐이다. 이 때 주입되는 파라미터들이 
UI 상태(state)를 나타낼 것이다. 이러한 state가 변화할 때마다 Composable은 
recomposition(재구성)을 한다.

다른 관점에서 보자면, state가 변화 하지 않으면 UI 상태도 변화 하지 않는 것이다.
XML에서 EditText에 해당하는 TextField의 예를 들어보자면

```kotlin
@Composable
fun TextFieldExample() {
    OutlinedTextField(
        label = { Text ("label") },
        value = "",
        onValueChange = {}
    )
}
```
TextField는 어떠한 값을 입력받아서 UI 상태가 변화 하지 않는다. TextField는 value가 변화 해야
UI가 업데이트를 하게 되는데, value 값이 없기 때문이다. 즉 TextField는 value라는 파라미터에서 
state 변수를 가지고 있어야 하며, onValueChange는 이러한 value가 변화 했을 때 행위를 
람다로 지정해 주는 것이다.

### composable에서 state 관리

composable 함수 내부에서는 remember composable 함수를 이용하여 단일 객체를 
메모리에 저장할 수 있다. remember에 의해 생성된 값은 초기 상태(initial composition)과, 
재구성(recomposition)시에 저장 된다. 이렇게 저장된 값은 remember를 호출한 
composable이 composition에서 지워질 때 사라진다.

remember는 mutable, immutable 객체 모두를 저장 할 수 있다.
보통은 `mutableStateOf<>()`, `immutableStateOf<>()` 형식의 값을 저장한다.

remember를 이용하여 state value를 선언할 때는 아래의 세 가지 방식이 모두 사용 될 수 있는데, 
세 방식은 모두 같은 기능을 하며, 코드의 구성상 가장 가독성이 높은 방식을 채택하면 된다.
```kotlin
val mutableState = rememeber { mutableStateOf(default) }
var value by remember { mutableStateOf(default) }
val (value, setValue) = remember { mutableStateOf(default) }
```

remember에 저장된 값은 UI 파라미터로 사용되는 것 이외에, logic으로도 구성할 수 있다.

```kotlin
@Composable
fun TextFieldExample() {
    Column(modifier = Modifier.padding(16.dp)) {
        var name by remember { mutableStateOf("") }
        if (name.isNotEmpty()) {
            Text(
                text = "Hello, $name",
                modifier = Modifier.padding(bottom = 8.dp),
                style = MaterialTheme.typography.h2
            )
        }
        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("Name") }
        )
    }
}
```
