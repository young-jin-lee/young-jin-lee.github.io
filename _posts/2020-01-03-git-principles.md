---
layout: post
title: "Principles"
comments: true
categories: GIT
---

## <u><b> Version Control System - git(principles) </b></u>

<br>

### ◆ Git 실습 매뉴얼 목차

#### 1. [기초](./git(1)-basics.md)
#### 2. [branch 활용법](./git(2)-branches.md)
#### 3. [원격저장소(Remote Repository) 활용법](./git(3)-remote.md)
#### 4. [원격저장소와 통신 방식](./git(4)-protocol.md)
#### 5. [병합 충돌 및 해결](./git(5)-merge.md)
#### 6. [중요 기능](./git(6)-practical.md)
#### 7. <a href="./img/git_sts.pdf" download>Spring Tool Suite(IDE)에서 git 사용법</a>


<br/>

### 작업 중

### 1. git init

- git init을 하면, .git이라는 폴더가 생성된다. 이 폴더안에는 모든 git관련 파일이 있다.

### 2. f1.txt 만들기

- f1.txt에 a 라고 한 글자만 입력해서 생성한다. 

### 3. git status

- untracked file로 f1.txt를 확인할 수 있다.

※ 존재하는 파일에 대한 정보가 .git/object 안에 없는 경우 untracked file로 인지하는 것이다.

### 4. git add : object 폴더 안에 commit id 파일 생성

- 파일을 staging area에 올렸을 경우 .git/objects 안에 (디렉토리, 파일) 세트로 관리한다.

##### add 시 git 동작 순서

0. ./git 안에 index 파일이 생성된다.
<pre>
~/git_principles/.git (GIT_DIR!)$ cat index
DIRC9k۸9{T▒▒x▒"a;*▒`%/▒▒▒▒▒▒N▒f1.txt▒▒▒u^>1 U▒▒▒Ӱ͟C▒
</pre>
1. a 라는 문자를 sha1 해시 알고리즘으로 해시코드(40자리)를 만든다.
2. .git/object 안에 그 중 첫 2자리로 이름을 갖는 디렉토리를 생성한다.
3. 생성한 디렉토리안에 나머지 38자리의 이름을 갖는 파일을 만든다.

※ 어느 컴퓨터에서든 파일의 이름과 상관없이 <b>내용</b>이 같으면 동일한 해시코드(인덱스 값)을 가진다. 즉, 파일이 여러 개여도 내용이 같으면 하나의 오브젝트로서 관리가 되는 것이다.(.git/index 파일)

<pre>
~/git_principles/.git/objects (GIT_DIR!)$ ls
<b>78/</b>  info/  pack/

~/git_principles/.git/objects/78 (GIT_DIR!)$ ls
<b>981922613b2afb6025042ff6bd878ac1994e85</b>
</pre>

### 5. git commit

##### commit 시 git 동작 순서


### ※ git status

.git 디렉토리 밑에 index라는 파일이 있다.

index파일에는 commit하기 위해 준비된 내용들이(;staging area에 올라가있는 내용) 저장 되어있다.

working tree, workspace - index, staging area, cache - repository

이런 구조에서 만약 workspace에서 f1.txt파일의 변경이 일어났다면 index와 내용이 다르므로 git status 명령어를 입력했을 때 수정된 파일이 있다고 파악하는 것이고 commit한 데이터와 index파일이 다르면 commit해야할 파일이 있다고 파악하는 것이다.


<div align="center">
    <img src=".https://sjh836.tistory.com/37"  width="70%" height="70%">
</div>