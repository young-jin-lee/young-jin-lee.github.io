
## <u><b> Version Control System - git(2) </b></u>

### ◆ Git 실습 매뉴얼 목차

#### 1. [기초](./git(1)-basics.md)
#### 2. [branch 활용법](./git(2)-branches.md)
#### 3. [원격저장소(Remote Repository) 활용법](./git(3)-remote.md)
#### 4. [원격저장소와 통신 방식](./git(4)-protocol.md)
#### 5. [병합 충돌 및 해결](./git(5)-merge.md)
#### 6. [중요 기능](./git(6)-practical.md)
#### 7. <a href="./img/git_sts.pdf" download>Spring Tool Suite(IDE)에서 git 사용법</a>

## ◆ branch란?

<div align="center">
    <img src="/img/git/git(2)-master.PNG"  width="70%" height="70%">
</div>

branch는 하나의 repository 안에서 소스를 나누어 관리하는 것이다. default로 master라는 branch가 자동 생성되며, 지금까지는 master branch에서 작업을 한 것이다.   
프로젝트를 개발 관점에서 component 별로 쪼갤 수 있을 때, 각각의 branch에서 작업 후 합쳐서 완성본을 만들면 된다.   
예) 기본 틀, 로그인, DB연동 등   

## ◆ GIT branch 실습

### 0. master branch에서 f1.txt 생성, commit 하기

입력할 내용: '1_1'   
※ commit 할 때 -m 옵션을 주고 쌍따옴표안에 메시지 내용을 적을 수 있다.
<pre>
$ vim f1.txt
$ git add f1.txt
$ git commit f1.txt -m "m_1_1"
[output]
[master (root-commit) 8bb2df2] m_1_1
 1 file changed, 1 insertion(+)
 create mode 100644 f1.txt
</pre>
<pre>
$ git log
[output]
commit 8bb2df2c03f57cbae194b45c2f76893f47e53574 (HEAD -> master)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 11:01:11 2020 +0900

    m_1_1
</pre>

<div align="center">
    <img src="/img/git/git-branches(1).PNG"  width="50%" height="50%">
</div>

### 1. master branch에서 f2.txt 생성

입력할 내용: '2_1'   
<pre>
$ vim f2.txt
</pre>

### 2. 새로운 branch 생성하기

exp라는 branch를 생성한다. <b>master branch에서 생성하기 때문에 master branch의 내용이 exp branch에 복제된다.</b>
<pre>
# exp branch 생성하기
$ git branch exp 
</pre>
현재의 branch 앞에는 * 이 붙어있다.

<div align="center">
    <img src="/img/git/git-branches(2).PNG"  width="50%" height="50%">
</div>

<pre>
# branch 목록, 현재 branch 확인하기
$ git branch 
  exp
* master
</pre>

### 3. 새로 만든 branch로 이동하기

위에서 git branch 명령어를 통해 현재 exp, master branch가 있으며 현재 master branch에 있음을 확인했다.
checkout 명령어는 exp branch로 이동할 때 사용한다.
<pre>
$ git checkout exp 
Switched to branch 'exp'
</pre>
<pre>
$ git branch 
* exp
  master
</pre>

<div align="center">
    <img src="/img/git/git-branches(3).PNG"  width="50%" height="50%">
</div>

### 4. log 확인하기

master branch를 복제했기 때문에 똑같은 로그가 남아있는 것을 확인할 수 있다.
<pre>
$ git log
commit 8bb2df2c03f57cbae194b45c2f76893f47e53574 (HEAD -> exp, master)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 11:01:11 2020 +0900

    m_1_1
</pre>

### 5. status 확인하기

status도 master branch와 같은 상황이다. 
<pre>
$ git status
On branch exp
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        f2.txt

nothing added to commit but untracked files present (use "git add" to track)
</pre>

### 6. f2.txt를 add하고 commit 하기

