---
title: "02. Informed Search (A*)"
layout: default
parent: 강의 노트
nav_order: 2
permalink: /docs/lecture-notes/02-informed-search/
---

# 02. Informed Search (정보 탐색)
{: .no_toc }

휴리스틱 h(n)으로 목표 방향을 추정해 탐색을 가속.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- **평가 함수 f(n)** 으로 확장 순서 결정.
- **휴리스틱 h(n)**: 노드 n에서 목표까지의 추정 비용.
- **Greedy Best-First**: f(n) = h(n). 빠르지만 최적·완전 보장 안 됨.
- **A\***: f(n) = g(n) + h(n). g는 실제 비용, h는 추정 비용.

## A* 최적성 조건

- **Admissible (허용성)**: h(n) ≤ 실제 비용. 절대 과대평가 안 함 → **tree search 최적**.
- **Consistent (일관성)**: h(n) ≤ c(n,n') + h(n'). 삼각부등식 → **graph search 최적**.
- consistent면 admissible (역은 성립 안 함).

## 휴리스틱 비교

- **Dominance**: 모든 n에서 h2(n) ≥ h1(n)이고 둘 다 admissible이면 h2가 우세 → 노드 더 적게 확장.
- 예 (8-퍼즐): 잘못 놓인 타일 수 < 맨해튼 거리 → 맨해튼이 우세.

## 시험 포인트

- A*가 BFS/UCS보다 좋은 이유: 같은 최적성 보장 + 휴리스틱으로 확장 노드 감소.
- h(n)=0 이면 A* = UCS.
- admissible vs consistent 구분, 반례 만들기 자주 출제.

## 관련 기출

- [기출문제 → 탐색]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [개념별: 탐색 방법]({{ site.baseurl }}/docs/concepts/search-methods/)
- [이전: 01. Uninformed Search]({{ site.baseurl }}/docs/lecture-notes/01-uninformed-search/)
- [다음: 03. Local Search]({{ site.baseurl }}/docs/lecture-notes/03-local-search/)
