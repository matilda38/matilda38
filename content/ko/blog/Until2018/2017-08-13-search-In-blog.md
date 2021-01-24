---
title: "Blog에 검색기능추가하기"
date: 2017-08-13

---

# Blog에 검색 기능 추가하기

전부터 계속 시도했던 건데, 왜 인지 기억은 안나지만, 중간에 포기했었다. Jekyll 테마찾다가 우연히 검색기능을 다룬 블로그글을 보고 뽐뿌받고 어제 몇시간동안 삽질하다가 결국 처리했다. 최종발표 피피티는 도대체 언제만들까 ㅋㅋ..

일단 몇가지 Jekyll 블로그가 어떻게 generate되는 지 이해해야 될 것이 있었는데, 그걸 모르고 계속 삽질 하다보니 오래걸렸었다.
[stackoverflow](https://stackoverflow.com/questions/13266369/how-to-change-the-default-order-pages-in-jekyll/16625558)에서 관련내용을 보고 무릎을 탁 치고 한줄 탁 치니까 해결되었다. ㅜㅜ

뭐였나면,,,

### 

각 html이나 markdown파일마다 layout을 정의하는게 규칙인데, 일반적인 글을 올릴때는 post로 지정해둔다. layout 폴더로 들어가보면, post, default, page, category_index가 있는데, 이것들이 site.pages, site.posts들을 의미하는 거였다. site는 이 블로그 자체를 의미하고 layout이 page냐 post냐 에 따라 각각 site.pages site.posts들에 저장되고 있었다(Jekyll 엔진 자체적으로 처리). 내가 원했던 건 메뉴에 search를 추가하는 것이었다.

```ruby
 assign pages_list = site.pages | sort:"url"
 for node in pages_list
     if node.title != null
         if node.layout == "page"
        <li class="sb-close { if page.url == node.url }active{ endif }">
            <a href="{{ node.url | prepend: site.baseurl }}">{{ node.title }}</a>
        </li>
         endif
     endif
 endfor
```

메뉴가 있는 includes/sidebar.html에는 이런식으로 코드가 작성되어있었기 때문에, pages_list = site.pages는 layout이 page인 파일들을 의미하는 것이었다. 따라서 page_list에 search를 추가하여 메뉴를 추가해야했다. 이는 search.html에

```
---
layout: page
title: Search
permalink: /search/
---
```

이런식으로 추가해주면 jekyll에 의해 자동으로 search가 page로 인식되고 pages_list에 search도 들어간다.

아무튼 이제 메뉴에 search를 추가했으니, 실제로 검색을 위한 로직이 필요하다. 이는
[Simple-Jekyll-Search](https://github.com/christian-fei/Simple-Jekyll-Search)라는 JS 라이브러리를 사용했다. JS 라이브러리이기 때문에 NPM을 사용하여 설치할 수 있는데,

*npm으로 설치하면 ~.min.js만 설치되서 이것만 로드하면 될 줄 알았더니 안되더라....물론 내가 잘 못 설치한 걸수도 있음.*

결국 레포지토리에서 Simple-Jekyll-Search.js인가 이거 다운받아서 루트 폴더 밑에 dest 폴더 만들고 넣어줬다. search.html에서 그 파일을 로드해줬고. 이외에도 plugin에 루비파일 넣고 몇가지 과정이 자잘자잘하게 해주면 된다. 이 [블로그](http://www.halryang.net/simple-jekyll-search/)에서 많이 참조해서 만들어놨다. 아마 Json을 이용해서 검색을 수행하는 듯하다. 매우 쫗은 라이브러리 감사합니당당당~.