f2.txt는 master branch에서 생성했지만, git의 staging area와 repository에 등록하는 작업은 exp branch에서 했다.
이 때 두 branch간에 어떤 차이가 생기는지 확인해본다.
<pre>
$ git add f2.txt
$ git commit f2.txt -m "m_2_1"
[exp 973b5f9] m_2_1
 1 file changed, 1 insertion(+)
 create mode 100644 f2.txt
</pre>

<div align="center">
    <img src="/img/git/git-branches(4).PNG"  width="50%" height="50%">
</div>

### 6.1. 로그와 파일 목록 확인하기

두 번의 commit이 이루어졌으며, m_1_1은 master branch에서, m_2_1은 exp branch에서 commit 되었음을 확인할 수 있다. 파일 목록에서도 f1.txt, f2.txt를 확인할 수 있다. 
<pre>
$ git log
commit 973b5f97add2c4b83c9bcdfbec80a13d771b572e (HEAD -> exp)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 14:24:09 2020 +0900

    m_2_1

commit 8bb2df2c03f57cbae194b45c2f76893f47e53574 (master)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 11:01:11 2020 +0900

    m_1_1

$ ls
f1.txt  f2.txt
</pre>

### 6.2. master branch로 이동해서 로그와 파일목록 확인하기

master branch로 이동한다.
<pre>
$ git checkout master
Switched to branch 'master'
</pre>

<div align="center">
    <img src="/img/git/git-branches(5).PNG"  width="50%" height="50%">
</div>

<b>(중요) master branch에서의 로그에는 m_2_1 commit이 존재하지 않으며, 파일 목록에도 f2.txt가 없음을 확인할 수 있다.   
이를 통해 git으로 관리되고 있는 폴더는 일반 폴더와 다름을 알 수 있다. 하나의 폴더 안에서 브랜드 별로 commit 된 파일들이 관리 되는 것이다.
파일이 어디에서 생성되었는지 보다, 어떤 branch에서 add가 되고 commit이 되었는지가 중요하다는 것이다. </b>
<pre>
$ git log
commit 8bb2df2c03f57cbae194b45c2f76893f47e53574 (HEAD -> master)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 11:01:11 2020 +0900

    m_1_1

$ ls
f1.txt
</pre>

### 현재 이 프로젝트의 형상 관리는 아래 그림과 같은 상태이다.

<div align="center">
    <img src="/img/git/git(3)-branches.PNG"  width="70%" height="70%">
</div>

### 7. f3.txt 생성하고 commit 하기
<pre>
$ vim f3.txt
$ git add f3.txt
$ git commit f3.txt -m "m_3_1"
[master 0bc8693] m_3_1
 1 file changed, 1 insertion(+)
 create mode 100644 f3.txt
</pre>

<div align="center">
    <img src="/img/git/git-branches(6).PNG"  width="50%" height="50%">
</div>

### 7.1. 로그/파일목록 확인하기
<pre>
$ git log
commit 0bc8693c2ababed4eef898d31145ecc256176d32 (HEAD -> master)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 14:38:03 2020 +0900

    m_3_1

commit 8bb2df2c03f57cbae194b45c2f76893f47e53574
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 11:01:11 2020 +0900

    m_1_1

$ ls
f1.txt  f3.txt
</pre>

### 10. branch 별로 로그 시각화하기
<pre>
$ git log --branches --decorate --graph
* commit 0bc8693c2ababed4eef898d31145ecc256176d32 (HEAD -> master)
| Author: young-jin-lee <youngjin@gmail.com>
| Date:   Wed Jul 8 14:38:03 2020 +0900
|
|     m_3_1
|
| * commit 973b5f97add2c4b83c9bcdfbec80a13d771b572e (exp)
|/  Author: young-jin-lee <youngjin@gmail.com>
|   Date:   Wed Jul 8 14:24:09 2020 +0900
|
|       m_2_1
|
* commit 8bb2df2c03f57cbae194b45c2f76893f47e53574
  Author: young-jin-lee <youngjin@gmail.com>
  Date:   Wed Jul 8 11:01:11 2020 +0900

      m_1_1
