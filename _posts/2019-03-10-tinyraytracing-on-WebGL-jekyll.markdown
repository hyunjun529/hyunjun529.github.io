---
layout: post
title: tinyraytracing을 WebGL로 만들어보자
categories: [3D삽질]
tags: [JavaScript, RayTracing, 삽질]
---

# tinyraytracing을 WebGL로 만들어보자
![image](https://user-images.githubusercontent.com/7877313/54085775-f5cb5b80-4384-11e9-93a1-c8f151d40f23.png)

[tinyraytracer](https://github.com/ssloy/tinyraytracer)를 WebGl, JavaScript로 조금 구현해본 기록입니다

## TL;DR
- **원본 C++ 코드는 256줄이지만 반쯤 따라했는데 JS가 1000+줄 나옵니다**
- [Live Demo](https://hyunjun529.github.io/tinyraytracer-WebGL/)
- [Repo](https://github.com/hyunjun529/tinyraytracer-WebGL)

## 일단 웹페이지에 띄워보자
![image](https://user-images.githubusercontent.com/7877313/54085042-52764880-437c-11e9-858a-0d580ec06c28.png)

[commit](https://github.com/hyunjun529/tinyraytracer-WebGL/commit/525ab7ede78036cb80706d38e80608d90fe4e15f)

1. 원래 예제에서는 RGB값을 ofstream에 직접 넣지만, WebGL에서 이를 따라하기 위해서는 glTexImage2D로 매 프레임을 생성하는 방법이 있습니다.
2. canvas에 WebGL Context를 생성하고, 1장만 그리도록 설정한 뒤, 간단한 쉐이더까지 구현합니다.
3. Polyfill과 자잘한 구현을 줄이기 위해서 util을 추가합니다.


## 수학 라이브러리를 짜자
[commit](https://github.com/hyunjun529/tinyraytracer-WebGL/commit/beb308447f1d5a45736572a63d0050ecf1ba4843)

1. 원본의 [geometry.h](https://github.com/ssloy/tinyraytracer/blob/master/geometry.h)는 템플릿과 연산자 오버로딩으로 되어 있습니다.
2. JS에서 비슷한 역할을 하는 라이브러리가 필요해 [three.js src/math](https://github.com/mrdoob/three.js/tree/master/src/math)를 참고했습니다.
3. 하지만 가장 큰 차이점, **연산자 오버로딩이 JS에선 안 됩니다.** 그래서 행렬 연산 관련된 코드가 길어질 수 밖에 없습니다.


## 하나하나 따라해보자
[commit](https://github.com/hyunjun529/tinyraytracer-WebGL/commits/master)

![image](https://user-images.githubusercontent.com/7877313/54085770-e815d600-4384-11e9-90de-d15774a4eb5d.png)

![image](https://user-images.githubusercontent.com/7877313/54085772-ea783000-4384-11e9-8c2a-08731ed54702.png)

1. 그래픽스의 미덕은 수식을 그대로 구현하면 결과가 그대로 나온다는 점이죠.
2. 그냥 하나하나 따라하면 즐거운 실습시간입니다.


## 안 잡히는 버그
[commit](https://github.com/hyunjun529/tinyraytracer-WebGL/commit/78fdd618e3c90e002b9b24064f8e7d5b461a3a50)

1. JS는 C/C++과 달리 연산자 오버로딩도 없고 포인터도 없죠, 그래서 계속 변수를 새로 만들 clone()이 필요합니다.
2. 이를 피하기 위해서 스코프를 따로 파고 let으로 만드는 방법도 있습니다.
3. 그러니까 이 버그가 뭐냐면 값이 사라져요 어딘가에서[..] 근데 clone()으로 떡칠하나 let으로 떡칠하나 디버깅도 수식 구현도 점점 힘들어집니다.

즉 Step.5에서 고칠 수 없는 버그를 발견하고 따라하는 실습을 중단 했습니다

## 애니메이션을 넣자
[commit](https://github.com/hyunjun529/tinyraytracer-WebGL/commit/ce72c55e7f4da066f5ee5b80885364089f6b00d3)

![image](https://user-images.githubusercontent.com/7877313/54085602-332ee980-4383-11e9-85a1-48c4c88f3726.png)

1. [requestAnimationFrame](https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame)으로 WebGL을 매 프레임마다 갱신하거나 애니메이션을 구현할 수 있습니다.
2. 여기다가 지금까지 구현된 조명과 구체를 간단히 움직이는 코드를 넣어봅니다. 뒤에 있는 specular 진짜 어디갔을까요 ㄱ=...


## 남은 것
1. JavaScript로 행렬 연산 하지말자, 연산자 오버로딩이 안 된다.
2. [TypeScript](https://www.typescriptlang.org/) 쓰자, 물론 연산자 오버로딩이 되는건 아니다.
3. 행렬 연산은 WebGL Shader로, 2D는 [transform, matrix()](https://developer.mozilla.org/ko/docs/Web/CSS/transform-function/matrix)로 짜자.
4. commit log를 잘 남기면 그 자체로도 강의자료가 될 수 있다.
