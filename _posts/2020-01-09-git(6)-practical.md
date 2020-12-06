---
layout: post
title: "practical"
comments: true
categories: git
---

## <u><b> Version Control System - git(6) </b></u>

### ◆ Git 실습 매뉴얼 목차

#### 1. [기초](./git(1)-basics.md)
#### 2. [branch 활용법](./git(2)-branches.md)
#### 3. [원격저장소(Remote Repository) 활용법](./git(3)-remote.md)
#### 4. [원격저장소와 통신 방식](./git(4)-protocol.md)
#### 5. [병합 충돌 및 해결](./git(5)-merge.md)
#### 6. [중요 기능](./git(6)-practical.md)
#### 7. <a href="./img/git_sts.pdf" download>Spring Tool Suite(IDE)에서 git 사용법</a>

<br/>

#### 형상 관리를 효과적으로 하기 위한 추가적인 기능이다. 개념적인 내용만 숙지하고 활용법은 구글을 참고한다.

### tag

- commit을 참조하려면 commit id를 활용해야했다. commit을 참조하기 쉽도록 이름을 붙이는 것이 태그이다. 
- tag 이름을 활용하여 checkout 하거나 reset하여 특정 commit으로 돌아갈 수도 있다. 

1) 일반태그(Lightweight tag): 이름
2) 주석태그(Annotated tag): 이름, 설명, 서명, 만든이 정보 등

### pull vs fetch

- pull은 fetch + merge 이다.
- fetch를 실행하면 원격저장소의 내용을 로컬로 가져오지만, merge는 하지 않는다.
- 원격 저장소의 내용은 FETCH_HEAD라는 임시 브랜치에 들어가 있으며, merge를 하고 싶으면 따로 merge 하거나, 원격저장소의 내용을 pull 한다.

### commit 수정하기

1. 특정 commit의 파일/내용 업데이트, commit 메세지 수정하고 싶을 때: commit --amend 
2. 과거 특정 commit 삭제하고 싶을 때: revert
※ rebase -i 나 reset을 사용은 위험하다. revert를 통해 특정 commit을 삭제하는 새로운 commmit을 만들어 안전하게 처리할 수 있다.
3. 특정 commit을 삭제하고 과거 commit으로 돌아가고 싶을 때: reset
※ 명령어 옵션을 통해 HEAD위치, 인덱스, 작업트리 내용을 선택적으로 되돌릴 수 있음.
   - <b>commit</b>만 되돌리고 싶을 때: soft
   - 변경한 <b>인덱스의 상태</b>까지 되돌리고 싶을 때: mixed
   - <b>최신 커밋을 버리고</b> 이전의 상태로 돌리고 싶을 때: hard
4. 다른 branch의 특정 commit을 현재 branch로 가져오고 싶을 때: cherry-pick
5. 특정 브랜치의 모든 commit을 병합하고 싶을 때: merge --squash


### merge vs rebase

- 두 개(이상)의 branch를 merge하고 log를 확인해보면 두 개의 갈래(branch)로 나누어 졌다가 합쳐지는 것을 확인할 수 있다.
- 병합할 때 하나의 줄기로 만들고 싶을 때 rebase를 활용하여 merge하면 된다.

