# Glide

- MinSdkVersion = 14
- CompileSdkVersion = 26
- Library Size : 440Kb
- GIF 지원 O

Google에서 만든 이미지 로더 라이브러리인 Glide는 빠른 이미지 로딩, 버벅 거림과 끊김 현상이 발생하지 않는다는 점을 강조하고 있다. 
커스텀된 HttpUrlConnection을 사용하지만 Volley 또는 OkHttp 라이브러리에 연결할 수 있는 유틸리티 라이브러리도 포함되어있어, 
우리가 자주 사용하고 있는 라이브러리와 호환성이 좋다. Glide는 아래처럼 싱글톤으로 만들어 간단하게 사용할 수 있다.

```kotlin
Glide.with(this)
    .load(url)
    .apply(RequestOptions.bitmapTransform(RoundedCornersTransformation(128, 3)))
    .listener(object : RequestListener<Drawble> {
        override fun onLoadFailed(e: GlideException?, model: Any?, target: Target<Drawble>?, isFirstResource: Boolean): Boolean {
            TODO("Not yet implemented")
        }
        
        override fun onResourceReady(resource: Drawble?, model: Any?, target: Target<Drawble>?, dataSource: DataSource?, isFirstResource: Boolean): Boolean {
            toast("Complete")
            return false
        }
    })
    .into(imageView)
```
- 최초 로딩 속도 : 6.2s
- 기본 캐싱 적용 : 0.72s

![screensh](https://velog.velcdn.com/images%2Fjshme%2Fpost%2Fbc24b36a-8b2e-49c1-89ef-192062e2f417%2Fglide%20test.gif)

Glide는 이미지뷰의 크기를 측정한 다음 원본이미지를 가져와 이미지뷰 크기에 맞게 리사이징 후 비트맵에 그려주는 것을 기본 옵션으로 
하기 때문에 메모리 효율성이 좋다.

### Heap Dump
![screensh](https://velog.velcdn.com/images%2Fjshme%2Fpost%2F179bed8b-6c74-4b2f-b27c-3a4e232b91da%2Fimage.png)
11,004,024byte (=11MB)