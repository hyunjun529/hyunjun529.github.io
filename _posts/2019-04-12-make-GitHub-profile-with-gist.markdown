---
layout: post
title: 깃헙 프로필에 Gist를 쓸 수 있는데 거기에 GIF가 된다고?
tags: [개발환경구축, GIF, ffmpeg, Script, 삽질, GitHub, Gist]
---

# 깃헙 프로필에 Gist를 쓸 수 있는데 거기에 GIF가 된다고?

이 [링크](https://twitter.com/mathdroid/status/1115372109641814017)를 보자마자 한 눈에 용도를 찾았습니다.
![image](https://user-images.githubusercontent.com/7877313/55985243-6d6d0d00-5cda-11e9-9ad8-dd50b8077180.png)

## TL; DR

x = 695 px, y = 492 px

vertical padding = 77px, horizontal padding = 64px

ffmpeg script
```
// windows
ffmpeg -i v2.mp4 ^
-vf crop=325:100:0:0 g1.gif ^
-vf crop=325:100:370:0 g2.gif ^
-vf crop=325:100:0:196 g3.gif ^
-vf crop=325:100:370:196 g4.gif ^
-vf crop=325:100:0:392 g5.gif ^
-vf crop=325:100:370:392 g6.gif 

// linux, macos
ffmpeg -i v2.mp4 \
-vf crop=325:100:0:0 g1.gif \
-vf crop=325:100:370:0 g2.gif \
-vf crop=325:100:0:196 g3.gif \
-vf crop=325:100:370:196 g4.gif \
-vf crop=325:100:0:392 g5.gif \
-vf crop=325:100:370:392 g6.gif
```

## 준비물
1. 영상 혹은 적당한 gif
2. Git
3. ffmpeg

## 작업 시작

### Gist를 6개 만듭니다!
여기서 두번째 input이 프로파일에 노출될 제목입니다. 첫 번째는 Gist에 대한 설명이에요!
![image](https://user-images.githubusercontent.com/7877313/55988253-57f9e200-5cde-11e9-9d68-b87bd88ce5ba.png)

Gist는 Repo와 달리 나중에 이름 수정할 수 있으니까 일단 숫자로 만들어둬도 무관합니다.
![image](https://user-images.githubusercontent.com/7877313/55989782-174f9800-5ce1-11e9-8b6c-4b568c2e8968.png)


### 깃헙 프로필에 핀을 박아봅시다!
아래와 같이 만들어주세요! 우상단에 Customize your pins가 있습니다.
![image](https://user-images.githubusercontent.com/7877313/55989988-84fbc400-5ce1-11e9-9b7b-4e646d589398.png)


### 각 Gist에 gif를 넣기 위해 모두 로컬에 clone합니다!
Gist에 돌아가서 Embed를 보면 Gist를 클론 할 수 있는 링크가 있습니다. 이거 받아주세요!
![image](https://user-images.githubusercontent.com/7877313/55990090-c68c6f00-5ce1-11e9-98d2-a9636a6dcf08.png)

이런 느낌으로 만들면 편할 것 같습니다.
![image](https://user-images.githubusercontent.com/7877313/55990230-28e56f80-5ce2-11e9-97af-b13e323544d0.png)

```
git clone [url] [dir name]
```


### 한 번 크기를 재봅시다!
일단 아무 이미지나 넣어봅시다.
![image](https://user-images.githubusercontent.com/7877313/55990483-bde86880-5ce2-11e9-8969-d3d433ca7897.png)

Gist를 생성할 때 생긴 파일은 삭제해야 합니다. 그리고 올립시다.
```
git add .
git commit -m [msg]
git push
```

한 칸에 325x100 px로 이미지를 넣으면 됩니다
![image](https://user-images.githubusercontent.com/7877313/55990611-10298980-5ce3-11e9-8117-9714fd90a294.png)


### 영상이 들어갈 곳의 견적을 내봅시다!
325x100 = 750x300으로 볼 수 있겠습니다. 만약 액자식 구성까지 노린다면 다시 계산해봐야죠
위로 53px이 남고 아래로 16px이 남습니다. 그리고 양 옆으로 16px씩 더 남으니까...
![image](https://user-images.githubusercontent.com/7877313/55991050-4582a700-5ce4-11e9-9019-8e66d3150db0.png)

> 가로 695 px, 세로 492 px

> 가로선 77px, 세로선 64px


### 영상을 편집해봅시다
일단 영상을 크기에 맞춰서 Crop! (v1에 입력 소스, v2에 출력 소스, crop=가로크기:세로크기:가로위치:세로위치)
[참고 링크](https://video.stackexchange.com/questions/4563/how-can-i-crop-a-video-with-ffmpeg)
```
ffmpeg -i v1.mp4 -vf crop=695:492:170:10 v2.mp4
```

이제 ffmpeg으로 영상에서 액자에 맞게 6개 gif가 튀어나오도록 스크립트를 짭시다.
[참고 링크](https://trac.ffmpeg.org/wiki/Creating%20multiple%20outputs)
```
// windows
ffmpeg -i v2.mp4 ^
-vf crop=325:100:0:0 g1.gif ^
-vf crop=325:100:370:0 g2.gif ^
-vf crop=325:100:0:196 g3.gif ^
-vf crop=325:100:370:196 g4.gif ^
-vf crop=325:100:0:392 g5.gif ^
-vf crop=325:100:370:392 g6.gif 

// linux, macos
ffmpeg -i v2.mp4 \
-vf crop=325:100:0:0 g1.gif \
-vf crop=325:100:370:0 g2.gif \
-vf crop=325:100:0:196 g3.gif \
-vf crop=325:100:370:196 g4.gif \
-vf crop=325:100:0:392 g5.gif \
-vf crop=325:100:370:392 g6.gif
```

그럼 아래와 같이 칸에 맞는 gif가 생성됩니다.
![image](https://user-images.githubusercontent.com/7877313/55994626-318f7300-5ced-11e9-9571-ed9b9632ef01.png)

### 전부 올려줍시다!
![image](https://user-images.githubusercontent.com/7877313/55994971-3274d480-5cee-11e9-8f7d-b88fc0c3bc06.png)

## 결과
[GitHub Profile](https://github.com/hyunjun529)
