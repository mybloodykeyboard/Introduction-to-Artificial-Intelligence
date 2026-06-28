---
title: 홈
layout: default
nav_order: 1
description: 인공지능 개론 기출문제와 이론을 정리한 위키
permalink: /
---

# 인공지능 개론 시험 정리
{: .fs-9 }

기출문제와 이론을 **강의별 · 개념별 · 시험유형별** 세 갈래로 정리한 위키.
원하는 방식으로 들어가서 보면 됨.
{: .fs-6 .fw-300 }

[강의 노트]({{ site.baseurl }}/docs/lecture-notes/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[시험 대비]({{ site.baseurl }}/docs/study-guides/){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## 강의별 (Lecture)

순서대로 따라가며 보는 구조.

- [01. Uninformed Search]({{ site.baseurl }}/docs/lecture-notes/01-uninformed-search/)
- [02. Informed Search (A\*)]({{ site.baseurl }}/docs/lecture-notes/02-informed-search/)
- [03. Local Search]({{ site.baseurl }}/docs/lecture-notes/03-local-search/)
- [04. Adversarial Search]({{ site.baseurl }}/docs/lecture-notes/04-adversarial-search/)
- [05. Constraint Satisfaction]({{ site.baseurl }}/docs/lecture-notes/05-csp/)
- [06. Logic & Knowledge]({{ site.baseurl }}/docs/lecture-notes/06-logic/)
- [07. Reinforcement Learning]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/)
- [08. Neural Networks]({{ site.baseurl }}/docs/lecture-notes/08-neural-networks/)

## 개념별 (Concept)

주제 단위로 묶어서 보는 구조.

- [탐색 방법 (Search Methods)]({{ site.baseurl }}/docs/concepts/search-methods/)
- [Local Search]({{ site.baseurl }}/docs/concepts/local-search/)
- [Adversarial Search]({{ site.baseurl }}/docs/concepts/adversarial-search/)
- [Reinforcement Learning]({{ site.baseurl }}/docs/concepts/reinforcement-learning/)

## 시험유형별 (Study Type)

시험 직전에 보는 구조.

- [기출문제]({{ site.baseurl }}/docs/study-guides/past-exams/)
- [모범답안]({{ site.baseurl }}/docs/study-guides/model-answers/)
- [비교표 모음]({{ site.baseurl }}/docs/study-guides/comparison-tables/)
- [핵심 요약 (암기)]({{ site.baseurl }}/docs/study-guides/cheat-sheet/)

---

## 쓰는 법

각 문서는 마크다운 파일 하나임. 내용 고치려면:

1. 좌측 사이드바에서 문서 선택
2. 페이지 맨 아래 **이 페이지 수정하기** 링크로 GitHub 편집기 열기
3. 고치고 commit → 1~2분 뒤 자동 반영

새 문서 추가는 `docs/` 아래에 `.md` 파일 만들고 상단 front matter(`title`, `parent`, `nav_order`)만 맞추면 사이드바에 자동 등록됨.
