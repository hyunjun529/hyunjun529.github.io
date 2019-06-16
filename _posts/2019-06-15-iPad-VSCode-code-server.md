---
layout: post
title: iPad에서도 VS Code로 코딩하고 싶어!
tags: [개발환경구축, VS Code, iPad]
---

# iPad에서도 VS Code로 코딩하고 싶어! 
iPad Air 3세대를 이번에 사서 정말 즐겁게 쓰고 있습니다. 특히 스플릿뷰 기능이 너무 좋더라고요. 저는 키보드와 에플 펜슬을 같이 쓰고 있습니다. 그런데 키보드가 생기니까 이걸로 ~~둠~~VS Code를 할 수 있으면 더 좋겠다는 욕심이 들어서 여러가지 삽질한 기록입니다.

구체적인 내용이나 자세한 방법은 설명하자니 너무 길어져서 생략했습니다.

![2](/assets/resource/19-06-15/2.png)
![3](/assets/resource/19-06-15/m1.gif)

## 3줄 요약
1. iPadOS 13이 되면서 키보드 + 화살표 기능이나 일부 단축키가 풀렸습니다.
2. code-server로 써본 결과 브라우저 이벤트, 특히 스크린과 터치 부분 이벤트가 여전히 꼬여있습니다.
3. **Please Don't try this at home. : 절대 집에서 따라하지 마세요**


## 어차피 VS Code도 Electron이면 앱으로 만들 수 있지 않나?
왜 iOS 앱스토어에 VS Code 웹앱이 없을까? 먼저 Apple과 WebKit에 맞춰서 설계되어 있지 않습니다. 그리고 라이센스 이슈라던가 정책적인 문제가 있습니다.

- 관련 링크 : https://github.com/Microsoft/vscode/issues/43947


## Code-Server로 브라우저를 띄워서 접속하면?
- [code-server](https://github.com/cdr/code-server)
- 브라우저만 있으면 VS Code를 사용 가능!
  - 단 초기 로딩시 다운로드 용량이 약 50MB입니다. VS Code를 통째로 받으니까요.
  - 만약 유사한 솔루션을 찾는다면 차라리 [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview)를 추천드립니다.
- 별이 많은 만큼 이슈가 깨알같습니다. 직접 빌드(비추천) 할 수도 있습니다.

### iPad를 위한 code-server의 수많은 이슈들
- 검색 결과 : https://github.com/cdr/code-server/search?q=ipad&type=Issues
- 쓰고싶다!(순한맛) : https://github.com/cdr/code-server/issues/89


## 정말 방법이 없는걸까?
당장은 없다. **무엇보다 어차피 개발자 도구도 못 쓰는데 이거 왜 하고 있는거지?** 사파리를 Mac에다 연결해서 개발자도구를 띄울 수 있지만 아무리 봐도 괴상한 모양새아닌가[..]

그리고 VS Code에서 에디터는 [모나코 에디터](https://microsoft.github.io/monaco-editor/index.html)라고 하위 프로젝트가 있다. 일반적인, 보통 웹 기반 위지윅 에디터가 그러하듯 보이는 커서가 사실은 평범한 커서가 아니고 텍스트에리어가 텍스트에리어가 아닌 것인데, 이런 자잘한 부분들을 iOS 환경에 맞춰서까지 커스터마이징이 되어 있진 않다~~MS가 왜 Apple을 지원해야하나~~.

### 구체적인 문제 예시
아마 이런 부분들이 고쳐지면 미래에는 사용할 수 있게되지 않겠지 않을까 하는 부분들 에시입니다. iPad로는 [Swift Playground](https://www.apple.com/kr/swift/playgrounds/)만큼의 편의성.. 아니 그냥 이거쓰러 갑니다.

#### 크기
![1](/assets/resource/19-06-15/1.png)

스플릿뷰만 봐도 당장 크기에 관한 이벤트가 사파리는 특히 다른 브라우저들에 비해서 독특한 경우가 많은데.. 실제로 VS Code를 돌려서 몇 분 사용해보면 가운데 정렬이라던가 Resize 이벤트가 비정상적으로 동작하는 경우가 많습니다.

#### 입출력 이벤트
![4](/assets/resource/19-06-15/m2.gif)

iPadOS 12 이하에서는 방향키조차 사용 불가능합니다. 안드로이드, 크롬탭북의 경우 가능하다고 하는데.. 당장 iOS 게임에서 텍스트 입력창이 따로 출력되는 그 구조의 영향아닐까 추측합니다.

[iPadOS 13 베타가 올라오자마자 code-server를 테스트한 분](https://github.com/cdr/code-server/issues/128#issuecomment-498767692)의 내용은 아래와 같습니다. 그리고 실제로 테스트해보니.. 그렇게 편하지 않았습니다 ㅠ
- 방향키 가능
- cmd 이벤트 사용 가능
- 마우스는 단순 터치만 지원 (우클릭 안 된다는 뜻)
- cmd+shift+p 숏컷 사용 가능
- 단 복붙의 경우 cmd+v가 막힘, 이 경우 copy copy paste 확장으로 cmd+shift+v로 변경해주면 사용 가능[..]