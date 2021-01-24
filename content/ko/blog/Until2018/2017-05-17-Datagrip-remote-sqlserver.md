---
title: "Datagrip으로 원격 Azure sql server와 연결하기"
date: 2017-05-17

---

Datagrip은 MSsql server 뿐만 아니라, mysql, sqlite 등 여러 DBMS들과 연결할 수 있는 툴이다.

Jetbrain사에서 개발한 위 툴은, 학생라이센스로 무료로 사용할 수 있다.(유후~)

MsSql을 로컬에 설치할수 없었고(맥 OS이기 때문에), 클라우드로 Azure 에 있는 내 sql server에 접근해서 다루는 툴 같은경우에는 불편한 점이 많았기 때문에, 다른 dbms를 모두 사용할 수 있는 Datagrip을 설치하였다.

Datagrip을 설치 한 후,

new database -> sql server -> microsoft
로 진행하던중

host, database, connectionstring을 제대로 입력했음에도 불구하고, 계속 connect test가 실패하는 문제가 발생했다.
도데체 왜!! 라고 울부 짖던 중
나와 같은 Broken pipe 문제를 겪던 jetbrain 블로그 질문 글

[Broken Pipe](https://youtrack.jetbrains.com/issue/JRE-221)

을 발견했다. 그리고,,,

>
Good news - thanks to the detailed log files created with Vasily Chernov's logging config file, I was able to locate the source of this nagging issue:
If the hostname of the connecting client is not set or ends with .local, the connection will fail
To fix it, follow these steps:
 open terminal (/Applications/Utilities/Terminal)
 run scutil --get HostName (case sensitive!)
 if hostname is not set or contains .local, run sudo scutil --set HostName "newname"
 try JDBC Azure SQL Connection again - hostname change is effective immediately
Thanks Denis Fokin & Vasily Chernov for your help, this has been bugging me for quite some time

위와 같은 댓글을 발견하고,

![터미널 스크린샷](https://nayoonhwang.file.core.windows.net/image/terminal.png)
그것을 따라 이렇게 해보았다.

그랬더니,,,!

똭!

![성공](https://nayoonhwang.file.core.windows.net/image/terminal2.png)

이렇게 잘 되었다..헤헤

이 문제의 원인은!


>
If the hostname of the connecting client is not set or ends with .local, the connection will fail

연결 클라이언트 호스트 이름이 지정되어있지 않거나 ~.local 로 되어있으면 연결이 실패할 것이라고 한다.
그래서 위와 같은 명령어를 입력하여 hostname을 지정해주도록한다.
