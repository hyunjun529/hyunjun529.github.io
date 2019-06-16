---
layout: post
title: WSL으로 윈도에서 Jekyll 쓰기
categories: [개발환경구축]
tags: [삽질, WSL, Jekyll]
---

Win10에서 Jekyll을 깔고 GitHub에 블로그 올린 과정에서 벌어진 삽질 기록입니다.

# 세줄요약
1. Windows 10 말고 WSL Ubuntu로 설치합시다.
2. Jekyll 관련해서는 StackOverflow나 GitHub Issue보다 에러 메세지를 읽읍시다.
3. 기본 설치, jekyll 기동에 대해서만 다룹니다.

## 준비물
- Windows 10, Ubuntu Subsystem
- Git, SourceTree
- VSCode

## 방법
1. 윈도우에서 우분투부터 깔자 https://docs.microsoft.com/en-us/windows/wsl/install-win10
2. Jekyll을 설치하자 https://jekyllrb.com/docs/installation/ubuntu/
3. `/mnt/c` 로 우분투에서 윈도우 파일 시스템을 접근할 수 있다.
4. GitHub에 자기 아이디로 된 저장소를 만든다 https://pages.github.com/
5. SourceTree로 윈도에서 클론, 단 이 때 **경로에 스페이스 없도록 주의** https://github.com/tmm1/http_parser.rb/issues/47#issuecomment-454397300
6. Jekyll의 `_config.yml`, `Gemfile`을 수정 https://jekyllrb.com/docs/step-by-step/10-deployment/
7. 다시 `bundle exec jekyll serve` 테스트해본다.
8. vendor 아래에 있는건 적절히 .gitignore처리하고 업로드

## 주의사항
윈도우의 경우 [RubyInstaller](https://rubyinstaller.org/)를 사용할 수 있지만 여기서 뭔가 꼬이면 해결하기가 더 어렵습니다(주관적인 의견입니다).

### http parser 어쩌고 0.6.0 에러
Jekyll 설치 경로에 스페이스가 있습니다.

### norigoki 관련 에러
zlib, xml 어쩌고를 깔아주셔야 합니다. https://github.com/mmistakes/minimal-mistakes/issues/328
해당 링크가 원인이긴한데 에러메세지를 읽어보시면 뭐가 빠졌는지 나옵니다.
