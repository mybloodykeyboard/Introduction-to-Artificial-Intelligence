---
title: "03. Adversarial Search"
layout: default
parent: 강의별
nav_order: 3
permalink: /docs/lecture-notes/03-adversarial-search/
---

# 03. Adversarial Search (적대적 탐색 · 게임)
{: .no_toc }

상대가 나의 이익을 최소화하려 두는 game에서 최선의 수를 선택하는 탐색.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

게임의 표준 가정 4가지:

- 2인 교대 턴(turn-taking) — 두 플레이어의 행동이 번갈아 일어남.
- 결정적(deterministic) — 행동 결과가 예측 가능.
- 제로섬(zero-sum) — 한쪽 효용이 +1이면 상대는 −1. 적대적 상황의 원천.
- 완전정보(perfect information) — 현재 게임 상태가 완전히 관측 가능.

일반 탐색과의 차이: 탐색의 해는 목표까지의 경로지만, 게임의 해는 상대의 모든 응수에 대한 수를 지정하는 전략(strategy). 시간 제한 탓에 근사해가 일반적.

게임을 탐색 문제로 정의하는 4구성요소:

| 구성요소 | 의미 |
| --- | --- |
| 초기 상태 | 보드 구성 |
| Successor 함수 | 합법 수(legal move) 목록 |
| Terminal test | 게임 종료 판정 |
| Utility 함수 | 종료 상태의 수치 — 승 +1, 패 −1, 무 0 |

트리 크기는 `O(b^d)`. 체스는 b≈35, d≈100 → 약 10^154로 전수 탐색 불가능 → 유한 시간 내 최적 결정이 핵심.

## Minimax

무결점(infallible) 상대를 가정하고 MAX의 최적 전략을 계산. 두 플레이어 모두 최적 플레이를 가정.

MINIMAX-VALUE(n):

- terminal이면 UTILITY(n).
- MAX 노드면 자식들의 최대값.
- MIN 노드면 자식들의 최소값.

완전한 깊이우선 탐색이며 시간 `O(b^d)`, 공간 `O(bd)`. Minimax는 MAX 입장에서 최악의 경우(상대 최적 응수) 결과를 최대화.

MIN이 비최적으로 플레이하면? MAX의 결과는 minimax 값보다 나빠지지 않으며 오히려 더 좋아질 수 있음.

## Alpha-Beta Pruning

아이디어: 최종 결정에 영향을 주지 않는 가지를 잘라(pruning) 같은 결과를 더 적은 탐색으로 얻음. 결과는 minimax와 완전히 동일하게 보장.

- α = DFS 경로상 MAX가 지금까지 확보한 최고값.
- β = DFS 경로상 MIN이 확보한 최저값.

탐색 중 α·β를 갱신하다가, 어떤 노드의 값이 조상의 α(또는 β) 기준으로 이미 선택될 수 없음이 확정되면(즉 α≥β) 남은 가지를 prune.

복잡도 비교:

| 경우 | 복잡도 | 비고 |
| --- | --- | --- |
| 최악 | `O(b^d)` | 프루닝이 전혀 안 되는 수 순서 |
| 최선 | `O(b^(d/2))` | 각 플레이어 최선 수가 가장 왼쪽 |

최선일 때 유효 분기계수는 √b(체스 35 → 약 6)로 줄어 같은 시간에 약 2배 깊이 탐색 가능. 실무 평균은 최선에 가깝고, 좋은 수 순서(move ordering)가 효율을 좌우.

## 평가함수

비종료 상태가 플레이어에게 얼마나 좋은지의 추정. 깊이 제한 탐색에서 terminal utility 대신 사용. 보통 (내 점수) − (상대 점수) 형태. 예: Othello는 백 돌 수 − 흑 돌 수.

체스는 특징들의 선형 가중합:

`Eval(s) = w1·f1(s) + … + wn·fn(s)`

예: w1=9, f1 = 백 퀸 수 − 흑 퀸 수. 제로섬이므로 한쪽에 X면 상대에겐 −X.

## 시험 포인트

- 게임 4가정(2인 교대 / 결정적 / 제로섬 / 완전정보)과 4구성요소(초기 상태 / Successor / Terminal test / Utility)는 한 세트로 암기.
- 게임의 해 = 경로가 아니라 전략(strategy).
- Minimax 시간 `O(b^d)`, 공간 `O(bd)`. MIN 비최적 시 MAX 결과는 minimax 값 이상.
- Alpha-beta pruning 조건은 α≥β, 결과는 minimax와 동일, 최선 복잡도 `O(b^(d/2))`, 유효 분기계수 √b.
- 트리 제시형 문제: 각 노드 옆에 α·β 구간을 적고 "β≤α이면 중단" 규칙을 명시하는 것이 채점 포인트.
- 평가함수는 선형 가중합 형태와 체스 예(w1=9, 퀸 수 차)를 함께.

게임 AI 역사 (단답 대비):

| 게임 | 사건 |
| --- | --- |
| Checkers | Chinook이 1994년 인간 챔피언 Marion Tinsley의 40년 시대를 종결 |
| Chess | Deep Blue(알파베타 기반)가 1997년 Kasparov를 6국 매치에서 격파 |
| Go | 트리가 너무 큼(b>300, d>150 → 300^150) — 당시 자료 기준 컴퓨터가 인간보다 크게 열세 |

## 더 보기

- 관련: [Adversarial Search 컨셉]({{ site.baseurl }}/docs/concepts/adversarial-search/)
- 이전: [02]({{ site.baseurl }}/docs/lecture-notes/02-informed-and-local-search/)
- 다음: [04. CSP]({{ site.baseurl }}/docs/lecture-notes/04-csp/)
