---
layout: post
title: GitHub Pages에 대한 여러가지
tags: [개발환경구축, GitHub Pages, Blog]
---

# GitHub Pages에 대한 여러가지
텍스트큐브, 워드프레스, 티스토리, 네이버 블로그, 텀블러, 블로거를 거쳐서 결국 다시 GitHub Pages에 Jekyll 기반으로 블로그를 또 해보고 있는데 편한 만큼 뭔가 알아둬야 할 것 같은.. 잘 설명되어 있지 않은 부분들이 있어서 조사하고 정리하는 글

1. Jekyll과 GitHub Pages에 대한 조사
2. 블로그 운영 방법에 대한 메모


## How to work GitHub Pages

<div class="wrapper-youtube">
<iframe class="video-youtube" width="560" height="315" src="https://www.youtube.com/embed/2MsN8gpT6jY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

- GitHub에 블로그를 만든다 = 유저 혹은 조직 계정으로 [github.io](https://github.io) == [GitHub Pages](https://pages.github.com/)를 만든다.
- [Wikipedia:GitHub Pages](https://en.wikipedia.org/wiki/GitHub#GitHub_Pages)
  - 2008년부터 서비스한 정적 웹 호스팅 서비스
  - 유저 블로그, 프로젝트 문서 등의 용도로 제공
  - Jekyll 정적 웹 사이트, 블로그 제네레이터, 깃헙을 매끄럽게 합쳐 놓은 파이프라인 사용 가능
  - [만약 Jekyll을 안 쓰면](https://help.github.com/en/articles/using-a-static-site-generator-other-than-jekyll) HTML 파일로 올려서 할 수도 있다.

### Jekyll on GitHub
- [공식 문서](https://help.github.com/en/articles/using-jekyll-as-a-static-site-generator-with-github-pages)
- 미리 설정된 publishing branch가 push되면 자동으로 publish한다. 
- [Jekyll Gem-based Theme 패키지](https://jekyllrb.com/docs/themes/)를 [_config.yml에 등록해서 쓸 수도 있다](https://help.github.com/en/articles/adding-a-jekyll-theme-to-your-github-pages-site).
- 지정된 테마를 쓰고 않거나 직접 수정해서 쓰고 싶은데 Gem으로 올리기는 귀찮을 경우, [직접 오버라이딩](https://help.github.com/en/articles/adding-a-jekyll-theme-to-your-github-pages-site)해서 사용할 수도 있다.
  - 이 경우 `_layouts`, `_includes`, `_sass`, `_assets` 를 오버라이딩할 수 있다.

### Hexo??
- Jekyll like 하다
  - _config.yml, YAML지원, _post나 _assets 같은 경로가 비슷하게 지원된다.
  - 근데 빌드해서 따로 정적으로 올리는게 추천되는 것 같다.
- Jekyll이 Ruby Gem인 반면 무려 Node.js, npm을 쓴다.
- 더 이쁘고 최신 스킨이 많다.
- 사담) 그런데 굳이..? 어째서..? 확장기능 붙이고 설정하고 뭐하느니 그냥 짜서 쓰는게 더 빠른거 같아서 안 쓸래... 정적 웹 페이지 배포하는데 어째서 이걸 써야하는거야...

### 선택
- [https://github.com/hyunjun529/hyunjun529.github.io](https://github.com/hyunjun529/hyunjun529.github.io)
- Jekyll, 테마 오버라이딩.


## 관리 방법에 대해서
1. 백업은 Git만 갖고 다녀도 그 자체로 완전하도록하자
2. 링크의 영속성, 호스팅이 바뀌어도 유지되도록, 왠만하면 URL은 영어만, 한글이 포함되지 않도록!
3. 코드나 무거운 컨텐츠 관리는 각각 방법을 세울 것, 자체 Hightlight를 사용할 필요가 있을까?

### Google Analytics
- 적당히 붙인다.
  - https://github.com/hyunjun529/hyunjun529.github.io/blob/master/_includes/google-analytics.html
  - https://github.com/hyunjun529/hyunjun529.github.io/blob/master/_includes/head.html#L11
  - 내 경우 jekyll.environment == 'production;를 사용해도 되는데 그냥 jekyll에 의존하고 싶지 않아서 일부러 사용하지 않았다.
- 이런 추적 기능이 없으면 내 블로그에 누가 어떻게 오는지 확인할 수가 없으니까 꼭 달자.
- 상술한 링크의 영속성 관점에서 도메인을 바꾸고 관리해야 하는데 귀찮다[..] 나중에
  - 에드센스 광고도 달아보고 싶다.
  - 에전엔 학교 과제 같은거 올려봐야 네이버 카페나 커뮤니티로 갔는데 요즘엔 구글에 치는 사람이 많은지 이상하게 학교과제 시리즈 조회수가 비정상적으로 높음 ~~돈을 벌려면 학교 과제에 관한걸 만들자~~

### Media Contents
- 코드 Highlights는 Gist나 직접 GitHub 코드 링크
  - 왠만하면 코드 스샷은 자제하자
- 이미지는 후술한 방법을 사용하자
- 큰 용량 파일은 Git으로 관리하기 보다는 외부 링크로 만들 것(Git LFS가 싫음)
- 동영상은 유튜브, 임베딩.

#### File attachments on issues and pull requests
- [공식 링크](https://help.github.com/en/articles/file-attachments-on-issues-and-pull-requests)
  - [image URLs](https://help.github.com/en/articles/about-anonymized-image-urls)
  - 
- 꽤 많은 프로젝트에서 사용되는 기능
  - 몇몇 프로젝트는 GitHub.io를 쓰는 경우 문서화에도 권장하는데, 리소스 관리가 편하기 때문으로 추정
  - 또 비교적 큰 파일도 안정적으로 올릴 수 있고, GitHub의 issue나 wiki 등에서는 유용하기 때문에 여기서 발생한 컨텐츠를 그대로 재활용하기도 쉬움
- HTTPS 이슈를 피하기 위해서 [Cameo](https://github.com/atmos/camo) http 이미지 프록시를 사용함
  - 편하지만 이 경우 이미지의 메타데이터가 대부분 손실 됨
  - 원래 이미지와 전혀 다른 URL, 의미를 넣는다거나 제어하기가 쉽지 않음.
  - 대신 대부분 이미지 업로드와 달리 이미지 원본을 보존해주는 장점이 있다.

#### 속도 비교
- CDN이니까 빠르지 않을까(?) 하는 막연한 기대심리에 대해서 확인
- Repo에 직접 올린 파일 vs issue에 올린 사진(CDN)
- CDN 사용시 빠르면 gif 정도나 zip파일 링크는 그 쪽으로 땡길만 하다!
- 속도에 이점도 없으면 언제 사라질지 모르는 불안정한 링크를 사용하는 것은 비추

![compare.png](/assets/resource/19-06-15/3.PNG)

- 결과 1 : 오히려 느리다.
- 결과 2 : 느리고 제어하기 힘들면 이점이 없다.
