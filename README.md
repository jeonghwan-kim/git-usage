# git-usage
자주 사용하는 깃 명령어 모음 


## 구조 

코드는 아래 세 단계에 걸쳐 저장된다.

스테이징 -> 커밋 -> 원격저장소

1. git add {파일명} 으로 파일을 스테이징 상태에 넣는다.
2. git commit 으로 스테이징 상태에 있는 모든 변경사항을 커밋한다. 여기까지가 로컬에서의 작업
3. git push 로 커밋된 저장소를 원격 저장소로 밀어넣는다.


## 기본 명령어

저장소 생성

```
git init
```

원격 저장소로부터 복제 

```
git clone {url}
```

변경 사항 체크

```
git status // 
```

특정 파일 스테이징

```
git add {파일명} 
```

변경된 모든 파일 스테이징

```
git add * 
```

커밋

```
git commit -m “{변경 내용}” 
```

원격으로 보내기

```
git push origin master 
```

원격저장소 추가

```
git remote add origin {원격서버주소} 
```

참고 페이지

- download(osx): http://code.google.com/p/git-osx-installer/downloads/list
- download(windows): http://git-scm.com/download/win
- 설치 메뉴얼: http://blog.outsider.ne.kr/389
- 사용 메뉴얼:http://dogfeet.github.io/articles/2012/how-to-github.html
- git 간편 안내서: http://rogerdudler.github.com/git-guide/index.ko.html
- 한장으로 핵심 기능만: http://rogerdudler.github.com/git-guide/files/git_cheat_sheet.pdf


## Commit

커밋 합치기

```
git rebase -i HEAD~4 // 최신 4개의 커밋을 하나로 합치기
```

커밋 메세지 수성

```
$ git commit --amend // 마지막 커밋메세지 수정(ref)
```

간단한 commit방법

```
$ git add {변경한 파일병}
$ git commit -m “{변경 내용}"
```

커밋 이력 확인

```
$ git log // 모든 커밋로그 확인
$ git log -3 // 최근 3개 커밋로그 확인
$ git log --pretty=oneline // 각 커밋을 한 줄로 표시
```

커밋 취소

```
$ git reset HEAD^ // 마지막 커밋 삭제
$ git reset --hard HEAD // 마지막 커밋 상태로 되돌림
$ git reset HEAD * // 스테이징을 언스테이징으로 변경, ref
```


## Branch

master 브랜치를 특정 커밋으로 옮기기

```
git checkout better_branch
git merge --strategy=ours master    # keep the content of this branch, but record a merge
git checkout master
git merge better_branch            # fast-forward master up to the merge
```

브랜치 목록

```
$ git branch // 로컬
$ git branch -r // 리모트 
$ git branch -a // 로컬, 리모트 포함된 모든 브랜치 보기
```

브랜치 생성

```
git branch new master // master -> new 브랜치 생성
git push origin new // new 브랜치를 리모트로 보내기
```

브랜치 삭제

```
git branch -D {삭제할 브랜치 명} // local
git push origin :{the_remote_branch} // remote
```

빈 브랜치 생성

```
$ git checkout --orphan {새로운 브랜치 명}
$ git commit -a // 커밋해야 새로운 브랜치 생성됨
$ git checkout -b new-branch // 브랜치 생성과 동시에 체크아웃
```

리모트 브랜치 가져오기

```
$ git checkout -t origin/{가져올 브랜치명} // ref
```

브랜치 이름 변경

```
$ git branch -m {new name} // ref
```


## Tag


태그 생성

```
git tag -a {tag name} -m {tag message} {commit hash}
git tag {tag name} {tag name} -f -m "{new message}" // Edit tag message
```

태그 삭제

```
git tag -d {tag name}
git push origin :tags/{tag name} // remote
```

태그 푸시

```
git push origin --tags
git push origin {tag name}
git push --tags
```


## 기타 

파일 삭제

```
git rm --cached --ignore-unmatch [삭제할 파일명]
```

히스토리 삭제

- 목적: 패스워드, 아이디 같은 비공개 정보가 담긴 파일을 실수로 올렸을 때 삭제하는 방법이다. (history에서도 해당 파일만 삭제)

```
$ git clone [url] # 소스 다운로드
$ cd [foler_name] # 해당 폴더 이동
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch [삭제할 파일명]' --prune-empty -- --all # 모든 히스토리에서 해당 파일 삭제
$ git push origin master --force # 서버로 전송
```

히스토리에서 폴더 삭제:

```
git filter-branch --tree-filter 'rm -rf vendor/gems' HEAD
```

리모트 주소 추가하여 로컬에 싱크하기

```
$ git remote add upstream {리모트 주소}
$ git pull upstream {브랜치명}
```

최적화

```
$ git gc
$ git gc --aggressive
```

## 서버 설정

강제 푸시 설정

```
git config receive.denynonfastforwards false
```

## Alias

~/.gitconfig 파일을 설정하여 깃 명령어의 앨리어스를 지정할 수 있다.

~/.gitconfig > alias 부분:

```

[alias]
  br = branch
  co = checkout
  rb = rebase
  st = status
  cm = commit
  pl = pull
  ps = push
  lg = log --graph --abbrev-commit --decorate --format=format:'%C(cyan)%h%C(reset) - %C(green)(%ar)%C(reset)  %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(yellow)%d%C(reset)' --all
  ad = add
  tg = tag
  df = diff 
```
