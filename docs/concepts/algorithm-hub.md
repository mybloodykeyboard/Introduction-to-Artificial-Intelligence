---
title: 알고리즘 허브
layout: default
parent: 컨셉별
nav_order: 9
permalink: /docs/concepts/algorithm-hub/
---

# 알고리즘 허브
{: .no_toc }

전 단원 알고리즘을 한눈에 비교. 시험 직전 훑기용.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

`b`=최대 분기계수(branching factor), `d`=최적해 깊이, `m`=상태공간 최대 깊이, `L`=깊이 제한, `C*`=최적해 비용, `e`=최소 단계비용.

## 탐색 알고리즘

확장 기준(expansion order)·완전성(completeness)·최적성(optimality)·시간·공간 비교. 무정보(uninformed) 5종은 [01. Uninformed Search](/docs/lecture-notes/01-uninformed-search/), 정보 기반(informed) 3종은 [02. Informed & Local Search](/docs/lecture-notes/02-informed-and-local-search/).

| 알고리즘 | 확장 기준 | 완전성 | 최적성 | 시간 | 공간 | 노트 |
|---|---|---|---|---|---|---|
| BFS (너비우선) | 가장 얕은 노드 (FIFO 큐) | Yes | Yes (단계비용 동일 시) | `O(b^d)` | `O(b^d)` | [01](/docs/lecture-notes/01-uninformed-search/) |
| DFS (깊이우선) | 가장 깊은 노드 (LIFO 스택) | No | No | `O(b^m)` | `O(bm)` | [01](/docs/lecture-notes/01-uninformed-search/) |
| DLS (깊이제한) | 깊이 `L`까지 DFS | No (`L<d`이면) | No | `O(b^L)` | `O(bL)` | [01](/docs/lecture-notes/01-uninformed-search/) |
| IDS (반복심화) | `L`을 1씩 늘리며 DFS 반복 | Yes | Yes (비용이 깊이의 비감소 함수일 때) | `O(b^d)` | `O(bd)` | [01](/docs/lecture-notes/01-uninformed-search/) |
| UCS (균일비용) | 경로비용 `g(n)` 최소 | Yes (`e>0`, `b` 유한) | Yes | `O(b^(1+C*/e))` | `O(b^(1+C*/e))` | [01](/docs/lecture-notes/01-uninformed-search/) |
| Greedy (탐욕) | 휴리스틱 `f=h(n)` 최소 | No (방문기록 없으면 루프) | No | `O(b^m)` | `O(b^m)` | [02](/docs/lecture-notes/02-informed-and-local-search/) |
| A* | `f=g(n)+h(n)` 최소 | Yes (`f<=f(G)` 노드 유한 시) | Yes (`h` admissible 시) | 지수적 (휴리스틱 품질 의존) | 지수적 | [02](/docs/lecture-notes/02-informed-and-local-search/) |

- A*는 같은 휴리스틱에서 어떤 최적 알고리즘보다 적게 확장하는 optimally efficient. 단 최악 시간·공간은 여전히 지수적.
- 8-puzzle 평균 확장 노드(`d=12`): IDS 3,644,035 / A*(h1) 227 / A*(h2) 73. `d=24`: A*(h1) 39,135 / A*(h2) 1,641.
- h1=제자리 아닌 타일 수, h2=맨해튼 거리 합. 모든 `n`에서 h2>=h1이고 둘 다 admissible이면 h2가 h1을 지배(dominance).

## 게임·CSP

게임(adversarial search) 복잡도는 [03. Adversarial Search](/docs/lecture-notes/03-adversarial-search/), CSP 휴리스틱은 [04. Constraint Satisfaction](/docs/lecture-notes/04-csp/).

| 알고리즘 | 시간 | 공간 | 비고 |
|---|---|---|---|
| Minimax | `O(b^d)` | `O(bd)` | 완전 깊이우선, 무결점 상대 가정. MIN이 비최적이면 MAX 결과는 나빠지지 않음 |
| Alpha-Beta (최악) | `O(b^d)` | `O(bd)` | 프루닝 전혀 안 되는 수 순서. minimax와 동일 결과 보장 |
| Alpha-Beta (최선) | `O(b^(d/2))` | `O(bd)` | 이상적 move ordering. 유효 분기계수 `sqrt(b)` (체스 35 -> 약 6), 같은 시간에 약 2배 깊이 |

`a`(alpha)=경로상 MAX가 확보한 최고값, `b`(beta)=경로상 MIN이 확보한 최저값. `beta<=alpha`이면 남은 가지 prune.

CSP backtracking 개선 기법 4종:

| 기법 | 종류 | 규칙 | 목적 |
|---|---|---|---|
| MRV (Minimum Remaining Values) | 변수 선택 | 남은 합법 값이 가장 적은 변수부터 | 실패를 빨리 발견 (most constrained variable) |
| Degree heuristic | 변수 선택 (tie-break) | 미할당 변수에 제약을 가장 많이 거는 변수 | MRV 동률 해소 (most constraining variable) |
| LCV (Least Constraining Value) | 값 선택 | 남은 변수 선택지를 가장 적게 줄이는 값부터 | 성공 가능성 유지 |
| FC (Forward Checking) | 추론 | 할당마다 미할당 변수 도메인 추적, 빈 도메인 시 즉시 종료 | 필연적 실패의 조기 감지 |

MRV+Degree+LCV 결합 시 1000-queens 가능. min-conflicts(위반 제약 최소화) local search는 n-queens를 `n=10,000,000` 규모에서 거의 상수 시간에 해결.

