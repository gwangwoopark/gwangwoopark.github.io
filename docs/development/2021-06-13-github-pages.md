---
layout: default
title: GitHub Pages & Jekyll
description: "GitHub Pages 로 블로그(Blog) 운영하기"
parent: Development
nav_order: 1
---

# GitHub Pages & Jekyll
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# GitHub Pages 로 블로그(Blog) 운영하기

GitHub Pages 를 사용하여 github.io 도메인을 사용하는 블로그를 운영할 수 있다. 블로그를 위한 respository 를 생성하고 add, commit, push 등의 git 명령어를 사용하여 작성 하듯이 블로그를 업데이트한다. 참고로 Public Repository만 무료로 Pages 를 운영할 수 있다.

## GitHub Repository

자신의 계정에 블로그 운영을 위한 repository 를 생성한다. repository 이름에 따라 GitHub Pages 주소(즉, 블로그 주소)가 결정된다. 참고로 repository 이름을 `계정이름.github.io` 로 하면 ,  root 주소를 사용할 수 있다.

- repository 이름이 blog 인 경우,

  ```
  https://github.com/gwangwoopark/blog → https://gwangwoopark.github.io/blog
  ```

- repository 이름이 `계정이름.github.io` 인 경우 (해당 방법 사용하여 진행),

  ```
  [https://github.com/gwangwoopark/gwangwoopark.github.io](https://github.com/gwangwoopark/gwangwoopark) → [https://gwangwoopark.github.io](https://gwangwoopark.github.io)
  ```

## Jekyll

