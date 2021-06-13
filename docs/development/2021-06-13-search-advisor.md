---
layout: default
title: Naver Search Advisor
description: "네이버에서 검색되게 만들기"
parent: Development
nav_order: 2
---

# Google Search Console
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# 네이버에서 검색되게 만들기

GitHub Pages 로 운영되는 블로그를 네이버에서 검색되게 만들기 위해서는 [Naver Search Advisor](https://searchadvisor.naver.com/) 에 등록해야 한다. 이를 위해 추가로 Rss feed 를 생성해야 한다(수정 커밋: []()).

## RSS feed 생성

Jekyll 을 사용하면서 RSS feed 를 생성하는 방법으로는 plugin 을 사용하는 방법과 생성을 도와주는 파일을 직접 작성하는 두가지 방법이 있다. 처음에는 plugin 을 사용하려고 했으나, 네이버에 입력할 때 계속해서 오류가 발생해서 직접 작성하는 방법을 사용하였다. [Jekyll Codex](https://jekyllcodex.org/without-plugin/rss-feed/)를 참고하였다.

- **feed.xml**

  [샘플 코드](https://raw.githubusercontent.com/jhvanderschee/jekyllcodex/gh-pages/feed.xml) 를 다운받고, 루트 폴더에 저장한다.
  ```xml
    {% raw %}---
    layout: 
    ---

    <?xml version="1.0" encoding="UTF-8"?>
    <rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
      <channel>
        <title>{{ site.title | xml_escape }}</title>
        <description>{{ site.description | xml_escape }}</description>
        <link>{{ site.url }}{{ site.baseurl }}/</link>
        <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
        <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
        <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
        <generator>Jekyll v{{ jekyll.version }}</generator>
        {% for post in site.posts limit:10 %}
          <item>
            <title>{{ post.title | xml_escape }}</title>
            <description>{{ post.content | xml_escape }}</description>
            <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
            <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
            <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
            {% for tag in post.tags %}
            <category>{{ tag | xml_escape }}</category>
            {% endfor %}
            {% for cat in post.categories %}
            <category>{{ cat | xml_escape }}</category>
            {% endfor %}
          </item>
        {% endfor %}
      </channel>
    </rss>{% endraw %}
  ```

- **_includes/head.html**
  
  head.html (사용하는 Jekyll 테마에 따라 위치가 다를 수 있다) 에 아래 코드를 추가한다.
  ```html
    {% raw %}<link rel="alternate" type="application/rss+xml" href="{{ site.url }}/feed.xml">{% endraw %}
  ```

## 네이밍 규칙

이제 feed.xml 을 확인할 수 있다(docker를 사용하는 경우 http://0.0.0.0:4000/feed.xml). 하지만, feed.xml 에 `{% raw %}<item>{% endraw %}` 항목이 하나도 없는 경우가 있을 수 있다. feed.xml 에 문서가 등록되기 위해서는 특정 네이밍 규칙을 따라야 하는 것으로 보인다([jekyllrb.com](https://jekyllrb.com/docs/posts/)).

```
YEAR-MONTH-DAY-title.MARKUP
```

아래 예제와 같은 형태로 `/docs/` 아래에 있는 문서들의 이름을 수정해주면 feed.xml 에 문서들이 등록되는 것을 확인할 수 있다.

```
2011-12-31-new-years-eve-is-awesome.md
2012-09-12-how-to-write-a-blog.md
```

## Naver Search Advisor

[Naver Search Advisor](https://searchadvisor.naver.com/console/board) 에 사이트를 등록한다.

![Register](/assets/images/docs/Development/search-advisor/SearchAdvisor-Register.png)

구글과 마찬가지로 입력한 사이트가 자신의 것임을 증명하는 소유권 확인 과정이 필요하다. 제공하는 html 파일을 블로그 repository 의 루트 디렉터리에 저장한다(수정 커밋: [dff802f](https://github.com/gwangwoopark/gwangwoopark.github.io/commit/dff802f57b9e239fa2d8c8ee2299d3764a40fe33)). `git: add → commit → push`. 업로드 후에 소유확인을 누르면 컨펌 메시지를 확인할 수 있다.

![Ownership](/assets/images/docs/Development/search-advisor/SearchAdvisor-Ownership.png)

등록한 사이트에 대하여 RSS 제출을 한다.

![RSS](/assets/images/docs/Development/search-advisor/SearchAdvisor-RSS.png)

구글과 마찬가지로 사이트맵 제출까지 완료한다.

![Sitemap](/assets/images/docs/Development/search-advisor/SearchAdvisor-Sitemap.png)