## 분류·군집

Decision Tree와 kNN은 [05. Classification](/docs/lecture-notes/05-classification/), linkage는 [06. Clustering](/docs/lecture-notes/06-clustering/).

| 항목 | Decision Tree | kNN |
|---|---|---|
| 학습 방식 | 분할 규칙(split rule) 트리 구성 | instance-based / lazy learning |
| 학습 시점 비용 | 트리 구축 비용 | 거의 없음 (숫자 변환·스케일링만) |
| 분류 시점 비용 | 트리 따라 내려감 (빠름) | 모든 사례와 거리 계산 (`k` 클수록 느림) |
| 핵심 척도 | 순도(purity): Information Gain, Gini | 거리: 절대거리 `sum|xi-yi|`, 유클리드 `sqrt(sum(xi-yi)^2)` |
| 타깃 타입 | 범주형 1개로 제한 | 이산·실수 모두 가능 |
| 전처리 | 사전 가정 불필요, 수치·범주 모두 처리 | 정규화 `x'=(x-mu)/sigma` 필요 (정확도 향상) |
| 결정 방식 | 잎 노드의 클래스 | 최근접 `k`개 다수결(majority vote) |

계층적 군집(HAC) linkage 3종:

| 방식 | 두 클러스터 유사도 | 갱신식 | 결과 성향 |
|---|---|---|---|
| Single-link | 가장 유사한 쌍 | `max(sim)` | 크고 느슨, chaining effect, 소수 클러스터 |
| Complete-link | 가장 덜 유사한 쌍 | `min(sim)` | 작고 단단, 다수 클러스터 (IR에 적합, 생성비용 큼) |
| Group average | 쌍별 평균 | 평균 | 중간 정도 결합 |

## 학습 알고리즘

강화학습은 [07. Reinforcement Learning](/docs/lecture-notes/07-reinforcement-learning/), 신경망은 [08. Artificial Neural Network](/docs/lecture-notes/08-neural-networks/).

| 알고리즘 | 핵심 식 | 비고 |
|---|---|---|
| Q-learning 갱신 | `Q(s,a) = r(s,a) + gamma * max_a' Q(s',a')` | `gamma` 안에서 [0,1] 보상 전파 계수, 즉각보상+지연보상 |
| 학습 후 행동 선택 | `pi(s) = argmax_a Q(s,a)` | 매 스텝 Q 최대화 행동 |
| Perceptron 출력 | `y = f(sum(Wi*Xi) - theta)` | 활성화: hard limiter / linear / sigmoid |
| Widrow-Hoff (delta rule) | `Wi(t+1) = Wi(t) + alpha * [d(t)-y(t)] * Xi(t)` | 지도학습, 출력=목표면 가중치 불변 |
| Backprop 출력층 오류 | `delta_pk = (d_pk - O_pk) * O_pk * (1 - O_pk)` | sigmoid 미분 `y(1-y)` 이용 |
| Backprop 은닉층 오류 | `delta_pj = [sum_k delta_pk * W_jk] * O_pj * (1 - O_pj)` | 출력 오류를 역전파 |
| 가중치 갱신 (은닉-출력) | `W_jk(t+1) = W_jk(t) + eta * delta_pk * O_pj` | `eta`=학습률 |
| 가중치 갱신 (입력-은닉) | `W_ij(t+1) = W_ij(t) + eta * delta_pj * X_pi` | 일반화된 델타 규칙 |

- 단층 perceptron은 AND 구현 가능(`theta=0.5`, `W0=W1=0.3`)하나 XOR은 linearly non-separable이라 불가 -> 다층(MLP)으로 해결.
- Q-learning 6칸 격자 예(목표 S6, `gamma=0.5`, 보상 100): 목표 인접 100, 두 칸 50, 세 칸 25, 네 칸 12.5로 역전파 감쇠.

## 공식 빠른참조

| 이름 | 식 | 용도 |
|---|---|---|
| A* 평가함수 | `f(n) = g(n) + h(n)` | 실제 비용 + 휴리스틱 추정 |
| Admissible 조건 | `h(n) <= h*(n)` | 과대평가 금지 (낙관적 추정) |
| Entropy | `Entropy = sum_j (-pj * log2 pj)` | 불순도, 클수록 impure |
| Information Gain | `IG(S,A) = Entropy(S) - sum_v (|Sv|/|S|) * Entropy(Sv)` | 매 노드 최대 Gain 속성 선택 |
| Gini | `Gini = sum_j pj^2` | 클수록 pure (루트 0.5, 잎 0.82) |
| 유클리드 거리 | `d_E(x,y) = sqrt(sum (xi-yi)^2)` | kNN 거리 |
| 절대 거리 | `d_A(x,y) = sum |xi-yi|` | kNN 거리 |
| 정규화 | `x' = (x - mu) / sigma` | kNN 스케일링 |
| K-means centroid | `mu(c) = (1/|c|) * sum x` | 클러스터 무게중심, 복잡도 `O(IKnm)` |
| Q-learning | `Q(s,a) = r(s,a) + gamma * max_a' Q(s',a')` | 효용 추정 갱신 |
| Sigmoid | `y = 1 / (1 + e^-x)` | 활성화 함수 |
| Sigmoid 미분 | `dy/dx = y(1-y)` | backprop 오류 계산 |
| 오차함수 | `E = (1/2) * sum (d-o)^2` | gradient descent 최소화 대상 |
| Alpha-beta prune | `beta <= alpha` | 남은 가지 잘라냄 |
