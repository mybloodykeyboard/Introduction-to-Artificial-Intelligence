---
title: "01. Uninformed Search"
layout: default
parent: 강의별
nav_order: 1
permalink: /docs/lecture-notes/01-uninformed-search/
---

# 01. Uninformed Search (무정보 탐색)
{: .no_toc }

목표나 비용에 대한 추가 정보 없이 탐색 전략만으로 해를 찾는 BFS·DFS·DLS·IDS·UCS를 평가 4기준으로 정리한다.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

탐색 전략을 평가하는 4가지 기준.

- **완전성(Completeness)**: 해가 존재하면 항상 찾는가.
- **최적성(Optimality)**: 항상 최소 비용(최단) 해를 찾는가.
- **시간 복잡도(Time)**: 생성되는 노드 수(최악).
- **공간 복잡도(Space)**: 메모리에 동시에 유지되는 최대 노드 수(최악).

복잡도 표기 변수.

- `b` = 최대 분기계수(branching factor)
- `d` = 최적해의 깊이(depth)
- `m` = 상태공간의 최대 깊이(무한일 수 있음)

## 주요 알고리즘

### BFS (너비우선 탐색)

- 가장 얕은 미확장 노드부터 확장. Fringe는 FIFO 큐, 새 후속 노드는 큐의 **끝**에 추가.
- 완전성 O, 최적성 O(단계 비용 동일 시), 시간 `O(b^d)`, 공간 `O(b^d)`.
- 치명적 단점은 지수적 공간 복잡도. `b=10, d=12`이면 메모리가 10PB 수준에 도달.

### DFS (깊이우선 탐색)

- 가장 깊은 미확장 노드부터 확장. Fringe는 LIFO 스택, 새 후속 노드는 스택의 **앞**에 추가.
- 완전성 X(무한 깊이 경로 존재 시), 최적성 X.
- 시간 `O(b^m)`, 공간 `O(bm)`으로 선형 — 최대 장점.

### DLS (Depth-Limited Search)

- DFS에 깊이 제한 `L`을 둠. 무한 경로 문제는 해결하지만 해가 `L`보다 깊으면 불완전.
- `L`은 문제 지식(예: 상태공간 지름)으로 정하나 실제로는 미리 알기 어려움.

### IDS (반복 심화 탐색)

- `L=1`부터 시작해 해를 찾을 때까지 깊이 제한을 증가시키며 DFS를 반복.
- 완전성 O, 최적성 O(경로비용이 깊이의 비감소 함수일 때), 시간 `O(b^d)`, 공간 `O(bd)`.
- DFS의 적은 메모리 + BFS의 완전성을 결합. 탐색공간이 크고 해의 깊이를 모를 때 선호되는 무정보 탐색 방법.

### UCS (균일비용 탐색)

- fringe에서 경로비용 `g(n)`이 최소인 노드를 항상 확장. 비용이 모두 비슷하면 BFS와 유사하게 동작.
- 모든 단계비용이 ε>0 이상이고 분기계수가 유한하면 완전성 O, 최적성 O.
- 시간·공간 `O(b^(1+⌊C*/ε⌋))`, `C*`는 최적해 비용.

### 종합 비교표

| 기준 | BFS | UCS | DFS | DLS | IDS |
|------|-----|-----|-----|-----|-----|
| 완전성 | Yes | Yes | No | No | Yes |
| 최적성 | Yes | Yes | No | No | Yes |
| 시간 | `O(b^(d+1))` | `O(b^⌈C*/ε⌉)` | `O(b^m)` | `O(b^L)` | `O(b^d)` |
| 공간 | `O(b^(d+1))` | `O(b^⌈C*/ε⌉)` | `O(bm)` | `O(bL)` | `O(bd)` |

## 시험 포인트

- **IDS "반복 낭비" 비판 반박**: 대부분의 노드는 마지막 깊이 `d`에서 생성됨. `b=10, d=5`일 때 N(IDS)≈123,450 vs N(BFS)≈111,110 → 약 11% 차이에 불과. 선형 메모리 `O(bd)`를 얻는 대가로는 미미함.
- **UCS 최적성 (귀류법)**: 비최적이라 가정하면 찾은 해보다 비용이 작은 목표 상태가 존재해야 하는데, UCS는 정의상 그 더 싼 노드를 먼저 확장했을 것이므로 모순.
- **UCS 완전성 조건**: 모든 단계비용이 ε>0 이상 + 분기계수 유한. 목표 비용에 도달하기까지 유한 번의 확장만 필요.
- **BFS vs DFS 사용 기준**:
  - BFS 유리 — 해가 얕음, 무한 경로·루프가 많음, 탐색공간이 작음.
  - DFS 유리 — 해(goal)가 많음, 루프가 적음, 무한 경로가 없음, 메모리 제약이 큼(공간이 선형이라는 점이 결정적).
- **DLS의 한계**: 깊이 제한 `L`을 미리 정하기 어렵고, 해가 `L`보다 깊으면 불완전.

## 더 보기

관련: [Search Methods 컨셉]({{ site.baseurl }}/docs/concepts/search-methods/), [예상문제]({{ site.baseurl }}/docs/notes/predicted-questions/) / 다음: [02. Informed & Local Search]({{ site.baseurl }}/docs/lecture-notes/02-informed-and-local-search/)
