---
layout: page
title: How to use
---


# 👩‍💻 Eye-Remocon 기술블로그

해당 기술 블로그는 Eye-Remocon 팀의 Home Edge 프로젝트 내용이 담겨있습니다.
Home Edge 프로젝트를 진행하면서 해당 기능을 어떻게 구현하는지에 대한 내용을 중심으로
관련된 기술에 대한 스터디 내용 또한 적을 예정입니다.

🏋️‍♂️ 프로젝트를 완성시키기 위한 우리들의 성장 과정을 지켜봐주시면 감사하겠습니다.



----

## ✍️ 블로그 포스팅 방법

### 블로그 카테고리
1. HW-control : IoT 장비 컨트롤과 라즈베리파이(OS)에 대한 내용
2. LF-Edge-Contribution : LF HomeEdge open source 사용법과 Contribution한 내용
3. Emotion-Behavior-Detection : 단말을 통해 감정 및 행동 인식에 대한 내용
4. Container : Docker를 사용하여 Container를 구현하는 내용



### 글작성 방법
1. 글 작성은 '_posts' 폴더에 md 파일로 작성
2. 글 제목은 'YYMMDD-(해당 카테고리 소문자로)-포스트 제목' 으로 작성
3. 글 작성 시 윗 부분에 작성

<pre>
---
layout: post
title: (포스팅 제목)
categories : (해당 카테고리)
---
</pre>


----

##  📁 Directory 구조

```
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── category.html
|   ├── google_analytics.html
|   ├── item.html
|   ├── pagination.html
|   ├── sidebar.html
|   └── tag.html
├── _layouts
|   ├── default.html
|   ├── page.html
|   ├── post.html
|   └── tag.html
├── _posts
├── _data
|   └── members.yml
├── _site
├── asset
├── js
├── page
|   ├── 1.category.html
|   └── 2.tag.html
├── public
|   └── css
└── index.html
```

### _config.yml
_config.yml파일은 지킬 사이트의 환경설정을 포함하고 있습니다.

### _posts
_posts 폴더는 블로그 글이 저장된 곳입니다. 포스팅할 때 글 작성법을 따라야 합니다.

### _site
_site는 컴파일된 HTML파일이 저장되는 장소입니다. 일단 빌트하면 파일들은 호스팅을 위해 서버에 직접 업로드됩니다.

### _includes
페이지의 틀을 위한 HTML파일이 저장되는 장소입니다.

### _drafts
퍼블리싱 안된 블로그 포스트를 담아놓는 폴더입니다. _draft폴더는 우리 실제 홈페이지에 퍼블리싱없이 작업할 수 있도록 도와줍니다.

### _layouts
_layouts 폴더에는 콘텐츠를 감쌀 템플릿이 포함되어 있습니다. 헤더와 푸터, 네이베이션과 같이 전형적으로 반복되는 코드가 _layouts안에 포함되어 있습니다.

### public
CSS 파일을 포함하는 폴더입니다.

### asset
이미지 및 폰트 파일을 포함하는 폴더입니다.

### js
javascript 파일을 포함하는 폴더입니다.

### index.html
페이지 첫 시작에서 사용되는 HTML 파일입니다.



