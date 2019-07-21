---
title: "Github Pages로 첫 블로그 포스팅하기"
date: 2019-03-25 14:18:44 -0400
categories: jekyll update
---

> Github Pages 서비스를 이용해 개인 블로그 만들기


어느덧 소프트웨어 공부를 시작한지 3개월, 슬슬 학습 기록의 필요성을 느끼고 블로그 개설을 결심했습니다.

> [GitHub Pages](https://pages.github.com/) is designed to host your personal, organization, or project pages from a GitHub repository.

Github Pages는 Github에서 제공하는 블로그 및 개인 웹페이지를 관리할 수 있는 서비스이며, 자체 url 경로를 설정하여 페이지를 공유할 수 있습니다.



#### Pages 장점

타 블로그 서비스는 어떤 기능을 제공하는지 모르겠습니다만... Pages는

1. Pages 역시 repository이기에 포스트를 입맛대로 편집 가능
2. Markdown 기본 제공으로 md문법에 익숙해지면 포스팅 용이
3. 오픈소스 Ruby Jekyll Theme으로 쉽게 웹페이지 편집 가능

정도의 장점이 있는 것 같습니다. 



### Repository 만들기

Pages는 깃허브 repository로 관리되기 때문에 먼저 repository를 만듭니다.

repository 이름이 곧 pages의 sub-URL 경로가 될 것이기에 semantic하게 만드는 것을 추천합니다.

- `blog`라 이름지으면 http://yomandawg.github.io/blog/ 처럼 sub-URL 주소가 생깁니다.

만약 이름을 `*.github.io`로 설정할 경우 `github.io` 도메인으로 웹페이지가 개설됩니다.

- 저같은 경우엔 `yomandawg.github.io`라고 짓고 https://yomandawg.github.io/라는 주소가 자동으로 생성되었습니다.




### 테마 설정하기

그 후 Pages의 source를 제공해야 페이지가 생성되는데, repository가 비어있으면 기능이 지원되지 않습니다.

빈 repository를 채워주기 위해 블로그 테마를 설정해줍니다.

먼저 [Jekyll Theme](https://github.com/topics/jekyll-theme)에서 마음에 드는 테마를 찾습니다.

![2](https://user-images.githubusercontent.com/45955154/61590773-d11ba600-abf8-11e9-93cd-e63a2611f9ea.PNG)

수많은 테마가 있지만 저는 다양한 색상과 깔끔함으로 많이들 사용하는 *mmistakes*를 채택했습니다.



설치엔 다음과 같은 세가지 방법이 있습니다.

1. [gem-based theme](https://jekyllrb.com/docs/themes/#understanding-gem-based-themes)
2. [remote theme](https://blog.github.com/2017-11-29-use-any-theme-with-github-pages/)
3. forking



저는 가장 간편한 remote 방식을 통해 페이지를 세팅하겠습니다.

테마 repository의 `_config.yml`을 내 repository로 복사해옵니다.

![3](https://user-images.githubusercontent.com/45955154/61590775-d4169680-abf8-11e9-96ea-c1c3d232da65.PNG)

파일 내용을 다음과 같이 모두 복사합니다.

![4](https://user-images.githubusercontent.com/45955154/61590780-d7118700-abf8-11e9-8b07-4aac03615976.PNG)

저는 불필요한 commit을 하지 않기 위해 local에서 작업했습니다.



여기서 상단 `Theme`의 remote_theme 주석을 해제합니다.

```yaml
# remote_theme           : "mmistakes/minimal-mistakes"
remote_theme           : "mmistakes/minimal-mistakes"
```

이 부분은 Github에 이미 저장돼있는 theme을 자동으로 불러오는 기능입니다. 



색상도 변경할 수 있습니다.

```yaml
minimal_mistakes_skin    : "default" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"
```

원하는 색상을 `"default"`에 치환하면 됩니다.




### Pages 세팅하기

앞선 `_config.yml`에서 다양한 설정들을 수정할 수 있습니다.

```yaml
locale                   : "ko-KR"
title                    : "Yomandawg"
title_separator          : "-"
name                     : "요맨"
description              : "개인 블로그"
# ...
```

이 외에 다양한 설정을 변경할 수 있습니다. 쭉 읽어보시고 



그 다음 바로 밑의 `Site Settings`에서 다음과 같이 바꿔줍니다.

```yaml
url                      : # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : # the subpath of your site, e.g. "/blog"

url : "https://yomandawg.github.io" # host 주소 
baseurl : "" # subpath 주소
```

예를 들어 pages URL이 `https://yomandawg.github.io/blog` 라면

`url: "https://yomandawg.github.io"`와 `baseurl: "/blog"`로 설정합니다.



기타 사항들도 변경할 수 있습니다.

```yaml
locale                   : "ko-KR"
title                    : "Yomandawg"
title_separator          : "-"
name                     : "요맨"
description              : "개인 블로그"
# ...
# 이 외에 다양한 설정을 변경할 수 있습니다.
```




### Pages 시작하기

페이지 URL로 접속하면 나오는 초기 `index` 템플릿을 설정할 차례입니다.

repository에 `index.html`파일을 만들고, 다음과 같이 작성합니다.

```html
---
layout: home
author_profile: true
---
```

참고로 같은 파일이 테마 repository에도 있습니다.

다음과 같이 저장합니다.

![5](https://user-images.githubusercontent.com/45955154/61590783-d973e100-abf8-11e9-8d30-5b41a98abd9c.PNG)

자 드디어 모든 준비가 완료됐습니다!

이제 내 repository의 Settings에 들어가 Danger Zone 위에 있는 **Github Pages** 기능을 봅니다. 

여기서 **Source**를 *master branch*로 설정해주시면  

![1](https://user-images.githubusercontent.com/45955154/61590771-ccef8880-abf8-11e9-9f2a-44406e22a4af.PNG)

![6](https://user-images.githubusercontent.com/45955154/61590784-db3da480-abf8-11e9-8237-7073794bcd89.PNG)

위와 같은 안내문구와 함께 설정한 주소로 pages가 생성됩니다.




### 포스팅하기

repository에 `_post/` 경로를 만듭니다.

포스팅은 이 `_post/` 경로 안에서만 관리되며 markdown 문서로 작성합니다.

파일 이름은 `2018-03-25-post-name.md` 형식으로 날짜와 이름을 hyphen과 묶어서 작성합니다.




## 후기

첫 포스팅인데 블로그 작성하기가 생각보다 쉽지 않네요.

부족하지만 읽어주셔서 감사합니다!