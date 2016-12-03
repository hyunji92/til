#django Setting

>이때부터 지옥이 시작된다. 나는 나의 자괴감과 짜증을 극도로 끌어올린 초기 세팅을 나의 입맛에 맞게 
아주 친절히 쓸 예정

$ brew install pyenv -  install 을 해보세요

그리고
$ brew info pyenv - 해보세요

자 인제 Shell 이 아직 안될꺼에요. 왜냐면 path가 안잡혔거등요.

github 에 나오는 readme를 따라해 봅니다.
```text
Define environment variable PYENV_ROOT to point to the path where pyenv repo is cloned and add $PYENV_ROOT/bin to your $PATH for access to the pyenv command-line utility.

$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile

Zsh note: Modify your ~/.zshenv file instead of ~/.bash_profile.

저는 `~/.zshenv` 이걸로 해써여 ~~~~
```
그렇다면 어떻게 하냐면

1. $ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshenv - 이걸 그냥 쳐요.
저처럼 vim으로 들어가 수정하는 과오를 벌이지 마세요....

또 
2. $ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshenv

```text
Add pyenv init to your shell to enable shims and autocompletion. Please make sure eval "$(pyenv init -)" is placed toward the end of the shell configuration file since it manipulates PATH during the initialization.

```
3. $ echo 'eval "$(pyenv init -)"' >> ~/.zshenv  - 이걸로 또해여.. 

또 또 

4. $ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshenv - 이것을 하므로 마무리 해봅니다...

그렇다면 이제 path 가 됫으니 shell이 먹을 꺼에요 

```text
Restart your shell so the path changes take effect. You can now begin using pyenv.

$ exec $SHELL  -  이거 해봅니다.
```


설치가 된것 같으면 

$ zsh - '열어본다'고 하네요. zsh을 실행해봄니다.

그럼이제 파이썬 버전을 설치해볼까요 
$ pyenv install --list - 하면 설치가능한 많은 버전들이 나올꺼에요

$ pyenv install 3.5.2 - 이걸로 갑니다.

$ python --version - 으로 확인합니다.

그리고 

$ brew install pyenv-virtualenv - 가상환경 설치해요

$ zsh - 하고

이제는 warp할수 있음

$ pyenv virtualenv warp 

$ pyenv activate warp - 됨

그럼이제 (warp) 실행될꺼에요

$ pip install django - 장고 설치하세요.

밑에와 같은 모습을 볼 수 있어여 ..

```text
(warp) 
10:17:33  ✘  ~/warp  /warp   master ✔ 
$ ls 
CONTRIBUTING.md    dev.yml            manage.py          setup.cfg
CONTRIBUTORS.txt   docker-compose.yml package.json       utility
LICENSE            docs               presentation       warp
README.rst         ebsetenv.py        pytest.ini
compose            env.example        requirements
config             gulpfile.js        requirements.txt
```

또 설치할거있어요 .

$ pip install -r requirements/local.txt - 설치하세요.

$ python manage.py migrate - 하라고 나와용 이거해야 runserver 됨.

$ python manage.py runserver 

띠롱 이제 createsuperuser 등 만들어서 admin 만들고 쭉쭉하시면 됩니다

* 모델 바까서 db 변경사항 생기면

$ python manage.py makemigrations
migration 파일 만들어주면 

그리고 

$ python manage.py - migrate 이걸로 바뀐것 올려용


