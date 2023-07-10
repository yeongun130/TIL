# Picasso

- MinSdkVersion = 14
- CompileSdkVersion = 29
- Library Size = 121Kb
- GIF 지원 X

Square에서 만든 Picasso는 최소한의 메모리로 이미지의 다양한 Transformation을 지원하며, 자동으로 메모리 & 디스크 캐싱, 
어댑터에서 ImageView를 재활용 및 다운로드 취소가 가능하다는 점을 강조하고 있다.

```kotlin
Picasso.get().load(url)
    .transform(RoundedCornersTransFormation(128, 3))
    .into(imageView5, object : Callback {
        override fun ouSuccess() {
            toast("Complete")
        }
        
        override fun onError(e: Exception?) {
            TODO("Not yet implemented")
        }
    })
```
- 최초 로딩 속도 : 6.4s
- 기본 캐싱 적용 : 1.6s

![screensh](https://velog.velcdn.com/images%2Fjshme%2Fpost%2F3dd57603-26d9-4738-8b87-75c60aa6c6a6%2Fpicasso%20test.gif)

Picasso는 원본 이미지 크기를 그대로 비트맵에 그린 후에 이미지뷰에 적용한다. 1000 * 800의 이미지가 존재할 때, Bitmap에 
1000 * 800 * 4bytes = 3MB가 ImageView 위에 올라갈 것이다. 그렇기 때문에 고화질의 이미지를 로드한다면 OOM을 발생시킬 수 있다.

이 문제를 방지하기 위해 fit() 함수를 이용한다면 고화질 이미지를 로드하기 전 이미지뷰의 크기를 먼저 측정하기 때문에 메모리 사용량을 
최소화 할 수 있을 것이다.

```kotlin
Picasso.get()
    .fit()
    .transform({ })
    .load(url)
```
### Heap Dump
![screensh](https://velog.velcdn.com/images%2Fjshme%2Fpost%2Fabb805e7-0a7f-4fcf-a85e-270163e7adea%2Fimage.png)
10,136,858byte (=10MB)
