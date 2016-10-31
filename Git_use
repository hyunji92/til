> <git add 후 취소>

git rm --cached 파일이름


> <modified file 표시 안하기/취소하기>

git checkout -- <filename>

 
> <update>

git pull origin master 


> <branch / tag 목록 다운받기 >

git fetch
 

> <마지막 commit 보기>

git show


> <commit 정보 확인>

git log


> <tag 보기>

git tag –l


> <모든 branch 보기 >

git branch -a 또는 git branch -r 또는 git remote show origin 또는 git ls-remote --heads origin

 

> <만들었던 local branch 삭제>

git branch -D [help]

 

> <프로젝트 생성후 처음 push할 때> 
git push -v --all origin
 

> <커밋하기> 
git commit -a -m "bug fixed" 
git push sv

 

> <모르고 파일 삭제/추가했을때, 원복하기> 
git reset --hard HEAD 
git pull

** commit 은 이미 해버렸을 때.

git reset HEAD^ 

 

> <target 디렉토리가 git 소스에 commit되어 있어서 날리기> 
git rm -r --cached <your directory>

.gitignore 파일을 이용해서 정리할 필요있음


> <rollback 하기> 
git log 
   (commit 넘버 확인) 
git reset --hard (commit 넘버)


<두 사람이 작업하다가 conflict 날 것 같으면, 작업하던 것을 임시저장(stash) 함> 
git stash save "work in progress" 
git pull 
git stash apply 
git commit -m '11'

 

> <rebase – merge 가 좀 더 좋아짐>
git rebase 
- merge될 대상 branch를 다운받기 git checkout .. 
- git rebase merge할 소스

 

> <crlf 적용>

git config core.autocrlf false


 

> <devolop으로부터 branch 하나 따기> 
git checkout -b feature-old-deletion master
(소스 수정) 
git commit -a -m "Rearrange old api" 
git push origin : feature-old-deletion



> <소스 검색>

git grep string 

grep보다 빠르다. 



> <submodule>

git submodule update --init --recursive

