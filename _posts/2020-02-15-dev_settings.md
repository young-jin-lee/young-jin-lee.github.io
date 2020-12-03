---
layout: post
title: "파이썬 설치 및 가상환경 구축(mac os)"
comments: true
categories: Python
---

개발환경을 구축할때마다 찾아서 해도 되지만 일관되게 하지 않으면 같은 문제를 만나도 다르게 풀어야한다. 
그래서 소개한다. 나는 이 방법이 편하고 아직까지 큰 문제는 겪지 않았다. 

파이썬 설치와 가상환경 구축에는 여러가지 방법이 있다. 공식사이트에서 직접 다운로드 받는 방법도 있지만, 향후 PATH 변수, 환경 변수를 다루는게 쉽지 않고, 업데이트나 삭제하는데도 어려움이 생긴다. 꼬이면 굉장히 골치 아프다. 따라서 Homebrew라는 유명한 패키지 매니저를 통해 자동 업데이트나 여러 버전의 파이썬을 쉽게 관리 하는 방법을 소개한다.

보통 mac에는 python2가 default로 설치되어있다. 하지만 요즘엔 3를 많이 사용하기에, Homebrew를 이용하여 python을 설치하고, 패키지의 여러 버전을 사용할 수 있도록 가상환경을 구축해보겠다.

1. 현재 설치되어있는 파이썬 버전 확인
2. XCode 설치
3. Homebrew 설치
4. Python3 설치
5. 가상환경 구축

### 현재 설치되어있는 파이썬 버전 확인
아래 세가지 방법을 통해 파이썬 버전을 확인할 수 있다. python --version에는 그 아래 2나 3중 하나가 물려있을 것이다.
~~~
$ python --version
$ python2 --version
$ python3 --version
~~~

### Install XCode
Apple의 Xcode 프로그램을 설치한다. 이것은 IOS개발이나 대부분의 프로그래밍 작업에 필요하다. XCode를 이용해 Homebrew를 설치한다.
~~~
$ xcode-select --install 
~~~

### Install Homebrew
~~~
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
~~~

~~~
$ brew doctor
Your system is ready to brew
~~~

### Install python3

~~~
$ brew install python3
~~~

~~~
$ python3 --version
python3.7.5
~~~

~~~
$ python3
Python 3.7.5 (default, Feb  15 2020, 02:16:23)
[Clang 11.0.0 (clang-1100.0.33.8)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
~~~
파이썬 쉘에서 빠져나오고 싶으면 exit() 이라고 입력하면 된다.

### 가상 환경 구축

설치한 파이썬을 통해 가상환경은 venv, virtualenv, pipenvd 세 가지로 구축할 수 있다. (이외에도 anaconda와 같은 패키지 매니저를 통해서도 할 수 있다.)

먼저 Homebrew를 이용해 pipenv를 설치한다.

~~~
$ brew install pipenv
~~~

이제 프로젝트 폴더로 이동한다.
예시)

~~~
$ cd ~/Desktop
$ mkdir my_project
$ cd my_project
~~~

프로젝트의 기반이 되는 API(파이썬, 장고 등)를 설치한다.

~~~
$ pipenv install python==3.7.5
~~~

프로젝트 폴더 안에는 pipenv가 사용하는 Pipfile과 Pipfile.lock이 생성된다.
가상환경에 들어가기 위해서는 pipenv shell을 입력하면 되고, 빠져나올땐 exit을 입력하면 된다.

~~~
$ pipenv shell
(my_project) $
(my_project) $ exit
$
~~~

