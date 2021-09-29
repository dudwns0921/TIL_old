# 멀티캠퍼스 Git 특강 1일차_Git 기본

- Git은 분산버전관리시스템 중 하나

- 2005년 리눅스 커널을 위한 도구로 리누스 토르발스가 개발

  > 당시 구글 개발자들에게 Git을 소개했는데, 개발자들 모두 "그게 된다고?" 이런 반응을 보였다고 한다.

  [소개 영상](https://www.youtube.com/watch?v=4XpnKHJAok8)

  

## 1. Git 저장소 만들기

```bash
$ git init
Initialized empty Git repository in C:/Users/User/Desktop/test/.git/
User@DESKTOP-THOHAFO MINGW64 ~/Desktop/test (master)
$
```

- .git 폴더가 생성되며, 버전이 관리되는 저장소
- git bash에서는 경로 옆의 ()안에 브랜치가 표기된다. 위의 브랜치는 master



## 2. 버전 만들기

버전을 만드는 작업을 큰 흐름으로 보자면

1. Working directory에서 작업
2. 작업한 파일을 add 명령어로 Staging Area에 모으기
3. commit 명령어로 버전 기록

이렇게 볼 수 있다.  각 명령어와 그 예시를 살펴보겠다.



### add

- Working Directory상의 변경 내용을 Staging Area에 추가하기 위해 사용

#### 사용방법

```bash
$ git add <파일/폴더/디렉토리>
$ git add . # 해당 디렉토리 전부
$ git add 파일명 # 특정 파일
$ git add folder/ # 특정 폴더
$ git add *.png # 특정 확장자
```

#### 예시

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.txt
nothing added to commit but untracked files present (use "git add" to track)
# 작업공간에서 작업한 파일들이 아직 Staging Area로 이동하지 않은 것이다.
User@DESKTOP-THOHAFO MINGW64 ~/Desktop/test (master)
$ git add .
# add 명령어를 통해 작업공간의 파일들을 Staging Area로 이동시킨다.

```



### commit

- Staging Area로 이동한, 즉 Staged 상태의 파일들을 버전으로 기록하기 위해 사용

#### 사용방법

```bash
$ git commit -m '<커밋메시지>'
# 커밋 메시지는 변경 사항을 나타낼 수 있도록 명확하게 작성해야 한다.
```

#### 예시

```bash
$ git commit -m "First Commit"
[master (root-commit) 7f9f854] First Commit
 1 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt
```



## 3. 상태관련 명령어

### status

- Git 저장소에 있는 파일의 상태를 확인하기 위해 사용

#### 사용방법

```bash
$ git status
```

#### 예시

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.txt
 # 아직 add하지 않았을 때의 status
 
User@DESKTOP-THOHAFO MINGW64 ~/Desktop/test (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   a.txt
 # add 후에 status
 
 User@DESKTOP-THOHAFO MINGW64 ~/Desktop/test (master)
$ git status
On branch master
nothing to commit, working tree clean
# commit을 한 후에 상태
 
```



### log

- 현재 저장소에 기록된 커밋을 조회
- 다양한 옵션을 통해 로그 조회 가능

#### 사용방법

```bash
$ git log
# 지금까지 저장소에 기록된 모든 커밋 조회
$ git log --oneline
# 저장소에 기록된 커밋들을 각각 한 줄로 간단하게 정리해서 보여준다.
$ git log -1
# 저장소에 기록된 커밋들 중 가장 최신의 커밋 1줄을 가져온다.
```

#### 예시

```bash
$ git log
fatal: your current branch 'master' does not have any commits yet
# 아직 커밋 기록이 없는 상태

User@DESKTOP-THOHAFO MINGW64 ~/Desktop/test (master)
$ git log
commit 7f9f85471ec96fd42a7237585850b1eaddddbc23 (HEAD -> master)
Author: Jun <dudwns0921@gmail.com>
Date:   Wed Sep 29 15:06:11 2021 +0900

    First Commit
    
User@DESKTOP-THOHAFO MINGW64 ~/Desktop/test (master)
$ git log --oneline
e758df8 (HEAD -> master) First Commit
# 커밋 기록을 한 줄로 정리해서 출력

User@DESKTOP-THOHAFO MINGW64 ~/Desktop/test (master)
$ git log -1
commit e758df86851dfdc387db6450f6c970181efee3f7 (HEAD -> master)
Author: Jun <dudwns0921@gmail.com>
Date:   Wed Sep 29 15:19:38 2021 +0900

    First Commit
# 가장 최신의 커밋 기록을 가져오는데, 현재 커밋 기록이 하나뿐이라 git log와 같은 결과가 나옴.



```

