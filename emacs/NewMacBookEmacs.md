# New MacBook Emacs  첫 사용기



> 호기심에 한번씩 사보고 번번히 때려친다는 emacs . 충동적인 맥북 로즈골드 구매를 계기로 이 맥북은 vim이나 emacs를 써서 간지 좀 내보자고 신년 계획을 새웠는데.. 잘될지는 모르겠지만, 일단 그약 내가 산다.

1. Mac 은 버전은 최신이 아닐지라고 emacs가 무려 기본내장;; 그렇지만 GUI가없기도 하고 일단 새로운 버전은 어떻게 설치하는지 찾아보고 설치 해보도록한다.

```text
우선 이맥스의 소스 코드를 내려받아야 한다. GNU 서버의 한국 미러는 카이스트에서 제공한다. 최신 버전을 찾아서 다운로드.
$ curl -O http://ftp.kaist.ac.kr/gnu/gnu/emacs/emacs-24.5.tar.gz

압축을 풀고, 디렉토리 안으로 들어가서 바로 빌드&설치를 시작하자.
$ ./configure --with-ns
$ make install

–with-ns 옵션은 NextStep 지원(아시다시피 NextStep은 OSX의 전신이다)을 의미하는데, 설치가 끝나면 소스 디렉토리 내 nextstep 디렉토리에 Emacs.app이 생긴 것을 확인할 수 있다.

이 Emacs.app 디렉토리를 파인더에서 찾아 응용프로그램(Applications) 디렉토리로 옮기면 설치 끝!
```

- 출저, 참고 블로그 - http://blog.nodance.net/1311/

2. 다른 블로그를 ( Emacsbook )보니까 emacs 리습 파일, 소스파일들은 다운로드 하는것 같은데 지금은 일단 그냥 건너 뛰어보겠다.  ( 이것은 바로 의식의 흐름 사용기 - )
3. 아 나는 젤리가 쓰는 이쁜 spacemacs 테마에 빠져 spaceamacs를 샀다. 그래서 emacs 순정 깔았다가 바뀜 ㅎㅎ
4. Emacs 단축키 규칙 

```shell
C-[char]
M-[char]
```

이렇단다. C는 컨트롤키이고 메타키가 M인데 메타키는 키보드의 Esc키와 Alt키를 의미한다고 한다.

5. 참고한 블로그에서는 터미널에서 `emacs -nw` 로 실행하시는군. emacs는 `C-x C-c`로 종료 가능;



# Emacs 용어

**버퍼**

기본적인 편집의 장소 ( 파일 - 버퍼 - emacs ) , emacs 가 편집할 수 있는 공간. 파일은 버퍼라는 형태로 emacs안에서 처리된다.

Emacs에서는 관행적으로 파일로 매핑되어 있지 않는 특별한 버퍼이름 앞에 *를 붙여 관리 한다.  `C-x b:버퍼변경`을 입력하려면,  `C-x b`

**윈도우**

버퍼의 출력. (버퍼 <-> 윈도우 <-> 사용자 ) 하나의 윈도우는 하나의 버퍼를 가질 수 있다.

# Spacemacs  (Vim mode)

spacemacs에서 파일이나 텍스트를 검색할때  -  `ctrl-j` to go down, `ctrl-k` to go up, and page up/down

Insert 모드 - `i` or `a` ( insert after the cursor )



