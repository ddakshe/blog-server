---
layout: post
title:  "Linux Proxy 설정"
date:   2016-2-15 12:10:52 +0900
categories: jekyll update
---

#1 **Linux Proxy 설정**

리눅스 환경에서 외부에 있는 파일을 다운로드 할 경우 리눅스 종류에 따라
yum, apt-get, wget 등으로 다운로드 하게 된다.

이때, 네트워크가 정상적으로 동작하더라도 외부로 나가는 트래픽이 막혀있는 서버의 경우 외부와 통신이 불가능 하기 때문에 timout에러가 발생한다.

이런 경우에 Proxy를 통해 외부와 통신을 가능하게 할 수 있다.

Proxy를 사용하기에 앞서 Proxy 서버가 존재해야 가능하다.

지금은 Proxy 서버가 존재한다는 가정하에 사용법을 작성한다.


2가지 방법이 있다.

### **1-1.첫번째는 로그인 세션 동안 유지하는 방법이다.**

~~~
export http_proxy=[PROXY_SERVER_URL]:[PROXY_SERVER_PORT]
export HTTP_PROXY=$http_proxy
export https_proxy=$http_proxy
export HTTPS_PROXY=$http_proxy
export no_proxy="localhost,127.0.0.1,..등등"
export NO_PROXY=$no_proxy
~~~

http와 https 두개 모두 등록해 줘야 한다.
no_proxy 의 경우 이에 해당하는 url 또는 IP의 경우에는 Proxy 서버를 통하지 않아도 된다는 뜻이다.


### **1-2.두번째는 로그인 세션이 종료된 후 재 로그인을 했을 경우에도 Proxy를 유지하는 방법이다.**

~~~
export http_proxy=[PROXY_SERVER_URL]:[PROXY_SERVER_PORT]
export HTTP_PROXY=$http_proxy
export https_proxy=$http_proxy
export HTTPS_PROXY=$http_proxy
export no_proxy="localhost,127.0.0.1,..등등"
export NO_PROXY=$no_proxy
~~~


# 2. sudo를 이용한 Proxy 사용

루트 권한일 경우에는 특별한 문제가 되지 않는다.
하지만 일반권한으로 proxy 서버를 통한 파일 다운로드 등을 할 때 권한이 없기 때문에 permission denided 가 발생하는 경우가 생긴다.

보통 이럴 경우 sudo를 이용한다. ([sudo??](https://ko.wikipedia.org/wiki/Sudo))

하지만 sudo를 사용해 다운로드를 시도할 경우에 proxy 서버를 타지 않는 경우가 있다.

예를 들어,

```
$ wget --no-check-certificate --no-cookies --header "Cookie:
oraclelicense=accept-securebackup-cookie"
http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz
```

위의 내용대로 실행하게 되면 persmission denied 가 발생한다.

그래서 sudo를 사용한다. (root 권한이 없기 때문에)

```
$ sudo wget --no-check-certificate --no-cookies --header "Cookie:
oraclelicense=accept-securebackup-cookie"
http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz
```

하지만 이때, persmission denied 는 발생하지 않지만 timeout 에러가 발생하는데, proxy가 sudo설정에 적용되지 않았기 때문이다.

자세한 설명은 구글링을 통해 확인해보면 될 것이다. (검색은 linux proxy sudo 등으로..?)

간단하게 확인할 수 있는 방법이 있다.

1-2에서 설명한 지속적으로 proxy를 유지하기 위해 ~/.bashrc 파일을 수정하고 적용까지 한 뒤에

~~~
$ env
또는
$ env |grep http_proxy
~~~

~~~
$ sudo env
또는
$ sudo env |grep http_proxy
~~~

두개의 명령어를 입력 후 결과를 확인해보자.

그러면 대부분의 경우 env 명령어만 입력한 경우 http_proxy 의 내용이 출력되는 반면, sudo env 명령어를 입력하는 경우 http_proxy의 내용이 없을 것이다.

이유를 간단하게 설명하면 sudo 명령어를 할 경우 /etc/sudoers (centos 기준) 파일에서 환경을 reset 하게 된다.
이때, bashrc 파일에서 적용한 http_proxy 관련 내용이 사라지게 된 것이다.

해결 방법은 간단하다.

~~~
$ cd /etc
$ sudo visudo -f sudoers
~~~
--visudo = vim 또는 vi 라고 생각하면 된다.


sudoers 내용 중에
Defaults    env_reset 를 검색한다.
그리고 그 아래 내용에  "Defaults    env_keep += 블라블라 " 라고 되어 있는 마지막 줄 아래 한줄을 더 추가한다.


~~~
Defaults    env_reset
Defaults    env_keep += "블라브라"
...
...
Defaults    env_keep += "블라블라"
~~~
아래 추가할 내용


~~~
Defaults    env_keep += "ftp_proxy http_proxy https_proxy no_proxy"
~~~

이렇게 적용한 다음 sudo 로 파일 다운로드를 실행하면 잘 될 것이다.

# ***여기서 주의해야 할 점이 한가지 있다.
/etc/sudoers 파일을 백업한다는 생각에 sudoers_bak 파일을 만들고 sudoers 파일을 실컷 주무르다가 "에이 모르겠다 다시 복구시켜!" 라고 생각한 다음
sudoers 파일을 삭제 한 뒤 sudoers_bak 파일을 "sudo mv sudoers_bak sudoers" 라고 입력하는 순간, 3초 전에 나의 행동이 엄청나게 후회될 것이다.

###그렇다 sudoers 파일은 sudo 명령어를 사용하는데 필요하다........... 그런데 sudoers 파일이 없다....그런데 sudo mv 를 하고 있다...

당신은 서버 담당자에게 굽신거려야 할 것이다. 마치 이 글을 쓰고 있는 누군가 처럼...

