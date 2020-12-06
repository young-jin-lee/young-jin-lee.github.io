---
layout: post
title: "protocol"
comments: true
categories: git
---

## <u><b> Version Control System - git(4) </b></u>

### ◆ Git 실습 매뉴얼 목차

#### 1. [기초](./git(1)-basics.md)
#### 2. [branch 활용법](./git(2)-branches.md)
#### 3. [원격저장소(Remote Repository) 활용법](./git(3)-remote.md)
#### 4. [원격저장소와 통신 방식](./git(4)-protocol.md)
#### 5. [병합 충돌 및 해결](./git(5)-merge.md)
#### 6. [중요 기능](./git(6)-practical.md)
#### 7. <a href="./img/git_sts.pdf" download>Spring Tool Suite(IDE)에서 git 사용법</a>


## 로컬저장소와 원격저장소의 통신 방식: HTTPS, SSH

github clone에 보면 Clone with HTTPS와 SSH가 있다. 

<div align="center">
    <img src="/img/git/git(13)-protocol.PNG"  width="70%" height="70%">
</div>

HTTPS를 사용하면 별도의 설정없이 유저네임과 패스워드만 입력하기만 하면 된다. 하지만 push 할 때마다, 그리고 원격저장소를 사용할 때마다 로그인 절차를 걸쳐야한다. 이 절차를 스킵할 수 있는 통신방식이 SSH이다. 자동으로 로그인해주는 편의기능이 큰 장점이다.

### SSH 통신 방식 원리

- 로컬컴퓨터에서 하나의 쌍으로 매핑 되어있는 public key와 private key를 생성한다.
- public key를 원격저장소에 등록한다.
- 이제 로컬에서 원격으로 접근/요청을 보내면, 원격은 요청을 보내온 로컬에 자신이 가지고 있는 public key에 매핑이되는 private key가 있는지 확인한다.
- 로컬의 요청을 승인/거절 한다.

### 1. ssh key 생성하기

리눅스, Mac OS, 윈도우 10에서는 ssh-keygen 명령을 통해 ssh key pair를 생성한다.   
명령어: ssh-keygen -t rsa -N "" -b 2048 -C "comment" -f <path/KeyFileName>

<div align="center">
    <img src="/img/git/git(14)-protocol.PNG"  width="70%" height="70%">
</div>

<pre>
$ ssh-keygen -t rsa -N "" -b 2048 -C "ssh key for demo" -f ./id_rsa
Generating public/private rsa key pair.
Your identification has been saved in ./id_rsa.
Your public key has been saved in ./id_rsa.pub.
The key fingerprint is:
SHA256:SFo9Bzyr8dzoANqGZZ9oxwdZIBo+TCsJ9b8Ed5I8OlY ssh key for demo
The key's randomart image is:
+---[RSA 2048]----+
|..+ . .o.        |
|.= = o o+.       |
|o * o Eo+o.      |
| . .+X+=.o       |
|   *==+BSo       |
|  o.=oB.= .      |
|   o ..+         |
|        .        |
|                 |
+----[SHA256]-----+
</pre>

위에서 f 옵션을 통해 지정한 위치에서 id_rsa(private key), id_rsa.pub(public key) 파일을 확인한다.
<pre>
ls -al id_rsa*
-rw-------  1 taewan  staff  1823  1 17 15:25 id_rsa
-rw-r--r--  1 taewan  staff   398  1 17 15:25 id_rsa.pub
</pre>

### 2. id_rsa.pub의 내용을 복사한다.

<pre>
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
NrRFi9wrf+M7Q== youngjinlee@yjlee.local
</pre>

### 3. GitLab에 복사한 내용(public key)를 등록한다.

우측 상단 이용자 아이콘 클릭 → settings → SSH Keys 에서 붙여넣기 하고 Title: 현재 사용중인 로컬컴퓨터명을 입력하고 Add Key 버튼을 눌러 등록을 완료한다. 유효기간은 입력하지 않아도 된다. 
<div align="center">
    <img src="/img/git/git(15)-protocol.PNG"  width="70%" height="70%">
</div>

이제 로컬의 private key와 함께 생성된 원격저장소의 public키가 매핑되었다. 

### 통신 방식을 HTTP에서 SSH로 바꾸기

원격저장소의 url을 다시 설정하는 것이다.
변경이 되었는지는 git remote -v 명령어를 통해 확인한다.
~~~
git remote set-url origin git@github.com:username/repo-name-here.git
~~~

이제 해당 컴퓨터에서 원격저장소로 접근할 때(push) 자동로그인이 된다.