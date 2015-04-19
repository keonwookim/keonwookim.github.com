---
layout: post
title: "Play Framework for JAVA - PART 2 IDEA CE 에서 Play Framework 개발하기"
---

#Play Framework in IDEA Community Edition

IDEA Ultimate Edition에는 Play Framwork를 지원하는 Plugin이 존재한다. 하지만 Community Edition에서 Play Framework를 이용해 개발하려면 약간의 수고를 거쳐야한다..(유료과 무료의 차이..ㅠㅠ) 이번에는 IDEA Communit Edition에서 Play Framework를 이용하려면 어떻게 해야하는지 살펴보도록 하겠다.

## Play Framework 설치

우선 Play Framework 공식사이트(https://www.playframework.com/)에서 다운받아서 설치한다. 이후 적당한 경로에 압축을 풀고, 해당 경로를 Path에 등록하면 된다. 커멘드라인 창에서 'activator -help' 명령이 수행된다면 제대로 설치한 것이다.

## 애플리케이션 생성

커멘드라인 창에서 수행해야 한다. 커멘드라인 창에서 적당한 경로로 이동한다. 예를 들면, workspace같은 디렉토리를 이용하면 될듯하다. 이제 새 애플리케이션을 생성해야하는데, 다음의 명령어를 이용한다.

    activator new [application name] play-scala
    activator new [application name] play-java

앞선 포스트에서 Play Framework는 Scala와 java 두가지 언어를 지원한다. play-scala를 이용하면 Scala언어를 이용해서 개발할수 있도록 생성해주고 play-java를 이용하면 java를 이용해서 개발할수 있도록 생성해준다. 우리는 java를 이용할것이므로 play-java를 이용해서 프로젝트를 생성해주면 된다.

프로젝트 생성 이후 빼먹지 말아야 할것이 있다. 바로 IDEA용으로 세팅해주는 명령어 이다. 다음 명령어를 수행해 주어야 IDEA에서 에러없이 import 할수 있다.

    activator idea


## IDEA 로 import

생성한 프로젝트를 IDEA로 import 하기 위해 IDEA를 실행해보자. import 하기전에 플러그인을 받아야한다. Ctrl+Alt+S를 눌러서 Settings 창을 열고, Plugins 항목을 선택한다. 그다음 scala를 검색해서 설치하면된다. 만약 나오지 않는다면 Browse repositories... 버튼을 눌러서 찾아 설치한다. 여기까지 완료했다면 프로젝트를 가져올 차례이다. File > New > Project from Existing Sources.. 를 선택한다. 프로젝트 디렉토리를 선택하고, Import project from external model에서 SBT를 선택한다. 그 이후 세팅은 필요에 맞게 해주고 완료 해주면 성공적으로 import 될것이다.

# Reference
* Play Framework - https://www.playframework.com

