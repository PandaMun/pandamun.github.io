---
layout: post
title: Git 간단 사용법
subtitle: Git 간단 사용법
category: knowledge
---

Git는 프로그래머라면 자주 사용하는 시스템중 하나입니다.

간단하게 Git 사용법에 대하여 정리해 보겠습니다.

## Git 그리고 Git 호스팅서비스의 개념

Git은 컴퓨터 파일의 변경사항을 추적하고 여러명의 사용자들 간에 해당 파일들의 작업을 조율하기위한 분산 버전 관리 시스템입니다. 하지만 로컬 저장소를 사용하기 때문에 실시간으로 작업을 공유할수 없습니다. 그래서 Git 호스팅 서비스를 이용합니다.

그렇다면 Git 호스팅 서비스(Github, Bitbucket, GitLab)는 무엇이냐?

Git 호스팅 서비스은 Git을 사용하여 로컬에서 관리한 소스코드를 업로드할수 있는 원격 저장소 입니다.

## Git 사용법

- git init

    git 초기화


- git add <file name>

    파일 추가


- git add —all, git add *

    변경된 모든 파일 추가, 추가하지 않고 싶지 않은 파일을 추가시키지 않기 위해서 “.gitignore” 이름의 파일을 만들어 무시할 파일을 입력하면 됩니다.


- git status

    현재 상태 확인, 변경된 파일중 추가되지 않은 파일과 추가된 파일을 확인 가능합니다.


- git commit -m <message>

    관리할 파일을 add 시켰다면 커밋을 통해 상태를 저장합니다. 커밋 메시지로 어떠한 상태를 저장했는지 알수있기 때문에 자세하게 입력하면 좋습니다.


- git log

    커밋 로그 확인


### Branch

개발을 할때 기존 개발하던 부분과 다른부분을 개발을 해야하는 상황이 있습니다.

리팩토링(가독성을 높이고 유지보수를 편하게 하기위하여 결과의 변경없이 코드의 구조을 재조정)중에서 새로운 기능을 추가하는게 어렵기 때문에 독립적으로 개발을 진행하기 위하여 branch을 만들어 서로 다른작업을 진행할수 있습니다.

서로의 branch는 변경이 일어나도 영향을 주지 않습니다.

- git branch

    branch 생성

    git branch <branch name>


- git checkout

    branch 변경

    git checkout <branch name>     (add 2.23 version)

    - git switch <branch name> : checkout이라는 명령어가 담당하는 부분이 꽤 많기 때문에 switch 명령어로 대체 되었습니다. branch 변경부분만 담당한다.


- git branch

    branch 확인


- git branch -d <branch name>

    branch 삭제


- git merge <branch name>

    branch 병합 (* 가장 오류가 많이 나는 지점 *)

    한 branch가 작업이 끝났을때 다른 브랜치에 개발 상황을 적용시키는데 사용합니다.

- 예시

    현재 <main> branch에 작업이 끝난 <develop> branch을 병합 할려고 합니다.


    (main 브랜치 상태) git merge develop

    위 명령어를 입력하게 되면 <main> branch에 <develop> branch의 변경된 내용이 합해진 형태가 됩니다.

    여기서 한 파일이 양쪽의 branch에서 모두 변경 되었다면 충돌이 발생하기 때문에 한 branch의 변경사항만을 선택 하여야합니다. 보통 충돌된 부분이 확인 되기 때문에 수정하고 git add <file name>를 하여 커밋하면 됩니다.

- rebase

    branch 병합의 다른 방법으로써 실행결과는 같지만 지저분한 커밋 히스토리를 깔끔 하게 정리할수 있습니다. (잘 모르고 사용한다면 까다롭고 위험함)

- git reset

    commit 취소합니다.

- git restore        (add 2.23 version)
    - git restore <file name>                         [git checkout — <file name>]

        특정 파일 HEAD commit(가장 최신 commit)으로 복구하기

    - git restore —staged <file name>          [git reset HEAD <file name>]

        Staging Area에 올라간 파일 다시 Unstaging 시키기


### GitHub와 연결하기

- git remote

    로컬 저장소와 원격 저장소를 연결합니다.

    git remote add <저장소 이름> <저장소 URL>

- git push <원격 저장소 이름> <보낼 브랜치>

- git pull

    원격 저장소에 있는 변경 사항을 로컬 저장소로 업데이트 받기

    pull 하기전 로컬에서 작업하던 내용을 pull 할경우 충돌이 일어날수 있으므로 fetch을 사용하여 확인후 merge를 사용하것이 좋습니다.


- git fetch

    원격 저장소의 최신이력을 확인 가능

    단순히 원격 저장소의 내용을 확인만 하고 로컬 데이터와의 병합을 하고 싶지 않을때 사용합니다.

    fetch를 한후 FETCH_HEAD 브랜치를 merge 한다면 pull과 같은 효과를 냅니다.


- git clone

    원격저장소를 복제해 로컬로 가져옵니다.
