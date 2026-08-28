# manduMY.dev — 기술 정리 블로그

GitHub Pages + Jekyll로 운영하는 기술 블로그. → https://mandumy.github.io

## 글 쓰는 법

`_posts/` 에 `YYYY-MM-DD-slug.md` 파일을 만들면 끝. GitHub에 push하면 몇 분 뒤 자동으로 배포된다.

```markdown
---
title: "글 제목"
date: 2026-08-28 21:00:00 +0900
tags: [Redis, Cache]
category: backend
excerpt: "목록에 보일 한두 문장 요약."
---

본문 (마크다운)
```

> 💡 이 저장소는 `tech-blog-writer` 스킬과 함께 쓰도록 만들어졌다.
> "오늘 ~를 배웠어"라고 말하면 스킬이 필요한 내용을 물어보고, 위 형식의 글을 대신 써준다.

## 구조

```
_config.yml          사이트 설정 (제목, 태그라인, 플러그인)
_layouts/            default / home / post 템플릿
_posts/              글 (마크다운)
assets/css/style.css 화이트 테마 스타일
index.html           홈 (최신 글 목록)
archive.html         글 전체
tags.html            태그별 모아보기
about.md             소개
```

## 로컬 미리보기 (선택)

```bash
bundle install
bundle exec jekyll serve
# http://localhost:4000
```
