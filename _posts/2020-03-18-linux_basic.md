---
layout: post
title: " 리눅스 호스트명 변경(Ubuntu)"
comments: true
categories: Linux
---

우분투 16.04 기준 호스트명 변경 방법이다.

~~~
root@myname:~# hostname
myname
root@myname:~# hostnamectl set-hostname mynewname
root@mynewname:~# hostname
mynewname
~~~

