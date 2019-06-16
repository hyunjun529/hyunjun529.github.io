---
layout: post
title: GitHub Pages에 대한 여러가지
categories: [개발환경구축]
tags: [GitHub Pages, Blog]
---

# GitHub Pages에 대한 여러가지
텍스트큐브, 워드프레스, 티스토리, 네이버 블로그, 텀블러, 블로거를 거쳐서 결국 다시 GitHub Pages에 Jekyll 기반으로 블로그를 또 해보고 있는데 편한 만큼 뭔가 알아둬야 할 것 같은.. 잘 설명되어 있지 않은 부분들이 있어서 조사하고 정리하는 글


## How to work GitHub Pages
- 공식 문서 링크 + 요약

### Jekyll on GitHub
- GitHub에 맞춰져서 뭐가 좀 다른가

### Hexo??
- Jekyll like

### GitHub Pages? IO? Jekyll?
- 차이점 서술

### _post에 저장한 MarkDown이 어떻게 HTML로 변환되는건데?
- Jekyll


## 관리 방법에 대해서
1. 백업은 Git만 갖고 다녀도 그 자체로 완전하도록
2. 링크의 영속성, 호스팅이 바뀌어도 유지되도록
3. 컨텐츠 관리

### Google Analytics
- 적당히 붙인다.
- 자세한 내용은 나중에
- 상술한 링크의 영속성 관점에서 도메인을 바꾸고 관리해야 하는데 귀찮다[..] 나중에
  - 에드센스 광고를 달려면 해야됨
  - 에전엔 학교 과제 같은거 올려봐야 네이버 카페나 커뮤니티로 갔는데,
  - 요즘엔 구글에 치는 사람이 많은지 이상하게 학교과제 시리즈 조회수가 비정상적으로 높음
  - ~~돈을 벌려면 학교 과제에 관한걸 달자~~

### Media Contents
- 코드 Highlights는 Gist나 직접 GitHub 코드 링크
  - 왠만하면 스샷 자제하자
- 이미지는 후술
- 큰 용량 파일은 Git으로 관리하기 보다는 외부 저장소(Git LFS가 싫음)
- 동영상은 유튜브

#### File attachments on issues and pull requests
- 공식 링크
- 자세한 내용은 딱히 없음, 근데 의외로 많이 사용되는 기능

#### 속도 비교
- CDN이니까 빠르지 않을까(?)
- Repo에 직접 올린거 vs CDN
- CDN 사용시 빠르면 gif 정도나 zip파일 링크는 그 쪽으로 땡길만 하다
- 속도에 이점도 없으면 언제 사라질지 모르는 불안정한 링크를 사용하는 것은 비추
- 결과 : TBD