</pre>

### 11. 시각화 간소화하기

<pre>
bash-3.2$ git log --branches --decorate --graph --oneline
* 55e3dc9 (HEAD -> master) m3_m1
| * 9cdf397 (exp) m2_m1
|/  
* f3078bc m1_m1
</pre>

### 12. f1.txt 수정하기

수정할 내용: 1_1 → 1_2
<pre>
$ vim f1.txt
$ git add f1.txt
$ git commit f1.txt -m "m_1_2"
[master 6a37ea3] m_1_2
 1 file changed, 1 insertion(+), 1 deletion(-)

</pre>

<div align="center">
    <img src="/img/git/git-branches(7).PNG"  width="50%" height="50%">
</div>

### 13. 로그 시각화하기

<pre>
$ git log --branches --decorate --graph
* commit 6a37ea376155c053ad7e00e765dd75ff8b87468a (HEAD -> master)
| Author: young-jin-lee <youngjin@gmail.com>
| Date:   Wed Jul 8 14:46:32 2020 +0900
|
|     m_1_2
|
* commit 0bc8693c2ababed4eef898d31145ecc256176d32
| Author: young-jin-lee <youngjin@gmail.com>
| Date:   Wed Jul 8 14:38:03 2020 +0900
|
|     m_3_1
|
| * commit 973b5f97add2c4b83c9bcdfbec80a13d771b572e (exp)
|/  Author: young-jin-lee <youngjin@gmail.com>
|   Date:   Wed Jul 8 14:24:09 2020 +0900
|
|       m_2_1
|
* commit 8bb2df2c03f57cbae194b45c2f76893f47e53574
  Author: young-jin-lee <youngjin@gmail.com>
  Date:   Wed Jul 8 11:01:11 2020 +0900

      m_1_1
</pre>

### 14. branch간 로그 차이 확인하기

exp에는 있는데 master에는 없는 커밋 내역
<pre>
$ git log -p master..exp
commit 973b5f97add2c4b83c9bcdfbec80a13d771b572e (exp)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 14:24:09 2020 +0900

    m_2_1

diff --git a/f2.txt b/f2.txt 
new file mode 100644
index 0000000..11b43c2
--- /dev/null
+++ b/f2.txt
@@ -0,0 +1 @@
+2_1

</pre>

### 15. branch간 파일 차이 확인하기

<pre>
$ git diff master..exp
diff --git a/f1.txt b/f1.txt
index 5c99658..94a0d26 100644
--- a/f1.txt
+++ b/f1.txt
@@ -1 +1 @@
-1_2
+1_1
diff --git a/f2.txt b/f2.txt
new file mode 100644
index 0000000..11b43c2
--- /dev/null
+++ b/f2.txt
@@ -0,0 +1 @@
+2_1
diff --git a/f3.txt b/f3.txt
deleted file mode 100644
index 4a3eba9..0000000
--- a/f3.txt
+++ /dev/null
@@ -1 +0,0 @@
-3_1
</pre>

### 16. 로그 그래프/파일 목록 확인하기

<pre>
$ git log --branches --decorate --graph --oneline
* 6a37ea3 (HEAD -> master) m_1_2
* 0bc8693 m_3_1
| * 973b5f9 (exp) m_2_1
|/
* 8bb2df2 m_1_1

$ ls
f1.txt  f3.txt
</pre>

### 17. exp의 작업을 master로 병합하기

<pre>
$ git branch
  exp
* master

$ git merge exp
Merge made by the 'recursive' strategy.
 f2.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 f2.txt
</pre>

<div align="center">
    <img src="/img/git/git-branches(8).PNG"  width="50%" height="50%">
</div>

### 18. 로그 그래프/파일 목록 확인하기

<pre>
$ git log --branches --decorate --graph --oneline
*   4d4ee46 (HEAD -> master) Merge branch 'exp'
|\
| * 973b5f9 (exp) m_2_1
* | 6a37ea3 m_1_2
* | 0bc8693 m_3_1
|/
* 8bb2df2 m_1_1

