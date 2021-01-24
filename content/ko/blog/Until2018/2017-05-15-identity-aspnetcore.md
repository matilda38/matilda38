---
title: "identity 모듈을 aspnetcore에 적용하기.!"
date: 2017-05-15

---
aspnet core application에

회원정보 관리를 모두 담당할 수 있는(=귀찮고 어려운 세션관리등을 모두 담당해줄 수 있는) identity 모듈을 탑재해보자.

일단 공식 다큐멘테이션.
[identity를 aspnetcore에 적용하는 documentation](https://docs.microsoft.com/ko-kr/aspnet/core/migration/identity)

요약해보자면.

1. Package Console 에 Microsoft.AspNetCore.Identity.EntityFrameworkCore 패키지를 추가해준다.
2. 기존에 회원관리를 위해 만들었던 모델. 나같은 경우엔 Account 였다. Account 에 IdentityUser를 상속해준다.
*IdentityUser에서 Id , UserName, Email등 필수 field들이 이미 선언되어있기 때문에 따로 Account에서 선언해줄 필요는 없다.*

3. startup.cs에 위와 같이 등록해준다.
```c#
//add identity module
services.AddIdentity<Account, IdentityRole>()
    .AddEntityFrameworkStores<DatabaseContext>()
    .AddDefaultTokenProviders();

services.Configure<IdentityOptions>(options =>
{
    options.Password.RequiredLength = 8;
    options.Password.RequireNonAlphanumeric = false;
    options.Password.RequireUppercase = false;
    var scheme = options.Cookies.ApplicationCookieAuthenticationScheme;
});
```
물론 아래 configure는 option이기 때문에 원하는 대로 지정할 수 있다. 그리고 Account같은 경우에는 회원관리 모델을 의미하므로 적절히 사용하도록 하자.

마지막으로 configure 함수에
```c#
app.UseIdentity();
```
를 추가해준다.
4. DatabaseContext도 dbcontext를 상속하는 것이 아닌,     

```c#
public class DatabaseContext : IdentityDbContext<Account>
```
위와 같이 IdentityDbContext<Account>를 상속해주도록 하자.

위와 같이 되면 일단
```c#
public DbSet<Account> Accounts { get; set; }
```
와 같은 건 필요없다. 해당 IdentityDbContext에서 *Users<Account> AspNetUsers 라고 따로 테이블이 형성되기 때문.*

이제 설정은 끝났고, Identity Module의 기능을 이용하기만 하면 된다.

```c#
private readonly UserManager<Account> _userManager;

private readonly SignInManager<Account> _signInManager;

public AuthController(UserManager<Account> userManager, SignInManager<Account> signInManager)
{
    _userManager = userManager;
    _signInManager = signInManager;
}

// GET: /Auth/Register
[HttpGet]
[AllowAnonymous]
public IActionResult Join(string returnUrl = null)
{
	ViewData["ReturnUrl"] = returnUrl;
	return View();
}

[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Join(JoinViewModel model){
  if (ModelState.IsValid)
	{
            var user = new Account { UserName = model.Email, Name= "연습중" , Email = model.Email, Phone= "010-010-010"};
		var result = await _userManager.CreateAsync(user, model.Password);
		if (result.Succeeded)
		{
			await _signInManager.SignInAsync(user, isPersistent: false);
			return RedirectToAction(nameof(HomeController.Index), "Home");
		}
		AddErrors(result);
	}
	// If we got this far, something failed, redisplay form
	return View(model);

}

    // GET: /Auth/Login
[HttpGet]
[AllowAnonymous]
public IActionResult Login(string returnUrl = null)
{
	ViewData["ReturnUrl"] = returnUrl;
	return View();
}

    [HttpPost]
    [AllowAnonymous]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null){
        ViewData["ReturnUrl"] = returnUrl;
        if(ModelState.IsValid){
            var result = await _signInManager.PasswordSignInAsync(model.Email, model.Password, true, lockoutOnFailure: false);
            if(result.Succeeded)
            {
                return RedirectToLocal(returnUrl);   
            }
            else
            {
                ModelState.AddModelError(string.Empty, "invalid attempt");
                return View(model);
            }
        }
        return View(model);
    }

[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> LogOut()
{
	await _signInManager.SignOutAsync();

	return RedirectToAction(nameof(HomeController.Index), "Home");
}

#region Helpers

private void AddErrors(IdentityResult result)
{
	foreach (var error in result.Errors)
	{
		ModelState.AddModelError(string.Empty, error.Description);
	}
}

    private Task<Account> GetCurrentUserAsync()
{
	return _userManager.GetUserAsync(HttpContext.User);
}

private IActionResult RedirectToLocal(string returnUrl)
{
	if (Url.IsLocalUrl(returnUrl))
	{
		return Redirect(returnUrl);
	}
	else
	{
		return RedirectToAction(nameof(HomeController.Index), "Home");
	}
}

#endregion

{% endhighlight%}

예제코드를 참고해본 회원관리 컨트롤러이다.
