---
title: 탐색 방법 (Search Methods)
layout: default
parent: 개념별 정리
nav_order: 1
permalink: /docs/concepts/search-methods/
---

# 탐색 방법 (Search Methods)
{: .no_toc }

상태 공간 탐색 전반을 한 장에 정리.

## 전체 분류

- **무정보 탐색**: BFS, DFS, UCS, DLS, IDS → [01. Uninformed Search]({{ site.baseurl }}/docs/lecture-notes/01-uninformed-search/)
- **정보 탐색**: Greedy, A\* → [02. Informed Search]({{ site.baseurl }}/docs/lecture-notes/02-informed-search/)
- **제약 충족**: backtracking + AC-3 → [05. CSP]({{ site.baseurl }}/docs/lecture-notes/05-csp/)

## 핵심 비교표

| 기준 | BFS | UCS | DFS | A\* |
|---|---|---|---|---|
| 확장 우선순위 | 얕은 노드 | g(n) 작은 것 | 깊은 노드 | g(n)+h(n) |
| 완전성 | O | O | X | O |
| 최적성 | 단계수 기준 O | O | X | h admissible면 O |

## 자주 헷갈리는 것

- **완전성 ≠ 최적성**. 답을 "찾느냐"와 "최적이냐"는 별개.
- tree search vs graph search: explored set 유무.
- A\*에서 h=0 → UCS, g=0 → greedy.
