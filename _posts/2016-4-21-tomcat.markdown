---
layout: post
title:  "tomcat 설치하기"
date:   2015-11-19 21:58:52 +0900
categories: jekyll update
---

Tomcat 설치하기

내가 원하는 곳에 설치하 기 위해
다음의 명령어로 tar.gz 압축파일을 다운로드 받는다.


```
wget http://mirror.apache-kr.org/tomcat/tomcat-8/v8.0.5/bin/apache-tomcat-8.0.5.tar.gz
```


그리고 설치하기 원하는 곳으로 이동해서 압축을 푼다.

```
$ mv apache-tomcat-8.0.5.tar.gz /data/program
$ cd /data/program
$ tar xvfz apache-tomcat-8.0.5.tar.gz
$ ln -s tomcat apache-tomcat-8.0.5.tar.gz tomcat
```


이제 Java, apache, tomcat 설치 했으니 전체적인 환경변수 등록을 한다.

```
$ vim /etc/profile

-- 아래네용 추가 --

JAVA_HOME=/data/program/java7
JRE_HOME=/data/program/java7/jre
CATALINA_HOME=/data/program/tomcat
APACHE_HOME=/usr/local/apache/
CLASSPATH=$JAVA_HOME/lib/tools.jar;
PATH=$PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$APACHE_HOME/bin:
export JAVA_HOME
export JRE_HOME
export CATALINA_HOME
export APACHE_HOME
export CLASSPATH
export PATH

```

그리고 profile 파일을 적용하고

```
source /etc/profile
```

톰캣을 시작한다.

```
/data/program/tomcat/bin/startup.sh
```

종료

```
/data/programe/tomcat/bin/shutdown.sh
```






