---
title: "getch, getchar, getche c 언어 입력에 대해서."
date: 2017-05-02

---

엠티 가는 날. 1학년 학생이 과제때문에 엠티에 못가고 있다하여 c언어 과제를 도와주었다.

우리의 미션은 주사위 게임 만들기. 교수님이 정한 규칙을 준수하는 주사위 게임을 만들어야했다.

[getch()를 써야되는 이유](http://kcoder.tistory.com/entry/getchar-getch-getche%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%98%88%EC%A0%9C%EC%86%8C%EC%8A%A4-%EA%B7%B8%EB%A6%BC)

getch(), getchar() 이것들 때문에 별짓 다했네

getch()랑 getche()는 버퍼를 사용하지 않기 때문에,

getchar()와 같이 엔터("\n")값이 들어올때 까지 기다리지 않는다.

예를 들어 k값을 입력하면 입력하자마자 getch()는 누르자마자 처리한다.

버퍼를 사용하는 경우, 입력을 하면 바로 들어가는 것이 아니라, 입력버퍼라는 곳에 담기게 된다.

[getchar()를 써야되는 이유](http://stackoverflow.com/questions/10720821/im-trying-to-understand-getchar-eof)
