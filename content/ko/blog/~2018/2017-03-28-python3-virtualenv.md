---
title: "python 가상환경에서 사용해보기."
date: 2017-03-27

---

[캡처 자동 업로드하기](https://beomi.github.io/2017/03/27/Use-GoogleDrive-as-Image-Server/)포스팅을 보다가 삘받아서 시작한 파이썬 가상환경 구축.

[가상환경에서 python3 사용해보기](http://www.marinamele.com/2014/07/install-python3-on-mac-os-x-and-use-virtualenv-and-virtualenvwrapper.html)

python 2, 3

기존에는 /library/framework.python/~ 폴더에 파이썬이 들어가있었어서, (아마 그렇게 해야됬었던 설정이 있었겠지. bash_profile에 그렇게 저장하도록 지정되어있었다.)

virtualenvwrapper를 쓸수 없는 문제가 있었다.

결국 기존에 설치되어있던 python3.5를 지우고 Homebrew를 통해 python3를 다시 설치했다.

virtualenvwrapper를 사용하기 위해, pip(package 설치 관리자)를 이용하여

{% highlight csharp %}
$ pip install virtualenv
$ pip install virtualenvwrapper
```

로 두가지를 설치하니 의도했던 /usr/local/bin에 잘 깔렸다.

가상환경들을 담을 폴더를 만들고
{%highlight csharp %}
$ mkdir ~/.virtualenvs
```

.bashrc를 열어(없으면 만들고)
{%highlight csharp %}
export WORKON_HOME=~/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```
를 추가해준다. /usr/local/bin/에 virtualenvwrapper.sh가 설치 되어있으므로 저런식으로 해준것.

{%highlight csharp %}
source .bashrc
```
를 통해 .bashrc를 활성화해준다.

{%highlight csharp %}
  mkvirtualenv
  //가상환경 만들기
  deactivate
  //빠져나오기
  workon myenv
  //다시 활성화하기
```
이렇게 하게 되면 각 가상환경마다 다른 패키지를 사용할 수 있으므로 독립된 가상환경이 구축된것이다.