$ ls
f1.txt	f2.txt	f3.txt
</pre>

### 19. 현재 exp는 * | 6a37ea3 m_1_2, * | 0bc8693 m_3_1를 가지고 있지 않다. 일단 확인하기

<pre>
$ git checkout exp
Switched to branch 'exp'
</pre>

<div align="center">
    <img src="/img/git/git-branches(9).PNG"  width="50%" height="50%">
</div>

<pre>
$ ls
f1.txt  f2.txt

$ cat f1.txt
1_1

$ git log
commit 973b5f97add2c4b83c9bcdfbec80a13d771b572e (HEAD -> exp)
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 14:24:09 2020 +0900

    m_2_1

commit 8bb2df2c03f57cbae194b45c2f76893f47e53574
Author: young-jin-lee <youngjin@gmail.com>
Date:   Wed Jul 8 11:01:11 2020 +0900

    m_1_1

</pre>

### 20. master를 exp로 가져오기
<pre>
$ git merge master
Updating 973b5f9..4d4ee46
Fast-forward
 f1.txt | 2 +-
 f3.txt | 1 +
 2 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 f3.txt
</pre>

<div align="center">
    <img src="/img/git/git-branches(10).PNG"  width="50%" height="50%">
</div>

### 21. 로그 그래프/ 파일 확인하기
<pre>
$ git log --branches --decorate --graph --oneline
*   4d4ee46 (HEAD -> exp, master) Merge branch 'exp'
|\
| * 973b5f9 m_2_1
* | 6a37ea3 m_1_2
* | 0bc8693 m_3_1
|/
* 8bb2df2 m_1_1

$ ls
f1.txt	f2.txt	f3.txt
$ cat f1.txt
1_2
$ cat f2.txt
2_1
$ cat f3.txt
3_1
</pre>

### 22. exp branch 삭제하기
<pre>
$ git checkout master
Switched to branch 'master'

$ git branch -d exp
Deleted branch exp (was 4d4ee46).

$ git branch
* master

$ git log --branches --decorate --graph --oneline
*   4d4ee46 (HEAD -> master) Merge branch 'exp'
|\
| * 973b5f9 m_2_1
* | 6a37ea3 m_1_2
* | 0bc8693 m_3_1
|/
* 8bb2df2 m_1_1 
</pre>

<div align="center">
    <img src="/img/git/git-branches(11).PNG"  width="50%" height="50%">
</div>

### ※ git stash: git 디렉토리에서 수정한 파일들을 특정 장소에 임시 보관하기

A branch에서 작업 중인데, commit 할 준비가 되지 않은 상태에서 급하게 B branch로 넘어갔다 와야하는 상황이 있다.

stash는 작업중이라 커밋하기는 모호한 상황에서 다른 브랜치로 넘어갔다 와야할 때 현 소스 상태를 임시로 저장하는 명령어이다.

1) Modified면서 Tracked 상태인 파일
- 수정된 tracked 상태의 파일
- 수정된 과거에 commit이 되어 스냅샷에 넣어진 관리 대상 상태의 파일 
2) Staging area에 등록된 파일 

### stash 실습

### 0. git 초기화
<pre>
(master)$ git init 
Initialized empty Git repository in C:/Users/IT기획팀_01/Desktop/git_practice/.git/
</pre>

### 1. 파일 생성

f1.txt와 f2.txt를 만들고 둘다 commit 한다.
<pre>
(master)$ vim f1.txt
(master)$ vim f2.txt
(master)$ git add .
(master)$ git commit -m "f1.txt, f2.txt created"
[master (root-commit) 62c17bf] f1.txt, f2.txt created
 2 files changed, 2 insertions(+)
 create mode 100644 f1.txt
 create mode 100644 f2.txt
</pre>

