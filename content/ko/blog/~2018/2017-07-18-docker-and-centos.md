---
title: "docker 이미지로 centos pull"
date: 2017-07-18

---

## docker 사용하기

도커는 굉장히 편하다. 일단 일반 vm처럼 네트워크설정을 할필요도 없고(host, 즉 그냥 내 컴퓨터의 resource들을 사용하므로) 가벼우므로, 완전한 가상머신을 제공하는 VMWARE나 VirtualBox보다 부담이 적다.

```bash
docker pull ubuntu:latest
{% endhighlight%}
docker pull <이미지>:<태그>로 간편하게 이미지를 받을 수 있다. 이를 pull이라고한다. 도커는 docker hub라는, image repository가 있기 때문에 docker hub에서 전세계 다양한 사용자들이 만든 이미지를 다운 받아 사용할 수 있다. centOs, Ubuntu, fedora등 대부분의 linux 운영체제는 공식 이미지를 사용하기 때문에 docker hub를 통하지 않고 위와 같이 다운로드할 수 있다.

터미널에서 *docker search ~* 명령어를 이용하면 이미지를 찾아볼 수도 있다. ex) docker search golang으로 검색하면 go 와 관련된 도커 이미지들이 나오고 star도 나오므로 유명한 이미지가 우선적으로 나온다

```bash
docker run -i -t [container_name] /bin/bash
//새롭게 컨테이너 만들 때는 아래와 같이 /bin/bash를 실행하도록 지정한다
docker exec -i -t [container_name] bash
//컨테이너에 직접접속
{% endhighlight%}

아까 설치한 이미지를 컨테이너로 생성한후, 배시셸을 실행하는 명령어이다.
--name은 해도되고 안해도 되는 option으로 이름을 지정하고 싶다면 해당 옵션을 주면된다. 안주게되면 manual하게 도커가 이름을 준다. (나름 다양하다)
-it 옵션을 통해 bash 셸에 입출력이 가능하게 된다.
```bash
docker ps
{% endhighlight%}
로 현재 실행되고 있는 컨테이너를 출력할 수 있다. container ID를 얻을수있다. (-a 옵션을 추가하면 실행되고 있지는 않지만 존재하는 container까지 출력할 수 있다) docker stop, restart 등으로 컨테이너를 중지, 재시작 할 수 있다.

중단한 container라고 하더라도, start로 다시 살려서 attach로 다시 접속할 수 있다.

```bash
docker login
docker commit -m "added golang" -a "centos with golang latest" affectionate_hugle nayoonhwang/centos_with_go
docker push nayoonhwang/centos_with_go

//docker hub에 올라가있는 centos_with_go 이미지를 다운받아 사용할 수 있다
docker pull nayoonhwang/centos_with_go
{% endhighlight%}
터미널로 내 docker계정에 로그인하여, 현재 작업하고 있던 컨테이너(affectionate_hugle)를 docker repository로 push할 수 있다. 나의 경우에는 go lang을 설치해놓은 centos를 원격저장소에 올려놓았다.
내가 참조한 링크: [How to create image and push it to docker hub](http://www.techrepublic.com/article/how-to-create-a-docker-image-and-push-it-to-docker-hub/)
