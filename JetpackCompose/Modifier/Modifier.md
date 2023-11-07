### Modifier

- 컴포저블의 크기, 레이아웃, 동작 및 모양 변경
- 접근성 라벨과 같은 정보 추가
- 사용자 입력 처리
- 요소를 클릭, 드래그, 스크롤, 확대, 축소 같은 상호 작용 추가

```kotlin
@Composable
private fun Greeting(name: String) {
    Column(
        modifier = Modifier
            .padding(16.dp)
            .fillMaxWidth()
    ) {
        Text(text = "Hello, ")
        Text(text = name)
    }
}
```

- `padding` : 요소 주위에 공간을 배치
- `fillMaxWidth` : 최대 너비를 채움
- `fillMaxHeight` : 최대 높이를 채움
- `fillMaxSize` : 화면 전체를 채움

### 수정자의 순서

- 수정자의 순서에 따라 요청이 달라질 수 있다.
- example1
```kotlin
@Composable
fun ArtistCard(/* ... */) {
    val padding = 16.dp
    Column(
        modifier = Modifier
            .clickable(onClick = onClick)
            .padding(padding)
            .fillMaxWidth
    ) {
        /* ... */
    }
}
```

- example2
```kotlin
@Composable
fun ArtistCard(/* ... */) {
    val padding = 16.dp
    Column(
        modifier = Modifier
            .padding(padding)
            .clickable(onClick = onClick)
            .fillMaxWidth()
    ) {
        /* ... */
    }
}
```

- example1, 2는 서로 같은 내용이지만 padding의 순서만 다르다.
하지만 example2는 주변 패딩을 포함하지 않고 전체 영역을 선택할 수 있으며
example1은 주변 패딩을 포함하여 영역을 선택할 수 있다.


