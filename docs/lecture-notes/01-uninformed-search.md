---
title: "01. Uninformed Search"
layout: default
parent: 강의 노트
nav_order: 1
permalink: /docs/lecture-notes/01-uninformed-search/
---

# 01. Uninformed Search (무정보 탐색)
{: .no_toc }

목표까지의 거리 정보 없이, 정의된 규칙으로만 상태 공간을 훑는 탐색.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- **문제 정의 5요소**: 초기 상태, 행동(actions), 전이 모델(transition model), 목표 검사(goal test), 경로 비용(path cost).
- **상태 공간(state space)**: 가능한 모든 상태 + 전이로 이루어진 그래프.
- **노드 vs 상태**: 노드는 탐색 트리의 구성 요소(부모/깊이/비용 포함), 상태는 세계의 표현. 한 상태가 여러 노드로 나타날 수 있음.

## 주요 알고리즘

| 알고리즘 | 자료구조 | 완전성 | 최적성 | 시간 | 공간 |
|---|---|---|---|---|---|
| BFS | 큐(FIFO) | O | O (균일 비용일 때) | O(b^d) | O(b^d) |
| DFS | 스택(LIFO) | X (무한·순환) | X | O(b^m) | O(bm) |
| UCS (Dijkstra) | 우선순위 큐 | O | O | O(b^(1+⌊C*/ε⌋)) | 동일 |
| DLS | 깊이 제한 DFS | X | X | O(b^l) | O(bl) |
| IDS | 반복 DLS | O | O (균일 비용) | O(b^d) | O(bd) |

> b = 분기 계수, d = 목표 깊이, m = 최대 깊이, C* = 최적 비용, ε = 최소 단계 비용.

## 시험 포인트

- BFS는 **얕은 목표**에 최적, 메모리가 약점.
- IDS는 BFS의 최적성 + DFS의 메모리 효율 → 깊이 모를 때 default.
- UCS와 BFS의 차이: UCS는 **경로 비용** 기준, BFS는 **단계 수** 기준.
- Graph search vs Tree search: 방문 집합(explored set) 유무. 중복 상태 재방문 막음.

## 관련 기출

- [기출문제 → 탐색]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [개념별: 탐색 방법]({{ site.baseurl }}/docs/concepts/search-methods/)
- [다음 강의: 02. Informed Search]({{ site.baseurl }}/docs/lecture-notes/02-informed-search/)
