---
layout: default
title: Google Search Console
description: "구글에서 검색되게 만들기"
parent: Development
nav_order: 1
---

# Google Search Console
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# 구글에서 검색되게 만들기

GitHub Pages 로 운영되는 블로그를 구글에서 검색되게 만들기 위해서는 [Google Search Console](https://search.google.com/search-console/about) 에 등록해야 한다. 이를 위해 필요한 것은 sitemap 과 robots.txt 이다(수정 커밋: [6ad4437](https://github.com/gwangwoopark/gwangwoopark.github.io/commit/6ad44376fa659deeac447710dd52e1be8fd380a0)).

## sitemap

sitemap(사이트맵)은 사이트에 대한 url 을 나열해놓은 xml 파일이다. 해당 파일은 [Sitemaps](https://en.wikipedia.org/wiki/Sitemaps) 프로토콜을 이용한다. sitemap 을 Google Search Console 에 등록함으로써 검색 엔진이 사이트를 더 효율적으로 크롤링할 수 있도록 한다. sitemap 의 위치는 robots.txt 에 명시한다.

- **Gemfile**
  ```bash
  gem 'jekyll-sitemap'
  ```

- **_config.yml**
  ```yaml
    # jekyll-sitemap 플러그인 추가
    plugins:
      - jekyll-sitemap
  ```

## robots.txt

robots.txt 는 로봇에 대해 해당 웹 사이트의 접근 제한에 대한 설명이 기술된 파일로 [robots exclusion standard(로봇 배제 표준)](https://en.wikipedia.org/wiki/Robots_exclusion_standard) 을 따른다. 간단히 설명하면 사이트의 어떤 범위를 크롤링할 수 있고, 어떤 범위는 할 수 없는지에 대해서 정의하여 검색 엔진이 자유롭게 해당 사이트를 인덱싱할 수 있도록 한다. 아래 파일은 모든 검색 엔진이 사이트 전체 내용을 크롤링 할 수 있도록 설정하였으며 sitemap 파일을 정의하였다.

- **robots.txt**
  ```
  User-agent: *
  Allow: /

  Sitemap: https://gwangwoopark.github.io/sitemap.xml
  ```


## Google Search Console

[Google Search Console](https://search.google.com/search-console/welcome) 에 속성을 추가한다. 먼저, URL 접두어 방식에 GitHub Pages 주소를 입력한다.

![Property](/assets/images/docs/development/search-console/SearchConsole-Property.png)

입력한 사이트가 자신의 것임을 증명하는 소유권 확인 과정이 필요하다. 제공하는 html 파일을 블로그 repository 의 루트 디렉터리에 저장한다(수정 커밋: [756ccf9](https://github.com/gwangwoopark/gwangwoopark.github.io/commit/756ccf955a2071c3657a693b8611d773d14896f0)). `git: add → commit → push`. 업로드 후에 확인을 누르면 컨펌 메시지를 확인할 수 있다.

![Ownership](/assets/images/docs/development/search-console/SearchConsole-Ownership.png)

![Ownership Confirmed](/assets/images/docs/development/search-console/SearchConsole-OwnershipConfirm.png)

속성으로 이동하여 색인 메뉴의 Sitemaps 에서 sitemap 을 등록한다. sitemap.xml 이 루트 디렉터리에 존재하기 때문에 sitemap.xml 이름을 적어주고 제출하면 된다.

![Sitemap](/assets/images/docs/development/search-console/SearchConsole-Sitemap.png)
