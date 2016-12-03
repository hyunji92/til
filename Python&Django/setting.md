#django Setting

>이때부터 지옥이 시작된다. 나는 나의 자괴감과 짜증을 극도로 끌어올린 초기 세팅을 나의 입맛에 맞게 
아주 친절히 쓸 예정

$ brew install pyenv -  install 을 해보세요

그리고

$ brew info pyenv - 해보세요

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

(warp) 
10:17:33  ✘  ~/warp  🐍 warp   master ✔ 
$ ls 
CONTRIBUTING.md    dev.yml            manage.py          setup.cfg
CONTRIBUTORS.txt   docker-compose.yml package.json       utility
LICENSE            docs               presentation       warp
README.rst         ebsetenv.py        pytest.ini
compose            env.example        requirements
config             gulpfile.js        requirements.txt

또 설피할거있어요 .

$ pip install -r requirements/local.txt - 설치하세요.

$ python manage.py migrate - 하라고 나와용 이거해야 runserver 됨.

$ python manage.py runserver 

띠롱 이제 createsuperuser 등 만들어서 admin 만들고 쭉쭉하시면 됩니다

* 모델 바까서 db 변경사항 생기면

$ python manage.py makemigrations
migration 파일 만들어주면 

그리고 

$ python manage.py - migrate 이걸로 바뀐것 올려용


