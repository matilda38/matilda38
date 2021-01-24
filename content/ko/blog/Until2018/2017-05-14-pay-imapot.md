---
title: "iamport!"
date: 2017-05-14

---
>ㅎ.ㅎ 3시간 돌려내^^
'''
안녕하세요, 아임포트 기술지원팀입니다.
주말에 좋지 않은 소식으로 연락드려 죄송합니다.
오늘 아임포트 REST API서버에 문제가 있어 API호출 시 500 오류가 간헐적으로 떨어지는 증상이 있었습니다.
(참고로, 결제 서버 및 관리자 페이지 서버는 문제없습니다)
발생 기간 : 2017-05-13 19:04:00 ~ 2017-05-13 22:18:00 ( 약 3시간 14분 )
로드밸런서에 연결된 여러 대의 API서버 중 1대가 오류를 발생시킴으로 인해, 해당 서버로 upstream연결되는 API요청들에 대해 500오류가 응답된 상황입니다.
'''
ㅠㅠㅠㅠㅠㅠㅠㅠㅠㅠㅠㅠㅠ


그지 같다!!

그래도 삽질끝에 결제 성공까진 어찌저찌 했으니 위안을 삼아야지...

[saved my life](https://elanderson.net/2017/04/entity-framework-core-with-sqlite/)
[sqlite studio](https://sqlitestudio.pl/index.rvt)

그리고, Cli tool을 visualstudio for Mac에서 사용하기 위해서는 csproj파일에 manual하게 아래 코드를 삽입해주어야한다는 사실을 약 2시간의 삽질 끝에 깨달았다.(github에 나같이 헤메고 있는애에게도 알려주었다 ㅋㅋ).쒯.ㅋ.ㅋ.쒯.ㅋ

'''
<ItemGroup>
 <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="1.0.0" />
</ItemGroup>
'''
