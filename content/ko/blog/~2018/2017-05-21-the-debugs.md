---
title: "디버깅 ㅠㅠ"
date: 2017-05-21

---

# Debugs

1. 500 error 해결
서버오류인 500 error, 즉 API 호출은 정상적으로 이루어졌다는 뜻이다.
여러가지로 찾아보다가, 디버깅 결과 어이없게도

{% highlight csharp %}
if (payment == null || checkout == null)
{
  throw new Exception("Invalid Operation. Null Parameters");
}
```

여기서 500 error가 발생하는 것을 깨달았다.(디버깅의 실생활화가 절실합니다.)
그리고 정말 어이없게도 정말 서버단의 문제였다.(api 회사 죄송합니다..)

>*_database.Checkouts.SingleOrDefaultAsync(c => c.imp_uid == imp_uid)*
요 줄이 문제였는데, 서버단에서 생성한 모델인 checkout에 아임포트 아이디가 있을리가 만무, 따라서 호출 결과 null이 호출되고 위 에러가 발생한 것이다.  
그래서, *_database.Checkouts.SingleOrDefaultAsync(c => c.merchant_uid == payment.TransactionId)* 위와 같이 고쳐줬다.

2. Parameter가 잘 안들어가는 문제.

js로 location.href를 통해 /checkout/ShowResult/{merchant_uid_value}를 날려주었더니,
디버깅 결과. Controller 상에서 merchant_uid를 받지 못하고 null로 표시되는 것을 확인할 수 있었다.
이를 해결하기 위해 *[Route("Checkout/ShowResult/{merchant_uid}")]* 를 추가해주었더니 파라미터로 잘 들어간다.

{% highlight csharp %}
[HttpGet]
[Route("Checkout/ShowResult/{merchant_uid}")]
public async Task<IActionResult> ShowResult(string merchant_uid){
    return View(await *_checkoutservice.ShowResultAsync(merchant_uid)*);
}

```

3. highlight 태그가 자꾸 안닫혔다고 하는 지킬 블로그.
[Tags are not closed](http://blog.slaks.net/2013-08-09/jekyll-tag-was-never-closed/)

excerpt_separator 때문이라고 하는데, 이 에러에 대해 잘은 모르겠다. 이게 맞아야 valid한 tag라나 뭐라나, 그래서
config.yml에

> excerpt_separator: ""

를 추가해주면 해결된다. 이를 추가하면 excerpt_separator가 조기에 종료되어 추가 검사를 하지 않는 효과를 본다고 한다.
