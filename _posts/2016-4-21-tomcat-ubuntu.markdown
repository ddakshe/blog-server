---
layout: post
title:  "ubuntu tomcat 설치 / 80포트 사용하기"
date:   2016-5-3 12:10:00 +0900
categories: jekyll update
---

# tomcat 설치 #

먼저 ubuntu에서는 apt-get으로 tomcat을 설치한다.

apt-get이 동작하지 않으면 update를 먼저 해본다.

```
sudo apt-get update
```

tomcat을 검색한다.

```
apt-cache search tomcat7
```

tomcat을 설치한다.

```
sudo apt-get install tomcat7
```

tomcat이 설치 완료 되었다.



# tomcat 80포트 사용하기 #

1. /etc/tomcat7/server.xml 파일 수정

```
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               URIEncoding="UTF-8"
               redirectPort="8443" />
```
8080 -> 80 으로 수정

2. etc/default/tomcat7 파일 수정

```
#AUTHBIND=no
```
주석 제거 하고 yes로 변경


3. /etc/authbind/byport/80 파일 추가

```
sudo touch /etc/authbind/byport/80
sudo chmod 500 /etc/authbind/byport/80
sudo chown tomcat7 /etc/authbind/byport/80
```

4. 실행

```
sudo service tomcat7 restart
```