Jekyll을 이용하면 마크다운(Markdown), HTML, CSS 파일로 구성된 정적 웹사이트를 만들 수 있다. 해당 파일을 GitHub Repository에 Push하면 GitHub에서 소스 파일을 컴파일하여 정적 페이지를 생성한 후에 GitHub Pages로 호스팅해준다. 이외의 Jekyll에 관한 설명은 [여기](http://jekyllrb-ko.github.io/)를 참조.

## Just the Docs

Jekyll을 사용하는 편한 방법은 이미 만들어진 테마를 하나 골라서 사용하는 것이다. [Free Jekyll Themes](https://jekyllthemes.io/free)와 같은 곳을 참고하면, 다양한 무료 테마를 확인할 수 있다. 이 블로그는 [Just The Docs](https://pmarsceill.github.io/just-the-docs/) 테마를 사용하였다. 해당 테마의 링크를 통해 이동해보면 안내 페이지 또한 Jekyll을 이용하여 GitHub Pages로 호스팅되고 있다.

### Clone

[pmarsceill/just-the-docs](https://github.com/pmarsceill/just-the-docs) 를 Fork 하여 사용하는 방법도 있지만, Pull Request 를 해당 Repository 로 해버리는 실수를 하는 경우가 있어서 Clone 을 사용하였다.

```bash
$ git clone https://github.com/pmarsceill/just-the-docs.git blog
```

### Origin 변경

클론 받은 위치에서 remote 명령으로 확인해보면 원작자의 주소가 origin 으로 잡혀있는 것을 확인할 수 있다. 이를 자신의 repository 주소로 변경한다.

```bash
$ git remote -v
origin  https://github.com/pmarsceill/just-the-docs.git (fetch)
origin  https://github.com/pmarsceill/just-the-docs.git (push)

$ git remote set-url origin https://github.com/gwangwoopark/gwangwoopark.github.io

$ git remote -v
origin  https://github.com/gwangwoopark/gwangwoopark.github.io (fetch)
origin  https://github.com/gwangwoopark/gwangwoopark.github.io (push)
```

### First Commit & Push

현재까지 내용을 repository 에 반영한다.

```bash
$ git commit -m "first commit"
$ git push
...
To https://github.com/gwangwoopark/gwangwoopark.github.io
 * [new branch]      master -> master
```

### Settings for GitHub Pages

```
GitHub Repository → Settings → Pages
```

Pages 를 사용하도록 브랜치와 폴더를 선택할 수 있다. repository 이름이 `계정이름.github.io` 이 아닌 경우에는 Source 의 브랜치를 None → master 로 변경하고 폴더는 / (root) 를 선택하여 Save 한다. repository 이름이 `계정이름.github.io` 인 경우에는 Pages 가 자동으로 동작 중일 것이다.

### Settings for My Repository

위의 과정을 모두 완료하면 `https://계정이름.github.io` 로 접속할 수 있다. 하지만, 기존 just-the-docs 테마의 몇가지 설정으로 인하여 제대로된 화면이 나오지 않으므로 해당 내용을 수정한다(수정 커밋: [ae8a9b6](https://github.com/gwangwoopark/gwangwoopark.github.io/commit/ae8a9b652dff4c8bdce598330ac6fbdbe4dccddb)).

- **.github/workflows/ci-master.yml**
  ```yaml
  # jobs 중 jekyll-latest 를 삭제하고, bundler 를 2.1.4 버전으로 설정

  jekyll/builder:3.8.5 /bin/bash -c "gem install bundler && chmod -R 777 /srv/jekyll && bundle install && bundle exec jekyll build && bundle exec rake search:init"
  → jekyll/builder:3.8.5 /bin/bash -c "gem install bundler:2.1.4 && chmod -R 777 /srv/jekyll && bundle _2.1.4_ install && bundle exec jekyll build && bundle exec rake 
  ```

- **_config.yml**
  ```yaml
  # url 을 나의 GitHub Pages 주소로 변경
  baseurl: "/just-the-docs" # the subpath of your site, e.g. /blog
  → baseurl: "" # the subpath of your site, e.g. /blog
  url: "https://pmarsceill.github.io" # the base hostname & protocol for your site, e.g. http://example.com
  → url: "https://gwangwoopark.github.io" # the base hostname & protocol for your site, e.g. http://example.com
  ```

- **Dockerfile**
  ```bash
  # bundler 를 2.1.4 버전으로 설정
  RUN gem install bundler && bundle install
  → RUN gem install bundler:2.1.4 && bundle _2.1.4_ install
  ```

### Run a local server

```bash
$ docker-compose up
Starting blog_jekyll_1 ... done
Attaching to blog_jekyll_1
jekyll_1  | Configuration file: /usr/src/app/_config.yml
jekyll_1  |             Source: /usr/src/app
jekyll_1  |        Destination: /usr/src/app/_site
jekyll_1  |  Incremental build: disabled. Enable with --incremental
jekyll_1  |       Generating... 
jekyll_1  |                     done in 4.52 seconds.
jekyll_1  |  Auto-regeneration: enabled for '/usr/src/app'
jekyll_1  |     Server address: http://0.0.0.0:4000/
jekyll_1  |   Server running... press ctrl-c to stop.
```

로컬 서버를 동작하기 위해서는 Ruby & Jekyll 을 설치해야 하지만, 도커 컨테이너를 이용하면 개발 환경에 신경쓰지 않아도 된다. 로컬 폴더가 Volume 으로 공유되기 때문에, 파일이 수정되면 컨테이너에서 즉시 컴파일 하여 결과를 반영한다. 로컬 서버의 URL은 `http://0.0.0.0:4000/` 이다.

컴파일된 페이지는 `/_site/`에 생성되며 해당 폴더는 `.gitignore` 에 등록되어 있다. 즉, 컴파일된 페이지는 로컬에서 테스트용으로 사용될 뿐, GitHub 에 반영되지는 않는다.

### Config

전반적인 설정(e.g., 블로그 타이틀, favicon 등)은 `_config.yml`을 수정하면 된다(수정 커밋: [4ecbe51](https://github.com/gwangwoopark/gwangwoopark.github.io/commit/4ecbe51a6e8679edd3405605b3e05824b754ee78)). 자세한 설명은 [Just the Docs - Configuration](https://pmarsceill.github.io/just-the-docs/docs/configuration/)을 읽어보는 것이 좋다.

### Edit

최종적으로 화면에 보이는 것은 `/docs/` 에 있는 마크다운 문서들이다. 해당 폴더에 파일을 추가하거나, 기존 파일을 수정하면서 블로그를 계속해서 업데이트 할 수 있다. 나의 경우에는 기존 파일들을 모두 다른 폴더로 옮기고 새로운 index.md 를 루트 폴더에 추가하였다(수정 커밋: [eea8be6](https://github.com/gwangwoopark/gwangwoopark.github.io/commit/eea8be63fe55edff932cfe586add4c01049f7fc3)).
