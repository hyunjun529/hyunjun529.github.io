---
layout: post
title:  "Unity ARKit XR Plugin으로 ARKit3 Face Tracking"
date:   2020-01-27 00:00:00 +0000
categories: unity arkit facetracking
comment: true
---

# Unity ARKit XR Plugin으로 ARKit3 Face Tracking(WIP)
2020년 1월 설날 연휴를 이용해서 조금씩 삽질한 내용에 대한 기록, 따로 iOS 개발자 계정을 등록하지 않아서 이런저런 데모 돌리다가 7일 10개 제한 걸린 덕분에 더 진행하긴 어렵고 그냥 진행한 내용까지 일단 저장.

> 나중에 다시 삽 뜨면 갱신 예정

## 목적
- 저가형 버추얼 아이돌
  - 모델링은 VRoid
  - 얼굴 인식은 ARKit, iOS
  - 그 모델링 파일들을 Windows 송출컴에 보내야됨(???)
- 최대한 직접 구현하기

## 지난 이야기
1. 2019년 중순 Python, OpenCV dlib으로 Face segmentation을 얻은 뒤, 이걸 2D에서 3D로 옮기면서 VRoid 캐릭터를 움직인 적이 있음.
2. 그 때 느낀 점은 정규화가 어렵고 카메라 하나로 굴리는 Face Segmetation 라이브러리의 성능이 좀 떨어지는데다 촬영 환경에 따른 파라미터 튜닝이 너무 번거로웠다.
3. ARKit3, Unity Face Tracking이 좋아보이더라

## 이번 삽질
- Unity
  - facial-ar-remote
  - Unity AR Foundation
    - arfoundation-samples
- iOS
  - [tracking_and_visualizing_faces](https://developer.apple.com/documentation/arkit/tracking_and_visualizing_faces)

### Unity

#### facial-ar-remote
- Deprecated됨, 빌드하려면 옛날 유물 끄집어내야함
- [어떤 분이 유튜브에 영상 올려주신게 있음](https://www.youtube.com/watch?v=VYvG8BwGOws)
- [iPhone-Windows 관련 issue](https://github.com/Unity-Technologies/facial-ar-remote/issues/28) 되긴 한다더라

##### Client-Server?
- Socket, Stream 이용
- BlendShape, 얼굴 관련 전송 파라미터 목록은 여기서 확인 : [BlendShapeUtils.cs](https://github.com/Unity-Technologies/facial-ar-remote/blob/master/Remote/Scripts/BlendShapeUtils.cs)
- Client 코드는 이 쪽 : [Client.cs](https://github.com/Unity-Technologies/facial-ar-remote/blob/master/Remote/Scripts/Client.cs)
- AR기기에서 딴 파라미터를 클라이언트로 통째로 전송하는건데, 일단 로컬 네트워크 9000포트로 돌아가게 되어있음
#### Unity AR Foundation
[ARKit XR Plugin](https://docs.unity3d.com/Packages/com.unity.xr.arkit@3.0/manual/index.html)과는 별도, 이건 인터페이스고 안드로이드나 iOS에 따라서 AR 기능은 각각 구현해야한다.

즉 ARKit을 쓰려면 저걸 통해야하는데, 멀티플랫폼에서 쓸 수 있게 뚫어둔 인터페이스 정도였음. 즉 [AR Face Manager](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@3.0/manual/face-manager.html)는 내부적으로 ARKit XR Plugin으로 통함

[Trackable Managers](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@3.0/manual/trackable-managers.html)가 super, 특이한 점으로 [RayCasting](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@3.0/manual/index.html#raycasting)을 쓸 수 있음.

##### arfoundation-samples
- [GitHub](https://github.com/Unity-Technologies/arfoundation-samples)
- [FaceBlendShape 돌려본 영상, Twitter](https://twitter.com/zaq1qaz/status/1221113902068486144?s=20)
- Scene에 그냥 덩그러니 ARFoundation 꽂아둔거랑 B 밖에 없어서 엄청 당황스럽다[..]

###### ARFace?
- [ARFace](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@3.0/api/UnityEngine.XR.ARFoundation.ARFace.html)
- BlendShape 세팅하는 방법이 딱히 안 적혀 있고 `.fbx` 나무늘보만 있어서 좀 곤란쓰;
- 문서도 제대로된게 설명된게 없고 코드도 보기 어렵게 되어있음, MeshVisualizer, ARFaceUpdatedEventArgs 등으로 어떤 파라미터가 나오는지 찍어봐야 알듯.

###### 차라리 ARCore
- [Augmented Faces developer guide for Unity](https://developers.google.com/ar/develop/unity/augmented-faces/developer-guide)
  - 이 쪽이 좀 더 설명이 잘 되어 있음
  - 단 여기는 캐릭터 전신이나 Blendshape 보단 AR스티커(네코미미 만들기 같은) 느낌


### iOS
- [tracking_and_visualizing_faces](https://developer.apple.com/documentation/arkit/tracking_and_visualizing_faces) 이거 돌릴려는 찰나에 딱 10개 제한이 걸려서 분석만 진행[..]
  - - [Tutorial](https://www.appcoda.com/arkit-face-tracking/)
- depth camera를 보려면 이 예제 [Streaming Depth Data from the TrueDepth Camera](https://developer.apple.com/documentation/avfoundation/cameras_and_media_capture/streaming_depth_data_from_the_truedepth_camera)

아마 유니티에 구현된 기능들을 좀 이해하려면 여길 먼저 뜯어봐야할거 같다. 사실 Unity를 쓸거면 상관없긴 한데, 대체 Unity가 어딜 기준으로 포인트 따는건지는 확인할 필요가 느껴짐


## 2018년에 ARKit2로 삽질했던 때와 지금을 비교하면
- 사실 그때나 지금이나 큰 틀에선 바뀐게 없는데, 버전이 올라가니까 세부적으로 별 기능이 더 추가되서 훨씬 편해진건 맞다.
  - 근데 이거 튜토리얼을 쓰자니 어디서부터 써야할지 감도 안 잡힐 정도로 자잘한 삽질이 너무 많이 들어갈거 같음
- 여전히 캐릭터 세팅이 가장 큰 문제, VRoid로 퉁칠 생각이긴한데 이거 호환성은 괜찮으려나..

## 다음 삽질
1. iOS 네이티브를 돌려본다. 일단 7일 제한 걸렸으니 존버.
2. 아무래도 Windows로 연결하려면 Unity를 쓰는게 좀 더 나을거 같음, VRoid, GLTF랑 Windows 호환성 생각하면 아무래도 iOS 네이티브는 여전히 좀 그래
3. 다음엔 iOS 네이티브에서 파라미터 뭐 나오는지 짜보고 Unity의 나무늘보를 VRoid 미소녀로 갈아치운다.
