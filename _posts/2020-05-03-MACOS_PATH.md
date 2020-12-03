---
layout: post
title: "MAC OS(CATALINA): ENVIRONMENT VARIABLE PATH"
comments: true
categories: MacOs
---


### 터미널을 통한 맥 카탈리나 쉘 경로 지정

쉘 경로란 파일시스템 경로들의 집합이다. 사용자는 특정 명령이나 프로그램, 어플리케이션을 실행 파일이 위치한 경로까지 찾아 들어가지 않아도 사용할 권한을 얻는다.

예를 들어 mysql을 실행하기 위해서

$ /usr/local/mysql/bin/mysql

이라고 입력하는 대신,

$ mysql

만 적어주면 된다.(현재 터미널상 사용자의 위치와는 관계없이)

쉘경로는 파일시스템의 절대경로의 집합으로 이루어져 있으며, 콜론(:)으로 구분된다. 이를 확인하는 위해서는 다음과 같이 터미널에 입력하면 된다.

$ echo $PATH

/Users/mymac/opt/anaconda3/bin:/Users/mymac/opt/anaconda3/condabin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

#### 임시로 경로를 추가하고 싶다면...

예를 들어 usr/local 을 경로로 추가하고 싶다면

$ PATH= /Users/mymac/opt/anaconda3/bin:/Users/mymac/opt/anaconda3/condabin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:usr/local

echo $PATH로 얻은 결과 뒤에 콜론을 붙이고 추가할 경로를 입력하면 된다.

#### 완전히 경로를 추가하고 싶다면...

cd 를 통해 홈디렉토리로 이동하고 .zshenv라는 파일을 생성한 후, 경로를 입력한다.

$ cd
$ nano .zshenv
// PATH="/usr/local:$PATH"

입력 후 컨트롤 o, 엔터, 컨트롤 x를 누르고 빠져나온 뒤, 새로 터미널을 열어 echo $PATH의 결과에서 경로가 추가된 것을 확인할 수 있다.

#### 경로 검토 순서 바꾸기

경로 검토 순서를 바꾸려면 .zshenv파일에서 $PATH 부분을 지우고 전체 경로 집합을 원하는 순서대로 작성하면 된다.



