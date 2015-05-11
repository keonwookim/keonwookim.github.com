---
layout: post
title: "Tomcat multi instance 생성하기"
---

# Tomcat multi instance
톰캣을 이용하다보면 하나의 서버에서 멀티 인스턴스를 실행시킬 일이 생기게 된다. 본인의 경우에는, 개발을 위한 테스트 환경때문에 멀티 인스턴스를 띄울일이 있었는데 톰캣에서의 멀티 인스턴스를 생성하는 방법에 대해 알아보도록 하자.
구동에 필요한 톰캣 및 JDK 설치는 완료되었다고 가정하고 진행하도록 하겠다.

## 인스턴스 디렉토리 생성
인스턴스들이 저장될 인스턴스들을 관리할 디렉토리를 생성한다. 적당한 위치에 디렉토리를 생성한다. 설치한 톰캣 디렉토리나 사용자 홈디렉토리 등을 이용해서 적당한 이름(instances, deploy or...)의 디렉토리를 생성한다. 인스턴스 디렉토리안에 실제 띄울 인스턴스 디렉토리를 생성한다. 본인은 api(~/instances/api)라는 이름으로 생성하였다.

* 생성한 디렉토리에 bin 디렉토리를 생성하고 $CATALINA_HOME/bin/tomcat-juli.jar 파일을 복사한다. 
* 인스턴스 디렉토리(~/instances/api)에 $CATALINA_HOME/conf 디렉토리를 복사하고,  logs, temp, work 디렉토리를 만들어준다.
* 인스턴스 디렉토리(~/instances/api)에 webapps 디렉토리를 만들고 $CATALINA_HOME/webapps/manager 디렉토리를 복사한다.

## 각 인스턴스를 위한 설정 변경
인스턴스 디렉토리(~/instances/api)에 conf/server.xml 파일을 수정한다.
conf/server.xml파일의 Connector port를 사용할 포트로 변경해준다. 또한 shutdown port를 찾아서 포트넘버를 적절히 바꿔준다. shutdown 포트는 해당 인스턴스의 shutdown명령을 받을 포트이다.

## startup.sh, shutdown.sh 파일 변경

$CATALINA_HOME/bin/startup.sh, $CATALINA_HOME/bin/shutdown.sh 파일을 수정한다.
\#bin/sh 아래 다음과 같이 입력한다.

    INSTANCE_NAME=$1
	export JAVA_HOME={자바 설치 위치}
	export CATALINA_HOME={tomcat 설치 위치}
	export CATALINA_BASE=$CATALINA_HOME/instances/$INSTANCE_NAME

## 실행

쉘에서 다음과같이 입력한다.

	$CATALINA_HOME/bin/startup.sh {실행할 인스턴스 이름}

위의 명령어를 실행하면 톰캣 인스턴스가 실행되며. logs디렉토리의 catalina.out 파일을 tail -f catalina.out 출력시켜서 잘 실행이 되는지 로그를 확인한다.

실행이 필요한 인스턴스들이 더 있다면 인스턴스 디렉토리(~/instance/) 밑에 생성해주고 위의 내용을 수행하면된다.

#Reference
* Running Multiple Tomcat Instances on Single Machine - http://www.ramkitech.com/2011/07/running-multiple-tomcat-instances-on.html