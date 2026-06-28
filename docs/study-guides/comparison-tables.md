---
title: 비교표 모음
layout: default
parent: 시험 대비
nav_order: 3
permalink: /docs/study-guides/comparison-tables/
---

# 비교표 모음
{: .no_toc }

헷갈리는 개념들을 표로 한 번에. 시험 직전 훑기용.

## 무정보 탐색

| 알고리즘 | 완전성 | 최적성 | 시간 | 공간 |
|---|---|---|---|---|
| BFS | O | 단계수 O | O(b^d) | O(b^d) |
| DFS | X | X | O(b^m) | O(bm) |
| UCS | O | O | 지수 | 지수 |
| IDS | O | 단계수 O | O(b^d) | O(bd) |

## A* 휴리스틱

| 성질 | 정의 | 보장 |
|---|---|---|
| Admissible | h(n) ≤ 실제비용 | tree search 최적 |
| Consistent | h(n) ≤ c + h(n') | graph search 최적 |

## Q-learning vs SARSA

| 항목 | Q-learning | SARSA |
|---|---|---|
| 정책 | off-policy | on-policy |
| 타깃 | max_a' Q(s',a') | Q(s',a') (실제 a') |
| 성향 | 공격적 | 보수적 |

## Minimax 관련

| 항목 | 값 |
|---|---|
| Minimax 시간 | O(b^m) |
| Alpha-Beta (최적 순서) | O(b^(m/2)) |

> 본인 시험 범위에 맞는 비교표를 계속 추가.
