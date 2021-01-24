---
title: "ruby setting 정리하기"
date: 2017-08-13

---
# RUBY 환경설정
ruby 자체를 Programming에 쓸 일은 별로 없지만, (요즘엔 Rails도 안쓰고 스크립트는 거의 Python으로 처리하므로,) 맥에 ruby를 깔아둬야 할 일은 꽤 자주 발생했다. 일단, Mac에서 많이 쓰는 Package Manager인 Homebrew도 ruby 기반이라 ruby가 설치 되어있어야하고, 지금 이 정적 블로그 엔진도 Jekyll이라는 ruby 기반 엔진이라, ruby가 없으면 로컬에서 돌려볼 수가 없었다. 컴퓨터를 바꾸기도 했었고 초기화로 메모리를 모두 날려먹은 적이 있어 기존에 설치해둔 ruby가 없어 설치하는 김에 이번 기회에 아예 게시글로 정리해두려고 한다.

알다시피 맥에는 기본 Ruby가 깔려있다. 초반에는 이 기본 Ruby로 어찌저찌 해결했는데, Gem을 깔아야되는 상황이 되니 기본 ruby를 건드렸다간 큰일이날 듯했다. 물론 권한도 없었다. 하지만 강제로 권한을 풀었다간 뭔가 큰 일이 날것같은 느낌. 어디인지는 모르겠지만 Mac에서 기본적으로 ruby를 가지고 있는 이유는 어딘가에서 ruby를 시스템용으로 사용하기 때문이다. 결국 이 기본 ruby는 사용하지 않고 막 쓸 별개 ruby를 깔기로 했다. 정확히 기억은 안나지만, python도 비슷한 이슈를 가지고 있어서 virtualenv라는 가상환경을 사용했던듯하다. ruby에도 이런 환경(?) 매니저를 가지고 있다. 프로젝트마다 필요한 루비 버전이 다를 수 있기 때문에 해당 프로젝트에서 사용할 루비를 지정할 수도 있고 시스템 전반에서 주로 사용할 ruby를 지정할 수도 있다.

Environment Manager for Ruby로 RVM과 rbenv 두 개가 있는데, 예전에 Rails를 2015년 하반기 처음 입문했을 때만해도 rvm을 많이 썼었다. 그때는 그냥 rbenv가 이름이 더 이뻐서 그걸 썼는데, 지금 다시 관련내용을 찾아보니, 거의 rbenv로 대세가 넘어왔다. 많은 게시물들이 rbenv를 다루고 있었다.

{% highlight bash%}
brew update
brew install ruby
```
Homebrew를 이용해서 Ruby를 설치하는것이 훨씬 편하다. 공식 페이지에서도 권장하고 있다. Homebrew가 없다면 밑에 링크에서 다운받도록 한다. Homebrew를 설치하는 데 필요한 루비는 맥 기본 루비이기때문에 루비를 따로 설치하지 않아도 그냥 Homebrew부터 깔면된다. Node, MySql등 여러 툴들을 사용할 때도 굉장히 유용하게 쓸 수 있기 때문에 맥 사용자라면, 깔아두는 것을 추천한다.
[Homebrew 설치하기](https://brew.sh/index_ko.html)

{% highlight bash%}
# If you use bash
echo 'export PATH=/usr/local/Cellar/ruby/2.4.1_1/bin:$PATH' >> ~/.bash_profile
# If you use ZSH:
echo 'export PATH=/usr/local/Cellar/ruby/2.4.1_1/bin:$PATH' >> ~/.zprofile
```
나는 Oh-my-zsh라는 zsh 쉘을 쓰고 있기 때문에 아랫줄을 실행해주었다. 쉘에 PATH를 설정해주는 작업이다. 2.4.1_1 이는 버전명으로 설치한 버전명을 넣도록 한다.

{% highlight bash%}
brew install rbenv ruby-build
# zsh
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zprofile
echo 'eval "$(rbenv init -)"' >> ~/.zprofile  

# list all available versions:
rbenv install -l

# install a Ruby version:
rbenv install 2.4.1

# set ruby version for a specific dir
rbenv local 2.4.1

# set ruby version globally
rbenv global 2.4.1

rbenv rehash
gem update --system

```
ruby를 깔았으니, 버전 관리 툴인 rbenv와 빌드 툴인 ruby-build를 brew를 통해 설치하도록 한다. 아까와 마찬가지로 .rbenv도 쉘 path에 등록해주고 쉘이 시작될때마다, rbenv가 init되도록 해준다.

설치가능한 버전들을 list해보고 원하는 버전을 설치한다. 설치후엔 **source ~./zprofile** 이 명령어를 한번 쳐주는 것도 좋다. 아까 환경변수 설정한것을 반영하도록 하는 명령어인데, 나의 경우엔 설치 후에 **rbenv global 2.4.1** 이 명령어가 먹히지않아서 **ruby -v** 했을 때 계속 옛날 루비 버전만 나왔었다. source 명령어 이후엔 잘 반영되었다. 어쩌면 터미널 껏다 켰음 그냥 됬을 수도 ㅋㅋ

rbenv rehash 이걸로 뭐였더라 무슨 변수 정리해주고, gem update를 해주면 <span style="color:#003366">ruby 설정완료.</span>

이제 2.4.1 버전 루비로 블로그에 Gem도 설치하고 jekyll serve도 해야겠다.

[참고](https://stackoverflow.com/questions/14607193/installing-gem-or-updating-rubygems-fails-with-permissions-error)
*echo $PATH* 로 환경변수를 볼때, 환경변수에 등록된 순서가 *~/.rbenv/shims* 가 ruby보다 늦게 나오면, 에러가 날 수 있다고 한다. 즉, rbenv가 먼저 load되어야한다는 말, 그때는 bash에 *export PATH=$HOME/.rbenv/shims:$PATH* 를 가장 마지막 줄에 넣으면 된다고 한다. 아마도 가장 마지막 줄의 path가 가장 먼저 load되기 때문인듯하다.