### 2. exp branch를 만들고 이동한다. 
<pre>
(master)$ git checkout -b exp
Switched to a new branch 'exp'

(exp)$ git branch
* exp
  master
</pre>

### 3. 파일 수정하기

f1.txt와 f2.txt를 수정하고 f2.txt만 add 명령어를 통해 staging area에 올린다. 
<pre>
(exp)$ vim f1.txt
(exp)$ vim f2.txt
(exp)$ git add f2.txt
</pre>

### 4. git 상태 확인

f2.txt만 staged 되었음을 확인한다.
<pre>
(exp)$ git status
On branch exp
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   f2.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   f1.txt
</pre>

### 5. stash로 임시 저장하기

staged에 등록이 되어있는지 여부와는 상관없이, 지난 commit을 기반으로 수정사항과 함께 두 파일 모두 임시저장된다.
(exp)<pre>
$ git stash save
Saved working directory and index state WIP on exp: 62c17bf f1.txt, f2.txt created
</pre>

### 6. git status 확인

stash 후 working tree가 비워졌음을 확인한다.
<pre>
(exp)$ git status
On branch exp
nothing to commit, working tree clean
</pre>

### 7. stash list 확인

저장한 stash가 마지막 저장되었음을 확인한다. stash id는 stash@{0}이다.
<pre>
(exp)$ git stash list
stash@{0}: WIP on exp: 62c17bf f1.txt, f2.txt created
</pre>

### 8. master branch에서도 git status, stash list를 확인한다. 
<pre>
(master)$ git checkout master
Switched to branch 'master'

(master)$ git status
On branch master
nothing to commit, working tree clean

(master)$ git stash list
stash@{0}: WIP on exp: 62c17bf f1.txt, f2.txt created
</pre>

### 9. 다시 exp branch로 돌아와서 stash id를 가지고 저장된 stash를 불러온다. 

git status와 동일한 아웃풋이 나오는데, 이전에 f2.txt를 staging area 등록했음에도 불구하고 등록이 되지 않은 것으로 나온다. staging area에 등록되었던 상태까지 stash를 통해 불러오고 싶다면 옵션으로 --index를 붙이면 된다.
<pre>
(master)$ git checkout exp
Switched to branch 'exp'

(exp)$ git stash apply stash@{0} 
On branch exp
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   f1.txt
        modified:   f2.txt

no changes added to commit (use "git add" and/or "git commit -a")

</pre>
 
### 8. stash 삭제하기

<pre>
(exp)$ git stash drop stash@{0}
Dropped stash@{0} (55de9065faa7912fdba157b020bb29856c95993d)

(exp)$ git stash list

</pre>

### 9. stash에서 가져온 내용 커밋하기

<pre>
$ git status
On branch exp
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   f1.txt
        modified:   f2.txt

no changes added to commit (use "git add" and/or "git commit -a")

(exp)$ git add .

(exp)$ git commit -m "updated with stashed files"
[exp b007ade] updated with stashed files
 2 files changed, 2 insertions(+), 2 deletions(-)
</pre>

### 9. 이전 커밋으로 돌아가기(reset, revert)

reset

로그를 확인해보면 지금까지 두번의 commit이 된 것을 확인 할 수 있다.
<pre>
(exp)$ git log
commit b007ade0e1facfa5e042b4caf84d42a5b80279f5 (HEAD -> exp)
Author: young-jin-lee <dof0717@gmail.com>
Date:   Wed Jul 15 09:52:46 2020 +0900

    updated with stashed files

commit 62c17bf2decf7e2f85d80fb8596e35468f85566a (master)
Author: young-jin-lee <dof0717@gmail.com>
Date:   Fri Jul 10 17:13:34 2020 +0900

    f1.txt, f2.txt created

</pre>

첫번째 commit으로 돌아간다.
<pre>
(exp)$ git reset --hard 62c17bf2decf7e2f85d80fb8596e35468f85566a
HEAD is now at 62c17bf f1.txt, f2.txt created
</pre>

