---
title: "asp.net study"
date: 2017-03-06
---

ASP.net Core 학습중이다

[내 비쥬얼 스튜디오 온라인 팀프로젝트](https://nayoonhwang.visualstudio.com/MySecondProject/_dashboards)

ASP.net Core 공식 다큐먼테이션을 참고하여 설치하였고

Editor로 VS code를 사용한다. (VS studio for Mac을 설치하여 주말동안 시도해보았으나 삽질하고 그냥 실패, 시간만 날렸다.)

![이미지](https://nayoonhwang.blob.core.windows.net/newcontainer/pic2.png)
VSCode에서 Git은 이렇게 사용하면 된다.
initalize 하고, 체크 버튼/ cmd+ enter로 commit 완료.

*본 이미지는 azure 저장소의 blob 을 이용. 컨테이너 설정을 개인이 아니고 blob으로 설정해주면 public access 가능.

터미널에 remote add origin URL 입력해주고 push 해주면 끝.

이때 비쥬얼 스튜디오 온라인 팀프로젝트를 사용하고 싶으면 온라인에서 프로젝트 new 해주고 나오는 URL을 써주면 된다(Github와 같다).

오늘은 기본적인 CRUD를 구현해보았는데,

먼저 게시글을 관리할 Post Controller가 필요하였기에 다음과 같이 만들었다.

{%highlight csharp %}
public IActionResult Create()
{
      if(HttpContext.Session.GetInt32("AccountId")!= 0)
            return View();
      else
            return RedirectToAction("Login", "Auth");
}
public IActionResult Create(string title, string content){
        Post new_post = new Post();
        new_post.Title = title;
        new_post.Content = content;
        var db = new DatabaseContext();
        db.Posts.Add(new_post);
        db.SaveChanges();
        return RedirectToAction("Detail", new{id = new_post.Id});
}
{%endhighlight%}


*dotnet ef 에 관련한 정보.*<br />
**migration file 추가 (If it’s the first one, it will add the neccessary folder structure and classes)**<br />
<span style="color:#003366">dotnet ef migrations add {MigrationName}</span><br />
**최근 migration 삭제**<br />
<span style="color:#003366">dotnet ef migrations remove</span><br />
**최신버전으로 database update (apply all migrations)**<br />
<span style="color:#003366">dotnet ef database update</span>

dotnet ef database update <previous-migration-name>
을 사용하면 특정 migration 으로 복귀 가능한듯하다.

Create Action같은 경우에는 post로 지정해주면 자동으로 <input> tag를 통해 입력해준 parameter 값들이 전달된다.

{% highlight html %}

<form method="POST">
    <p>게시글 작성</p>
    <p>제목: <input name= "title" type = "text"></p>
    <p>내용:<input name= "content" type = "text"></p>
    <input type= "submit" value = "Submit">
</form>

{% endhighlight%}

```c#

    public IActionResult Create()
    {
            if(HttpContext.Session.GetInt32("AccountId")!= null)
                return View();
            else
                return RedirectToAction("Login", "Auth");
    }

    //Post method를 위해 post 어트리뷰트 붙여주기
    [HttpPost]

    public IActionResult Create(string title, string content)
    {
        Post newPost = new Post();
        newPost.Title = title;
        newPost.Content = content;

        var db = new DatabaseContext();
        db.Posts.Add(newPost);
        db.SaveChanges();

        return RedirectToAction("Detail", new{id = newPost.Id});
    }

{%endhighlight%}

[HttpPost]를 이용해서 Post Method을 사용하는 Action임을 알리자.  HttpPostAttribute의 준말.

기본 Default인 Get방식을 사용하는 Create()와 구분가능.

모호한 함수지정 에러를 피할 수 있다. (AmbiguousActionException: Multiple actions matched.)

Create Action으로 Post 객체를 생성했으니, Detail로 이동할 차례,

<span style="color:#003366">return RedirectToAction("Detail", new{id = newPost.Id});</span>

으로 이동해줄텐데, 동일 컨트롤러 내의 Detail 액션으로 이동할 때, 익명 객체를 생성하여 파라미터로 함께 보내준다. new{} 안에 id 객체를 넣을 건데 이는 생성된 post의 id를 넣어 보낸다.

이렇게 보내진 id는

```c#

public IActionResult Detail(int id)
{
    var db = new DatabaseContext();
    var post = db.Posts.Find(id);

    return View(post);
}

{%endhighlight%}

기본적으로 controller / action / id로 routing 이 진행된다.

```c#
    app.UseMvc(routes =>
        {
            routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
        });
{%endhighlight%}

따라서,

{%highlight html %}
//Post 객체가 나올 것이라고 예고하기 위해 @model을 썼다.
//@는 c#코드를 html파일에 쓰기 위해 사용하는 기호
@model Post

//c# 코드가 1줄동안 이어지기 때문에 중괄호를 사용해주었다 띄어쓰기가 포함되어있는 c#코드는 {}가 필요하다.
@{Post post= Model;}
<h1>게시글 @post.Id </h1>

<div>
<p>제목: @post.Title</p>
<p>내용: @post.Content</p>
</div>
{%endhighlight%}

여기서 @Model의 Model은 넘겨진 View(post)의 파라미터 Post객체를 의미하겠지.
물론 Detail.cshtml파일에서 Model 을 다루기 위해서는

_ViewImports.cshtml 파일에
@using WebApplication.Models
을 삽입해줘야한다.


마지막 정리는 Session 문제

Login, Join 문제를 위해!

using Microsoft.AspNetCore.Http; 일단 이거 import 해주고,


```c#
{%endhighlight%}

```c#
{%endhighlight%}

```c#
{%endhighlight%}

```c#
{%endhighlight%}

nullable에 대하여 => model 간 관계가 optional일때는 nullable 객체가 필요하다.

즉,

```c#

  //related exhibition OPTIONAL
  public long? ExhibitionId { get; set; }
	public Exhibition RelatedExhibition { get; set; }

{%endhighlight%}

이런식이 되는 것인데, RelatedExhibition은 exhibition 객체이다. 즉. class 객체일 경우 reference 타입(주소값 타입)이라 ?라는 nullable 표시가 필요없다. 하지만 ExhibitionId의 경우 value 타입(값 타입)이기 때문에 빌 수 있다(여기서는 연관된 exhibition을 가지지 않을 수 있다라는 뜻)는 nullable 표시 ?을 붙혀주어야한다. 그래서 long? 타입으로 선언해 준것이다.
