---
layout: post
title: "Play Framework for JAVA - PART 3 Play Project 살펴보기"
---

# Play Project 실행
이전 포스트를 잘 따라했다면, Idea 상에서 무리없이 실행이 될 것이다. 한 번 실행을 해보도록 하자. 메뉴의 Run > Edit Configurations..를 선택한 후 좌측 상단의 +버튼을 이용해 SBT Task를 선택한다. 선택하면 우측에 설정하는 부분이 나오는데 이름을 적당히 적어주고, Tasks에 run을 입력하면 된다. 이후 Alt+Shift+F10을 눌러서 실행해주면 된다.

제대로 프로젝트가 생성이 되었다면 콘솔창에 무언가가 주르륵 올라가면서 다음과 같은 문구가 표시될 것이다.

	--- (Running the application, auto-reloading is enabled) ---

	[info] play - Listening for HTTP on /0:0:0:0:0:0:0:0:9000

	(Server started, use Ctrl+D to stop and go back to the console...)

브라우저를 실행해서 주소창에 localhost:9000을 입력하면 Play 기본 페이지가 나타날 것이다!

# Play Project 구성

Play Project의 구조를 살펴보도록 하자. 프로젝트 디렉토리 내에는 여러 디렉토리가 존재한다. 간략히 설명하자면 다음과 같다.

	app - 애플리케이션 소스 디렉토리
    conf - 애플리게이션 설정파일 디렉토리
    public - Public assets (CSS, Javascript, images..)
    project - SBT 설정파일
    lib - library files
    logs - log 디렉토리
    target - 생성되는 파일들. (클래스파일 등.)
    test - unit 또는 functional test를 위한 디렉토리
조금 더 자세히 살펴보도록 하자.

## app 디렉토리

app 디렉토리에는 controllers, models, views 디렉토리가 존재한다. MVC패턴 구조를 취하고 있다. 앞서 말했듯이 실제 소스코드들이 저장되는 디렉토리이며. 이 패키지들 외에 자신만의 패키지를 따로 추가할수도 있다.

## public 디렉토리
static asset들이 저장되는 디렉토리이다. public디렉토리에는 images, javascripts, stylesheets 디렉토리가 존재한다.

## conf 디렉토리
애플리케이션의 설정파일들이 저장되는 디렉토리이다. 대표적으로 application.conf와 routes 파일이 위치하고 있다. application.conf 파일을 이용하여 애플리케이션 전반적인 설정을 할수있다. routes파일은 URL 라우팅 할수있는 파일이다.

## project 디렉토리
project 디렉토리에는 SBT 빌드 관련 파일들이 위치한다. plugins.sbt 파일은 해당 프로젝트에서 사용되는 플러그인들을 정의해놓은 파일이며, build.properties 파일은 애플리케이션에서 사용되는 SBT 버전 정보를 포함한다.

## target 디렉토리
target 디렉토리에는 빌드시스템에 의해서 생성되는 모든것들이 저장된다. 컴파일된 클래스파일등이 위치하게 된다.

