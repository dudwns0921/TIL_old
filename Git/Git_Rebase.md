# rebase

merge와 같이 브랜치 통합을 위해 사용하는 rebase 명령어에 대해 알아보자.

아래 그림과 같은 작업이 이루어졌다고 가정해보겠다.

![브랜치](https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup1_4_6.png)

우선 'bugfix' 브랜치를 'master' 브랜치에 rebase 하면, 'bugfix' 브랜치의 이력이 'master' 브랜치 뒤로 이동하게 된다. 그 때문에 그림과 같이 이력이 하나의 줄기로 이어지게 된다.

이 때 이동하는 커밋 X와 Y 내에 포함되는 내용이 'master'의 커밋된 버전들과 충돌하는 부분이 생길 수 있다. 그 때는 각각의 커밋에서 발생한 충돌 내용을 수정할 필요가 있다.

![rebase를 사용하여 브랜치 통합](https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup1_4_8.png)

'rebase'만 하면 아래 그림에서와 같이, 'master'의 위치는 그대로 유지된다. 'master' 브랜치의 위치를 변경하기 위해서는 'master' 브랜치에서 'bugfix' 브랜치를 fast-foward 병합 하면 된다.

![rebase를 사용하여 브랜치 통합](https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup1_4_9.png)

## :bulb:Tip

merge 와 rebase 는 통합 브랜치에 토픽 브랜치를 통합하고자 하는 목적은 같으나, 그 특징은 약간 다르다.

- **merge**  
  변경 내용의 이력이 모두 그대로 남아 있기 때문에 이력이 복잡해짐.
- **rebase**  
  이력은 단순해지지만, 원래의 커밋 이력이 변경됨. 정확한 이력을 남겨야 할 필요가 있을 경우에는 사용하면 안됨.

## Rebase 실습

토픽 브랜치에서 작업 후 통합 브랜치에 불러올 때

위 내용을 실제로 실습해보도록 하겠다.

```bash
$  git log --branches --graph --oneline
* 0328e1b (master) commit D
* 90097aa commit C
| * 5159913 (HEAD -> bugfix) commit Y
| * d1078a6 commit X
|/
* 10e09a7 commit B
* 2b58a2a commit A
* 8668f5c first commit
# 우선 위의 그림처럼 작업을 진행시켰다. B커밋에서 bugfix 브랜치로 분기해
# 두 번의 커밋을 더 진행했고, master 브랜치에서도 마찬가지로 2번의 커밋을 더 진행했다.

$ git rebase master
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
error: could not apply d1078a6... commit Y
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 5159913... commit Y
# rebase 명령을 실행하자 같은 파일에서 작업된 부분이 있어 충돌이 일어났다.
# 충돌이 일어난 부분을 수정해주고 add 명령어를 실행했다.

$ git add .

$ git rebase --continue
[detached HEAD f142f12] commit F
 1 file changed, 1 insertion(+), 1 deletion(-)
Successfully rebased and updated refs/heads/bugfix.
# 충돌 후 add로 변경 사항을 staging area에 올려준 다음에
# rebase --continue 명령어를 사용해준다.

$ git log --branches --graph --oneline
* f142f12 (HEAD -> bugfix) commit Y
* 8dcfc2e commit X
* 0328e1b (master) commit D
* 90097aa commit C
* 10e09a7 commit B
* 2b58a2a commit A
* 8668f5c first commit

# merge와 달리 이력이 한 줄로 깔끔하게 정리된 것을 확인할 수 있다.
# 하지만 master의 HEAD가 bugfix 브랜치에서 진행한 2번의 커밋 전에 있으므로
# master에서 bugfix 브랜치를 병합해준다.

$ git log --graph --oneline
* f142f12 (HEAD -> master, bugfix) commit F
* 8dcfc2e commit E
* 0328e1b commit D
* 90097aa commit C
* 10e09a7 commit B
* 2b58a2a commit A
* 8668f5c first commit

# 마지막으로 필요없어진 bugfix 브랜치를 삭제해준다.

$ git branch -d bugfix
Deleted branch bugfix (was f142f12).

$ git log --graph --oneline
* f142f12 (HEAD -> master) commit F
* 8dcfc2e commit E
* 0328e1b commit D
* 90097aa commit C
* 10e09a7 commit B
* 2b58a2a commit A
* 8668f5c first commit
```

# :books:참고자료

[브랜치 통합하기 【브랜치 (Branch)】 | 누구나 쉽게 이해할 수 있는 Git 입문~버전 관리를 완벽하게 이용해보자~ | Backlog](https://backlog.com/git-tutorial/kr/stepup/stepup1_4.html)
