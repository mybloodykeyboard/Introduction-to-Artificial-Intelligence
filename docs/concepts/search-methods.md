---
title: Search Methods
layout: default
parent: 컨셉별
nav_order: 1
permalink: /docs/concepts/search-methods/
---

# Search Methods
{: .no_toc }

탐색(search) 알고리즘은 크게 휴리스틱을 쓰지 않는 무정보 탐색(uninformed search)과 목표까지의 추정 비용을 쓰는 정보 탐색(informed/heuristic search)으로 나뉜다. 평가 기준은 완전성(completeness), 최적성(optimality), 시간/공간 복잡도 4가지이며, `b`=분기계수, `d`=최적해 깊이, `m`=최대 깊이다.

## 한눈에

| 알고리즘 | 확장 기준 | 완전성 | 최적성 | 시간 | 공간 |
|---|---|---|---|---|---|
| BFS | 가장 얕은 노드 (FIFO) | O | O (단계비용 동일 시) | `O(b^d)` | `O(b^d)` |
| UCS | 경로비용 `g(n)` 최소 | O | O | `O(b^(1+⌊C*/ε⌋))` | `O(b^(1+⌊C*/ε⌋))` |
| DFS | 가장 깊은 노드 (LIFO) | X | X | `O(b^m)` | `O(bm)` |
| DLS | DFS + 깊이 제한 `L` | X | X | `O(b^L)` | `O(bL)` |
| IDS | `L`을 1씩 늘리며 DFS 반복 | O | O (경로비용=깊이 비감소 시) | `O(b^d)` | `O(bd)` |
| Greedy | `f(n)=h(n)` | X | X | `O(b^m)` | `O(b^m)` |
| A* | `f(n)=g(n)+h(n)` | O | O (admissible h) | 지수적 | 지수적 |

## 핵심 포인트

- 완전성 ≠ 최적성: 해를 "찾는가"(완전성)와 "최소 비용 해를 찾는가"(최적성)는 별개. DFS는 둘 다 X, IDS는 둘 다 O.
- IDS는 DFS의 선형 메모리 `O(bd)`와 BFS의 완전성을 결합. 노드 대부분이 마지막 깊이에서 생성되므로 반복 낭비는 작다(`b=10, d=5`에서 BFS 대비 약 11%).
- best-first search는 평가함수 `f(n)`을 기준으로 fringe를 정렬. UCS·Greedy·A*는 모두 이 일반형의 특수 사례다.
- A*에서 `h=0`이면 UCS(`f=g`), `g=0`이면 Greedy(`f=h`)와 같아진다.
- admissible 휴리스틱(`h(n) ≤ h*(n)`, 절대 과대평가하지 않음)이면 A*는 최적. 직선거리(SLD)가 대표 예.
- Dominance: 두 휴리스틱이 admissible이고 항상 `h2 ≥ h1`이면 `h2`가 탐색에 더 좋다(8-puzzle에서 맨해튼 거리 `h2`가 제자리 아닌 타일 수 `h1`을 지배).

## 자세히

- [01 Uninformed Search]({{ site.baseurl }}/docs/lecture-notes/01-uninformed-search/)
- [02 Informed and Local Search]({{ site.baseurl }}/docs/lecture-notes/02-informed-and-local-search/)
