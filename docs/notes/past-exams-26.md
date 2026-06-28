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

> 시험의 트리 그림은 강의 PDF(`1.UninformedSearch`, `2-1.InformedSearch-HeuristicSearch`)의 **8-puzzle 예시 그림과 동일한 수준**. 그 예시 기준으로 각 알고리즘의 확장 순서를 따라가면 됨.

## 02. Informed & Local Search

- Informed search(h1=misplaced tiles, h2=Manhattan distance) 부분은 **01 Q1의 8-puzzle 문제가 커버** — 8-puzzle 한 문제가 01~02를 관통함.
- Local search(hill-climbing, 8-queens 등) 단독 기출: **미확인**.

> Local search 쪽 기출이 따로 있었으면 알려주면 추가.

## 03. Adversarial Search

### Q1. Tic-tac-toe 게임 트리 (그림 제시형)

**문제**
- **Tic-tac-toe 게임 트리**가 강의 PDF(`3.Adversarial_search`) 예시 그대로 제시됨.
- (a) **minimax**로 각 노드 값을 채워 최적 수를 결정.
- (b) 일부 탐색으로 **채워지는 칸(보드 상태 / 노드 값)을 직접 그려보는** 문제.

**풀이 포인트**
- terminal utility: 승 `+1`, 패 `−1`, 무 `0`. MAX는 자식 최대값, MIN은 자식 최소값.
- 말단(terminal)부터 위로 `MINIMAX-VALUE`를 전파하며 각 노드 값을 채움.
- 보드를 직접 그리는 문항은 successor(합법 수) 전개 → 각 상태의 utility/minimax 값 순으로 표기.
- 방법론: [03. Adversarial Search]({{ site.baseurl }}/docs/lecture-notes/03-adversarial-search/)

### 그 외 꼬리문제 (추정 — 미확정)

실제 출제 여부 확인 안 됨. 단원·문제 성격상 가능성 높은 유형(제안):

- **Alpha-beta pruning**: 같은 트리에서 `α ≥ β`로 잘리는 노드를 직접 표시. move ordering에 따른 차이.
- **평가함수(evaluation function)**: 비종료 상태 추정, `Eval(s) = w1·f1 + w2·f2 + ...` 적용.
- **MIN 비최적 플레이 시** MAX 결과가 minimax 값보다 나빠지지 않음을 서술.
- **복잡도 비교**: minimax `O(b^d)` vs alpha-beta(최선) `O(b^(d/2))`.

> 위는 추정. 실제로 나온 "가지 문제"가 기억나면 알려주면 확정해서 정리.

## 04. Constraint Satisfaction (CSP)

### Q. 4-Queens (보류)

- 4-Queens 관련 문제가 출제됨 — 세부는 **보류**.
- **4-Queens example 참고할 것**: [04. CSP]({{ site.baseurl }}/docs/lecture-notes/04-csp/)의 forward checking 추적 예제(해: X1=2, X2=4, X3=1, X4=3) 및 강의 PDF `4.ConstraintSatisfactionProblem`의 4-Queens 예시.

> 기억나는 대로 알려주면 정확히 채움.

## 05. Classification (Decision Tree · kNN)

_채울 예정._

## 06. Clustering

_채울 예정._

## 07. Reinforcement Learning

_채울 예정._

## 08. Artificial Neural Network

_채울 예정._
