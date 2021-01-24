---
title: "Docker로 서버 배포 도전"
date: 2017-08-28

---

# azure deploy!(Nginx+Passenger)
애저 deploy 게시글을 쓴지 1년이 지나있다. 작년 8월 23일 작성한 게시글이었는데, 1년동안 많이 성장했다는 생각이 들면서도 많이는 나아가지 못한 듯하여 아쉽다. 제대로 이해했다고 생각했는데, 제대로 이해하지 않고 넘어간 부분이 많았고(Nginx가 어떤 역할을 하는 줄은 알고 썼던 거니...?), 지금 생각해보면 후회되는 시간들이 많았던듯하다. 빨리 가을이 왔으면 좋겠고, 에너지가 생겨서 더 열심히 살 수 있었으면 좋겠다.

# docker로 서버 배포하기

1년 전에 작성한 그 게시글을 docker로 대체해보고자 한다. 가상컴퓨터에 docker를 올려서 db용, web application server 용 두 개의 docker를 올려서 배포를 시도해보려고 한다. 해당 시도를 위해 몇 가지 정말 좋은 튜토리얼을 찾았다.

[raccoonyy님의 블로그](http://raccoonyy.github.io/docker-for-ghost-blogging/index.html)
[subicura님의 블로그](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)

갓개발자! 님들이 정말 자세한 튜토리얼을 올려주셨다. 대체로 위 튜토리얼을 따라해보는 형태로 진행해보았다.

나의 경우에는 비교적 간단하니
docker-compose 기능을 통해 web server, db server를 docker-compose.yml 파일에 정의하면 될 것으로 예상된다.

## vm 접속, docker 설치
기본 credit으로 vm을 만들었다. 컴퓨터를 바꿔서 그런지, 아니면 1년 전과 달리 애저 포탈이 업그레이드 된 건지, 모든 과정이 훨씬 순조롭게 진행되었다. 그냥 공개 키 넣고 만든 후에 *ssh username@public_ip* 이렇게 하니깐 바로 접속되서 꽤 놀랐다.

접속한 vm에서
```bash
curl -s https://get.docker.com/ | sudo sh
```
docker는 root 권한인 sudo가 필요하기 때문에, 새로운 비밀번호를 설정하기 위해 sudo passwd로 비밀번호를 설정해준 후 root 권한으로 명령어를 수행한다.

설치 완료는
```bash
docker version
```
명령어로 확인 할 수 있고, Client, Server 두 가지 모두 나와야 정상적으로 설치된 것이다. 도커 자체적으로 사용자로부터 커맨드를 받아 Server에 전달하는 Client, 실제 일을 수행하여 결과를 client에게 전달하는 docker Server로 나눠져 있다고 볼 수 있기 때문이다.

| Option | Description |
| --- | ----- |
| -d | detached mode, 백그라운드 모드 |
| -p | 호스트와 컨테이너의 포트를 연결(포트 포워딩), 9000:9000 |
| -v | 호스트의 볼륨을 컨테이너와 공유하여 사용(마운트) |
| -e | 컨테이너에서 사용할 환경변수를 설정한다 |
| -name | 컨테이너의 이름을 설정한다 |
| -rm | 프로세스 종료 시에 컨테이너를 자동으로 제거한다 |
| -it | -i, -t 옵션을 동시에 준것으로 터미널 입력을 위한 옵션 |
| -link | 컨테이너들을 연결한다 |
{:.mbtablestyle}


```bash
Docker -link ~ ~ -p 12201:12201/udp -p 5555:5555
//should publish in multiple ports to listen udp/tcp, default tcp -> it is port binding between docker and host

docker create -i -t -v ~/projects:/root/workspace -w /root --name centos7_project -e TZ=Asia/Seoul centos

docker start -I centos7_project
```
맨 위 명령어는 MongoDb, ElasticSearch, GraylogServer 세개의 컨테이너를 한번에 열었어야 했을 때 사용했던 명령어였다. 두개의 포트를 열고자 했고 하나는 udp, 하나는 tcp로 열고자 했다. 이렇게 /udp를 붙여주지 않으면 기본으로 tcp 포트로 생성된다.

두번째 커맨드는 centos라는 컨테이너를 생성하는 커맨드인데, -it 옵션에, -v 옵션을 통해 host의 projects폴더를 마운트하여 사용하였다. -w는 컨테이너의 작업 디렉토리를 root로 하겠다는 것이고 컨테이너 이름을 centos7_project로 할것이며 centos 이미지 위에 writable container layer을 만들것임을 의미한다. 컨테이너를 만들었으니 아래 커맨드는 만든 컨테이너를 시작하는 커맨드이다.

## 컨테이너 실행해보기
```bash
docker run ubuntu:16.04
```
run 커맨드를 통해 ubuntu:16.04 이미지가 저장되어 있는지 확인하고, 없다면 Pull을 통해 다운로드 하여 컨테이너를 Create한 후 Start한다. 즉, run 커맨드는 여러 커맨드들이 합쳐져 있는 커맨드라고 할 수 있다.

References)
[docker 공식 documentaion](https://docs.docker.com/engine/reference/commandline/create/#initialize-volumes)
[jekyll에서 표 그리는 법](https://stackoverflow.com/questions/28806135/jekyll-kramdown-how-to-display-table-border)
