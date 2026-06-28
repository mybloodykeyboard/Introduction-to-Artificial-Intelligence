---
title: 26전기 기출 모음
layout: default
parent: 정리노트
nav_order: 1
permalink: /docs/notes/past-exams-26/
---

# 26전기 기출 모음
{: .no_toc }

2026학년도 전기 인공지능 개론 실제 기출. 단원별로 정리.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

> 아래 단원별 칸은 실제 기출을 받는 대로 채워 넣음. 각 문항은 **문제 → 풀이/답 → 관련 강의** 형식으로 정리.

## 01. Uninformed Search

### Q1. 8-puzzle 탐색 비교 (그림 제시형, 01→02 연계)

**문제**
- 8-puzzle을 **트리 형태로 그린 탐색 그림**이 그대로 제시됨.
- (a) 여러 **uninformed search**(BFS·DFS·IDS·UCS 등)로 노드를 **어떤 순서로 확장/이동**하는지 서술.
- (b) 이어서 **informed search**로 휴리스틱 **h1 = misplaced tiles**(제자리에 없는 타일 수), **h2 = Manhattan distance**(각 타일의 목표 위치까지 거리 합)를 적용해 A\*가 **어떤 순서로 탐색**하는지 서술.
- (c) 꼬리문제로 이어지며 최종적으로 **어떤 알고리즘이 가장 좋은지 비교**.

**풀이 포인트**
- 실제 노드 확장 순서는 제시된 트리 그림에 종속됨 → 그림 없이는 순서 확정 불가.
- fringe 기준: BFS=FIFO 큐, DFS=LIFO 스택, UCS=g(n) 최소, Greedy=h(n) 최소, A\*=`f=g+h`.
- `h2 ≥ h1` 이고 둘 다 admissible → **h2가 h1을 dominate** → A\*(h2)가 노드를 더 적게 확장.
- 실증(d=12 평균 확장 노드): IDS 3,644,035 / A\*(h1) 227 / A\*(h2) 73.
- 결론: 무정보(IDS 등) < A\*(h1) < A\*(h2). admissible을 유지하며 휴리스틱 값이 클수록 효율 ↑.
- 방법론: [01. Uninformed Search]({{ site.baseurl }}/docs/lecture-notes/01-uninformed-search/), [02. Informed & Local Search]({{ site.baseurl }}/docs/lecture-notes/02-informed-and-local-search/)

> 트리 그림을 주면 각 알고리즘의 실제 노드 확장 순서까지 정확히 채워 넣을 수 있음.

## 02. Informed & Local Search

_채울 예정._

## 03. Adversarial Search

_채울 예정._

## 04. Constraint Satisfaction (CSP)

### Q. 4-Queens (세부 미확정)

- 4-Queens 관련 문제가 출제됨 — **세부 내용 기억 미확정**.
- 추정 유형: backtracking + forward checking 도메인 변화 추적, 또는 변수·도메인·제약 형식화.
- 참고: [04. CSP]({{ site.baseurl }}/docs/lecture-notes/04-csp/)의 4-Queens forward checking 추적 예제(해: X1=2, X2=4, X3=1, X4=3).

> 기억나는 대로 알려주면 정확히 채움.

## 05. Classification (Decision Tree · kNN)

_채울 예정._

## 06. Clustering

_채울 예정._

## 07. Reinforcement Learning

_채울 예정._

## 08. Artificial Neural Network

_채울 예정._
