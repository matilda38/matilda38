---
title: "efcore로 관계형 데이터베이스 설계하기"
date: 2017-04-23

---
# 1. 1:1 관계

*본 게시글은 [ef core documentation](https://docs.microsoft.com/en-us/ef/core/modeling/relationships)을 참고하여 만들어졌음을 알려드립니다.*

1:다 관계에서는 reference navigation을 가지는 개체가 종속(dependent)개체이고, collection으로(ICollection등) 가지고 있는 개체가 주(principal)개체임을 쉽게 알수있지만, 1:1관계의 경우 이를 쉽게 알 수 없기 때문에(두 모델 모두 서로의 id를 가진다.) foreign key를 설정할 때 종속 개체를 명시해주어야한다. 다음 예시를 보자.

{% highlight csharp %}
modelBuilder.Entity<Artwork>()
       .HasOne(a=> a.Auction)
       .WithOne(b=> b.Artwork)
       .HasForeignKey<Auction>(c => c.ArtworkId)
       .OnDelete(DeleteBehavior.Cascade);      
```
Artwork라는 개체를 만들었고 이 개체에 Auction이라는 테이블의 개체를 1:1 Matching시킨다. 이때 ForeignKey는 Auction의 ArtworkId로 한다. 즉 주(Principal)개체가 Artwork이고, 종속 개체가 Auction인 셈!

{% highlight csharp %}
```

{% highlight csharp %}
```
