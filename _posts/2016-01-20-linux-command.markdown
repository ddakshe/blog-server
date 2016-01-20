---
layout: post
title:  "리눅스 명령어 모음"
date:   2016-1-20 21:58:52 +0900
categories: jekyll update
---



# adduser

~~~
권한 : root
설명 : 이용자를 추가한다.

ex)
 # adduser kennen
  ** kennen 이란 유저를 추가한다.
 # adduser -p 1234 -g dev -s '/bin/bash' -d /home/dev' kennen
  **
   유저이름 : kennen
   비밀번호 : 1234
   그룹 : dev
   쉘 : bash
   홈디렉토리 : /home/dev
   의 유저 등록
~~~


# alias

~~~
설명 : 명령어를 수정한다.

ex)
 $ alias ls='ls --color=auto -altr'
~~~