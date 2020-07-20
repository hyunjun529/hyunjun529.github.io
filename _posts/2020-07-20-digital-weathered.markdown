---
layout: post
title:  "디지털 풍화 ~Twitter Fleet의 글씨는 언제쯤 흐릿해질까~"
date:   2020-01-02 21:00:00 +0000
categories: image_processing ffmpeg JPEG
comment: true
---

![플릿](/img/200720/thumb.jpg)

Twitte Fleet으로 글씨를 열화시키는게 유행처럼 돌고 있어서 대충 몇 바퀴를 돌아야 글씨가 사라질지 왜 때문인지 알아보는 글.

## 세줄요약
1. 디지털 풍화는 실존한다.
2. `JPEG`은 손실 압축 포맷이고 `Quality Parameter` 때문이다.
3. 타임머신을 타보자 :clock12: :clock3: :clock6: :clock9:


## 디지털 풍화
이미지를 어딘가에 올리고 공유할 때 마다 점점 이미지 화질이 나빠지는 현상을 겪은 적 있다. 특히 Twitter Fleet은 눈에 띄게 열화가 발생해서 사람들이 

### 생활 속의 디지털 풍화
![카카오톡](/img/200720/k.jpg)

[YouTube : 그저 카카오톡에 수동으로 풍화시켜보려는 영상](https://youtu.be/3DnYO39W5OM)


## JPEG
손실 압축이다. 인터넷의 80% 이상이 해당 포맷으로 유통된다. 더 나은 포맷도 많지만 선점효과 때문에 쓰인다.

![standards](/img/200720/standards.png)
[xkcd: standards](https://xkcd.com/927/)

![awesome](/img/200720/awesome.PNG)
놀랍게도 JPEG만 해도 10개가 넘는 유사 포맷이 있다.

### Quality Parameter
- 0~100 범위를 갖는다.
  - DCT 양자화에 사용되는 값이다.
- 90 이상은 거의 변화가 없으므로 고화질 또는 원본 화질이라고 표현한다.
  - 물론 이 때도 속칭 `풍화`가 발생한다.
- 80 이상을 고화질이라고 일반적으로 표현한다.
- 70 이상을 저화질이라고 하는 경향이 있다.
- 대략 95 이상, 60 이하는 크게 의미가 없다. 용량 측면에선 더 늘거나 줄지도 않는다.
- 해당 파라미터로 인한 품질 저하, 압축 성능은 영어 위키피디아를 참고 : [JPEG, effect of compression](https://en.wikipedia.org/wiki/JPEG#Effects_of_JPEG_compression)
- **같은 값으로 다시 인코딩해도, 인코딩하는 순간 DCT 양자화를 다시 하기 때문에 같은 파라미터로 인코딩을 해도 원본과 차이점이 발생한다.**

![12](/img/200720/k12.jpg)

## JPEG을 가지고 놀아보자

### 실습) ImageMagick
- 썸네일과 같이 diff를 만들 수 있다.
- compare를 하면 아래와 같이 나온다.

![12](/img/200720/k13.jpg)

- Quality Paramter를 뽑아볼 수 있다.

![ang2](/img/200720/ang2.jpg)
![ang](/img/200720/ang.jpg)
![ang3](/img/200720/ang3.jpg)

### 스크립트
#### [magick compare](https://imagemagick.org/script/compare.php)
```bash
magick compare k1.jpg k3.jpg k13.jpg
```
#### [magick identify](https://imagemagick.org/script/identify.php)
```bash
magick identify -verbose k2.jpg
```

#### [magick convert](https://imagemagick.org/script/convert.php)
```python
import os

for i in range(1, 100):
    os.system("magick convert go85/{}.jpg -quality 55 go85/{}.jpg".format(i, i+1))
```

### Quality Paramter만 돌려서 확인해본 결과
- 이게 아닌거 같다, 단순히 Quality Parameter만 수정할 경우 아래와 같이 별로 깨지지 않는다.

![101.jpg](/img/200720/101.jpg)


### 그렇다면 혹시 해상도 차이에서 리사이즈가 발생하면서 생긴 일 일까?
- 단순히 파라미터만 바꿔서는 저런 느낌으로 풍화가 발생하지 않음
- 층층이 쌓일 때 해상도가 바뀌는데, 혹시 이 영향일까?

#### 설계
- 내가 받은 원본은 970x2048 크기의 스크린샷
- 정말 정확하게 하려면 한국내 스마트폰 비율을 따지고 각 기기별 해상도를 불러와서 스크린샷처럼 만들어야 하지만
  - 구글은 이런걸 다 저장해뒀다 : [https://material.io/resources/devices/](https://material.io/resources/devices/)
- 귀찮다. 그냥 random신이 모든걸 해결해주실꺼야!

#### 스크립트
```bash
import os
import random

start_width = 970
start_height = 2048

for i in range(1, 100):
    target_w = start_width * random.uniform(0.2, 2.4)
    target_h = start_height * random.uniform(0.2, 2.4)
    os.system("magick convert go85/{}.jpg -quality 90 -resize {}x{} go85/{}.jpg".format(i, target_w, target_h, i+1))
```

![boom](/img/200720/boom.PNG)



### 결과
- 다른 디지털 풍화보다 더 플릿이 빨리 늙는 이유는 사람들이 스크린샷을 찍을 때 마다 해상도가 꾸겨지고 리사이즈되면서 발생한 것이다.
- Qualit Parameter의 영향은 생각보다 적다.
  - 오히려 퀄리티가 높아서 자연스럽게 블러가 되며 뭉개진 것에 가깝다.
- 플릿 글씨가 흐려진 원인으로 중간에 누군가 일부러 블러로 뽀샤시 쳤을 가능성이 있을 수도 있다, 자연스러운 풍화라고 하기엔 너무 블러가 자연스럽다는 의심이 들기 시작했다.

![101](/img/200720/101-re.jpg)

![102](/img/200720/102-re.jpg)



## QnA
### Q. 복원할 수 있나요?
A. 손실 압축입니다. 다시 압축되면서 이미 손실된 픽셀 정보는 복원이 불가능합니다. [waifu](https://waifulabs.com/) 같은 SR(Super Resolution) 기술로 복원해도 아직은 부자연스러운 부분이 남아있습니다.

### Q. 어디서 어떻게 올린건지 추적할 수 있나요?
A. 사이트나 어플리케이션마다 인코딩 설정이 보통 정해져 있기 때문에 어디에 올렸었는지 알 순 있지만, 손실 정보만 가지고 추적하기엔 정보가 부족합니다.

### Q. 다른 이미지 포맷은 어떤가요?
- PNG
  - 기본적으로 PNG는 비손실 압축 방식을 사용합니다.
  - 하지만 [pngquant](https://pngquant.org/)와 같은 팔레트를 사용한 Color Quantization이 적용되면 손실이 발생합니다.
    - [libpng spec](http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf) 4.1.2 , 9.5 참조
    - 흔히 PNG의 용량을 줄이기 위해서 압축한다고 하면 이런 로직이 수행되며, 설정에 따라 원본과 달라질 수 있습니다.
- GIF
  - 거의 무조건 색이 바랩니다. 한 팔레트에 256색만 존재할 수 있습니다.
  - [gifski](https://github.com/ImageOptim/gifski)와 같은 인코더는 한 팔레트에 256색을 두고, 다음 프레임에 새로 256색을 추가하는 방식으로 이 한계를 조금 넓힐 수는 있습니다.
    - 그래서 가끔 보면 GIF 품질이 좋은 경우가 있는데, 이런 원리입니다. [이 경우 각 프레임은 대충 이런 느낌으로 생겼습니다.](https://github.com/ImageOptim/gifski/issues/30)
- WebP
  - [WebP는 Lossless and Lossy](https://developers.google.com/speed/webp) 포맷입니다.
  - [WebP에서 Lossy](https://developers.google.com/speed/webp/docs/compression)의 경우 : DCT 양자화 과정에서 JPEG과 마찬가지로 손실됩니다.
