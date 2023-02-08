# Git

## Clone과 Fork의 차이

### Clone한 저장소에서의 작업

1. remote 저장소 local로 복제
2. local 저장소에서 작업 진행
3. local 저장소에서의 변경사항을 remote 저장소로 바로 push

위에서 remote 저장소란 일반적으로 서버에 있는 저장소를 의미한다. 그리고 local 저장소란 말 그대로 git init, git clone 명령어로 생성한 local에 존재하는 저장소를 의미한다.

주의할 점은 마지막 과정처럼 local 저장소의 변경사항을 remote 저장소로 바로 push하기 위해서는 remote 저장소에 대해 write 권한이 있어야 한다는 점이다.

### Fork한 저장소에서의 작업

1. 원본 저장소 fork
2. fork한 remote 저장소를 local로 복제
3. local 저장소에서 작업 진행
4. local 저장소에서의 변경사항을 fork한 remote 저장소에 push
5. PR(pull request)을 통해 변경 사항을 원본 저장소에 merge하도록 요청 

fork를 하는 이유는 내가 작업을 진행하고 싶은 저장소에 대한 write 권한이 없기 때문이다. 위에서처럼 원본 저장소에 직접적으로 변경사항을 push할 수 없으므로 fork를 통해 본인의 계정에 write 권한이 있는 저장소를 만드는 것이다. 

만약 fork한 저장소를 독자적으로 수정해나가며 사용할 것이라면, fork 이후에 'Clone한 저장소에서의 작업' 과정을 따라가면 된다. 하지만 그보다는 원본 저장소에 변경 사항을 적용시키기 위해 fork를 사용하는 경우가 일반적일 것이다.

이를 위해서는 PR이 필요하다. PR은 저장소의 소유자에게 '내가 수정을 했는데, 이 수정 사항에 문제가 없다면 merge를 부탁할게' 라고 요청하는 것이다. 

___

정리하자면, Clone은 내가 작업하고자 하는 저장소의 write 권한이 있을 때, Fork는 write 권한이 없을 때 사용하는 방식이라고 할 수 있다.

# :books:참고자료

https://www.toolsqa.com/git/difference-between-git-clone-and-git-fork/

