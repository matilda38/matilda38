---
title: "퇴사."
date: 2017-06-01

---

5.31 최종적으로 바리엘을 퇴사했다. 3, 4, 5월 3개월동안 인턴 생활을 했고, Asp.net core 프레임워크를 다루며 c#으로 서버를 다뤄보는 경험을 할 수 있었다. Http Request Response 경험도 적었던 초짜라 결제모듈 API 사용이나, Facebook Login Api 사용등에서 많은 어려움을 겪었지만, 지금 다 끝내고 보니 정말 귀중하고 소중한 경험이었다고 생각한다.

# Asp.Net Core + Azure 로 개발하면, 서버 설정이 필요없다.

asp.net core 의 가장 큰 장점은 이것이라고 생각한다.(물론 c#에 익숙하지 않은 초짜의 입장이다)

서버 설정이 필요없다는 것은 Deploy 시에 클라우드의 가상 머신에 Nginx나 Passenger를 설치하여 배포 환경을 구축하지 않아도 된다는 의미이다. 닷넷 코어의 경우에는 IIS라는 마이크로소프트 자체 서버를 이용하는데, 당연히 Azure 에서는 해당 서버를 미리 다 구축해놓았다(마이크로 소프트 제품이니깐..). 덕분에 자신의 비쥬얼스튜디오 닷컴(나의 경우엔 nayoonhwang.visualstudio.com)에 프로젝트를 업로드 후 Publish만 누르면 된다. 비쥬얼스튜디오에서 작업 했을 시 작업하고 있는 프로젝트를 azure에 publish 하는 기능또한 제공하고 있다. 위와 같이 배포가 굉장히 간편하기 때문에 귀찮고 고된 서버 설정을 거치지 않아도 된다는 큰 장점을 가지고 있다. rails로 작업시에 했던 그 귀찮은 작업들을 안해도 된다니... 신세계였다.

# Facebook API 사용

기존에 Asp.net Core 하면서, Identity라는 모듈(?)을 사용했었다. Identity의 정의를 일단 찾아보면,

>ASP.NET Core Identity is a membership system which allows you to add login functionality to your application. Users can create an account and login with a user name and password or they can use an external login providers such as Facebook, Google, Microsoft Account, Twitter and more.2
You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data. Alternatively, you can use your own persistent store to store data in another persistent storage, such as Azure Table Storage.

라고 되어있다. 간단히 번역해보면, asp.net core identity는 당신의 application에 로그인 기능을 추가할 수 있게 하는 멤버쉽 시스템이다. 유저들은 기존의 로그인 방식으로 회원가입하고 로그인 할 수 도 있고, Facebook, Google, Twitter등 외부 로그인 provider들을 이용하여 로그인할 수도 있다. Identity를 Sql Server DB를 이용해 유저 이름, 비밀번호, 프로필 데이터를 저장하도록 설정할 수 있고, Azure Table Storage와 같은 다른 지속성있는 저장소에 저장할 수도 있다.

이 Identity 시스템을 적용하여 로그인을 구현했었다. Rails에서는 Devise Gem와 같은 역할인듯하다. 참고로 Rails에서 Gem으로 Package를 설치하는 것과 같이 Asp.net Core에서는 Nuget으로 패키지를 설치할 수 있다. Identity 패키지 설치 후 자유롭게 Identity Method들을 사용할 수 있다. 대표적으로 SignInAsync()이런 메소드들을 쓸 수 있다.

이야기가 삼천포로 빠졌지만, 아무튼 이렇게 로그인/회원가입 기능을 구현해놓았는데, 여기다 페이스북 로그인 기능을 넣게 되었다.

페이스북 로그인이라고 하면, 해당 사이트에 회원가입을 하지 않고 Facebook 회원정보만으로 이 사이트를 이용할 수 있는 것이다. 물론, 당연히 페이스북에서 인증 절차를 수행하고, 통과한 유저 정보를 받아온후 우리 Application DB에 해당 회원의 정보를 저장해야한다. 3rd party external login을 처음 이용할 시에는, **기존 유저 회원가입과는 다르게** 유저를 생성하고, 생성한 유저의 ID를 이용하여 유저의 로그인 정보를 AspNetUsersLogin에 테이블에 저장하게 되는데, 이 테이블 같은 경우엔, IdentityDbContext를 사용하면 자동으로 추가되는 테이블 중 하나다. 이 테이블에 Provider 정보, 여기서는 Facebook이므로 Facebook의 정보를 저장하고(이 정보는 Facebook에서 인증 절차가 완료되었을 때 App에 제공하는 id등이 포함되고 이는 providerkey 칼럼에 저장하게 된다) 추후 로그인 시에는 저장된 정보를 참조하여 authenticate 후 signin 처리를 하는 방식이다.

여기서 궁금했던 점은,

## Logic 적으로써 OAUTH 2.0의 동작 방식
*아래 그림은 몇 없는 한국어 그림*
![Oauth2](https://drive.google.com/uc?id=0B01HgT0PZhPsYUFzTHpTX0RvSlk)

## 3rd party 로그인 사용시, 해당 회원 정보 저장 문제.
였다.

위의 궁금점은 추후 다시 정리하도록 하고,
아래 궁금점같은 경우에는,

**일단 우리 어플리케이션의 경우에는 User를 비밀번호(또는 PasswordHash) 없이 생성하고, 생성한 User 정보를 AspNetUsersLogin에 로그인 정보를 저장할 때 활용하는 것으로 처리했다**

그런데 다른 어플리케이션들의 경우, 예를 들어 LinkedIn은 Facebook으로 로그인을 수행하더라도 내 페이스북 프로필의 이름과 메일주소가 회원가입 칸에 표시되고 새로 회원가입을 해야되는 형태였다. (도대체 이럴거면 왜 페이스북으로 로그인을 하는거죠?)

하지만 내가 생각한 External 3rd Party Login은 해당 사이트또는 App에 회원가입을 하고 싶지 않고, 기존 3rd Party의 아이디 비밀번호를 통해 로그인할 수 있어야 한다고 생각했다.

이런 방식은 해당 Application에 처음 로그인 하는 사람들을 우리 application에서 처럼 임의로 Create할 건지, **또는 일정 정보를 받은 후 Create**
할 건지에 따라 달라지는 것으로 보인다. LinkedIn에서 한 번 위 방식으로 회원가입을 수행하면, 로그인 시에 계속 페이스북 로그인 기능을 사용할 수 있는 걸로 보아, 해당 방식으로 User를 생성하고 저장 후, UserLogin테이블에 저장하여 로그인을 처리하는 듯하다.

![agile](https://nayoonhwang.file.core.windows.net/image/agile.png)
