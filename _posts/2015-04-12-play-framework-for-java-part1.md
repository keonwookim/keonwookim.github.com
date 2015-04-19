---
layout: post
title: "Play Framework for JAVA PART 1"
---

#Play Framework

## Play Framework 란?
Play Framework는 JAVA의 경량 웹 애플리케이션 프레임워크이다. Play Framework의 공식 페이지에서는 다음과 같이 정의한다.

> Play Framework makes it easy to build web applications with Java & Scala.

웹 애플리케이션을 쉽게 빌드할 수 있게 해주는 프레임워크임을 강조하고 있다.
좀 더 자세하게는 다음과 같이 설명하고 있다.

> Play is based on a lightweight, stateless, web-friendly architecture.
> Built on Akka, Play provides predictable and minimal resource consumption (CPU, memory, threads) for highly-scalable applications.

간단히 말하면, 플레이 프레임워크는 경량이며, stateless하고 웹 친화적이다. 또한 Akka기반이고, 고확장성을 위해 예측 가능하고 최소한의 자원을 소비할 수 있도록 지원한다. 한가지 특이한 점은 Java와 Scala 둘다 지원 한다는 점이다. Java Framework로 시작해서 2.0 버전대에서는 Scala 의 비중이 높아졌다고 한다. 내부코드도 Scala로 재작성 되었다고 한다. (Scala가 매우 마음에 들었던 모양..) 이와같은 이유로 공식 사이트에서도 Java와 Scala 두가지 버전의 문서를 제공하고 있다. Scala기반으로 재작성 되면서 빌드 툴로 SBT를 이용하고 있다. 다음으로 SBT에 대해서 알아보도록 하자.

## SBT
SBT는 Scala 기반의 빌드 도구이다. 많이 사용되고 있는 maven을 생각하면 쉬울것이다. 공식사이트의 설명을 보자. 다음과 같이 설명하고 있다.
> sbt uses a small number of concepts to support flexible and powerful build definitions.

sbt는 컨셉 자체가 심플한듯하다. 부가적으로 여타 다른 빌드 도구들 (문서를 읽지 않고서는 사용하기 힘든) 보다 간단한 사용성을 강조하고 있다.

##### 프로젝트 디렉토리 구조
프로젝트의 소스 디렉토리 구조는 다음과 같다.

![디렉토리 구조]({{ D:\PortableJekyll\keonwookim.github.com\assets\project_structure.jpg }}/assets/project_structure.jpg)

기본적으로 Maven과 같은 디렉토리 구조를 사용하고 있다.

build.sbt 파일에는 빌드 정의가 저장되어 있으며, 프로젝트의 베이스 디렉토리에 위치한다. /project디렉토리내에는 .scala파일이 위치할 수 있는데 이 .scala파일은 .sbt파일과 결합되어 완전한 빌드 정의를 구성하게 된다.

빌드의 결과물들(컴파일된 클래스들, jar파일, documentation등)은 /target 디렉토리 내에 위치하게 된다.

##### 주요 명령어
* clean - target 디렉토리 내의 생성된 파일들을 모두 지운다.
* compile - 메인 소스를 컴파일한다. (src/main/scala, src/main/java)
* test - 컴파일하고 모든 테스트를 실행한다.
* console - Scala interpreter를 시작한다. 종료는 Ctrl+D (Unix), Ctrl+Z (Windows)
* run - 메인 클레스를 실행한다.
* package - jar 파일을 생성한다.
* help - 해당 커멘드 사용법을 보여준다.
* reload - 빌드 정의를 reload 한다.

##### SBT in Play Framework
위에서 언급했던 것처럼 Play Framework에서는 SBT를 빌드툴로 이용하고 있다. Dependency 관리는 Apache Ivy를 이용한다. 좀더 정확하게 이야기하자면 SBT를 통해서 이루어지고 있다. Dependency는 build.sbt 파일에 정의해서 관리할수 있다. 문법은 다음과 같다.

{% highlight scala %}
libraryDependencies += "group" % "artifact ID" % "revision" % "configuration"
{% endhighlight %}

예를 들자면 실제 다음과 같은 형태로 정의되어 있을 것이다.

{% highlight scala %}
libraryDependencies += "org.codehaus.jackson" % "jackson-core-asl" % "1.6.1"
{% endhighlight %}

이러한 dependency list를 build.sbt 에 정의하는 것으로 dependency 관리를 할수 있다.

SBT는 기본적으로 Maven2 repository와 the Typesafe Releases repository를 사용한다. 이외의 다른 repository를 이용하고 싶다면 다음과 같이 추가해주면 된다.

{% highlight scala %}
resolvers += 저장소 위치
{% endhighlight %}

## Reference
Play Framework - https://www.playframework.com
SBT - http://www.scala-sbt.org/


