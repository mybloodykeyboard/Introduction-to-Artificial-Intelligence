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

### Q1. 지도 색칠 (Map Coloring) — backtracking 효율화 + forward checking 추적

**문제**
- 호주 지도 색칠 CSP가 제시됨.
- **Improving backtracking efficiency 3가지 전략**을 적용했을 때 탐색이 어떻게 진행되는지를, **forward checking** 방식으로 각 단계의 도메인 변화를 **그려서 표현**.

**풀이 포인트**
- 변수: `WA, NT, Q, NSW, V, SA, T` / 도메인: `{red, green, blue}` / 제약: 인접 지역은 색이 달라야 함.
- 3가지 전략:
  - **MRV** (Minimum Remaining Values): 남은 합법 값이 가장 적은 변수부터 → 실패를 빨리 발견.
  - **Degree heuristic**: MRV 동률일 때 다른 미할당 변수에 제약을 가장 많이 거는 변수 선택. 초기엔 모든 도메인이 3으로 동률이므로, 인접 5개인 **SA가 먼저 선택**됨.
  - **LCV** (Least Constraining Value): 값은 이웃의 선택지를 가장 적게 줄이는 값부터.
- **Forward checking**: 할당할 때마다 인접 미할당 변수의 도메인에서 충돌 색을 제거하고, 어떤 변수의 도메인이 비면 즉시 backtrack.
- 채점 포인트: 각 할당 단계마다 **변수별 남은 도메인을 표/그림으로 갱신**해 보여주는 것.

**풀이 (구체 forward-checking 추적 — 색 배정은 예시)**

인접: SA는 WA·NT·Q·NSW·V 5개와 인접(degree 최대), T는 고립. 초기 도메인 모두 `{R,G,B}`. MRV 동률 → Degree로 **SA 먼저**.

| 단계 | 할당 | forward checking 후 남은 도메인 |
|---|---|---|
| 1 | SA=R | WA·NT·Q·NSW·V = `{G,B}`, T=`{R,G,B}` |
| 2 | WA=G | NT=`{B}` (G 제거) |
| 3 | NT=B (MRV) | Q=`{G}` (B 제거) |
| 4 | Q=G | NSW=`{B}` (G 제거) |
| 5 | NSW=B | V=`{G}` (B 제거) |
| 6 | V=G | — |
| 7 | T=R | — (어디와도 비인접) |

→ 도메인이 빈 적 없어 **backtrack 없이** 완료. 결과 예: `SA=R, WA=G, NT=B, Q=G, NSW=B, V=G, T=R` (모든 인접쌍 색 상이).

- 방법론: [04. CSP]({{ site.baseurl }}/docs/lecture-notes/04-csp/)

### Q2. 4-Queens (보류)

- 4-Queens 관련 문제가 출제됨 — 세부는 **보류**.
- **4-Queens example 참고할 것**: [04. CSP]({{ site.baseurl }}/docs/lecture-notes/04-csp/)의 forward checking 추적 예제(해: X1=2, X2=4, X3=1, X4=3) 및 강의 PDF `4.ConstraintSatisfactionProblem`의 4-Queens 예시.

> 기억나는 대로 알려주면 정확히 채움.

## 05. Classification (Decision Tree · kNN)

### Q1. kNN 분류 (KNN Example 유형)

**문제**
- 데이터셋이 제시되고, 질의(query) 인스턴스가 **어떤 클래스로 분류**되는지 적기.
- 강의 **kNN Example**(PlayTennis, K=3) 수준.

**풀이 포인트**
- 범주형 속성 → 숫자 변환, 필요 시 정규화 `x' = (x−μ)/σ`.
- 거리: 유클리드 `√Σ(xᵢ−yᵢ)²` 또는 절대거리 `Σ|xᵢ−yᵢ|`.
- 최근접 K개 선정 → **다수결(majority vote)**.
- 구체 예시 (강의 PlayTennis, K=3):
  - 범주형 변환 후 스케일링한 질의 = `{1.18, −1.18, 1, 1}`.
  - 거리 계산(예: Day1) `√((1.18−1.18)² + (1.18−(−1.18))² + (1−1)² + (−1−1)²) = √(0+5.57+0+4) ≈ 3.09`.
  - 전 Day와 거리 → 최근접 3개 = Day2(No)·Day7(Yes)·Day11(Yes) → `t_q = (0+1+1)/3 = 2/3 ≥ 1/2` → **YES**.
- 채점 포인트: 변환·스케일링 → 거리 계산 → top-k → 다수결 4단계 명시.
- 방법론: [05. Classification]({{ site.baseurl }}/docs/lecture-notes/05-classification/)

### Q2. Decision Tree — Information Gain 계산 (Information Gain Example 유형)

**문제**
- 데이터셋이 제시되고, **Information Gain을 계산**해 분할 속성을 고르고 classification.
- 강의 **Information Gain Example**(Transportation 데이터) 수준.

**풀이 포인트**
- `Entropy = Σ −pⱼ·log₂pⱼ`, `Information Gain(S,A) = Entropy(S) − Σᵥ (|Sᵥ|/|S|)·Entropy(Sᵥ)`.
- 매 노드에서 **Gain 최대 속성**을 분할 속성으로 선택 (Gini `Σ pⱼ²`로 대체 가능).
- 구체 예시 (강의 Transportation, 10건 = 4 Bus·3 Car·3 Train):
  - 전체 `Entropy = −0.4·log₂0.4 − 0.3·log₂0.3 − 0.3·log₂0.3 ≈ 1.571`.
  - 속성별 Information Gain: **Travel Cost 1.210**(최대) → 루트, Income 0.695, Car Ownership 0.534, Gender 0.125.
  - 1차 분할(Travel Cost): Expensive→Car(pure), Standard→Train(pure)은 잎. Cheap(4 B·1 T, Entropy 0.722)만 재귀 → 2차에서 Gender(Gain 0.322) 선택 → Male→Bus(pure) ...
- 채점 포인트: Entropy·Gain 식과 단계별 계산값, pure 여부 판정.
- 방법론: [05. Classification]({{ site.baseurl }}/docs/lecture-notes/05-classification/)

## 06. Clustering

### Q1. k-means (k=2) — 시드 배치에 따른 수렴

**문제**
- 직사각형 꼭짓점에 점 4개(위 두 개 white `ㅇ`, 아래 두 개 black `ㅁ`). 초기 시드 `x` 2개를 **왼쪽 두 점 근처에 위/아래로** 배치했을 때 k=2 k-means가 어떻게 수렴하는지.

```
ㅇ   x          ㅇ
ㅁ   x          ㅁ
```

**풀이 (구체 계산 — 좌표는 이해용 예시)**

좌표: 흰점 `TL(0,2)`, `TR(4,2)` / 검은점 `BL(0,0)`, `BR(4,0)`. 초기 시드는 왼쪽에 위/아래로 → `시드1=(1,2)`, `시드2=(1,0)`.

**1차 할당** (각 점을 더 가까운 시드로, 유클리드 거리):

| 점 | 시드1(1,2)까지 | 시드2(1,0)까지 | 배정 |
|---|---|---|---|
| TL(0,2) | `√(1²+0²)=1` | `√(1²+2²)=√5≈2.24` | 시드1 |
| TR(4,2) | `√(3²+0²)=3` | `√(3²+2²)=√13≈3.61` | 시드1 |
| BL(0,0) | `√(1²+2²)=√5≈2.24` | `√(1²+0²)=1` | 시드2 |
| BR(4,0) | `√(3²+2²)=√13≈3.61` | `√(3²+0²)=3` | 시드2 |

→ 클러스터1 = {TL, TR}, 클러스터2 = {BL, BR}.

**centroid 갱신** (할당된 점들의 평균 `μ = (1/|c|)·Σx`):
- `c1 = ((0+4)/2, (2+2)/2) = (2, 2)`
- `c2 = ((0+4)/2, (0+0)/2) = (2, 0)`

**2차 할당** (새 centroid 기준):

| 점 | c1(2,2)까지 | c2(2,0)까지 | 배정 |
|---|---|---|---|
| TL(0,2) | `2` | `√(2²+2²)=√8≈2.83` | c1 |
| TR(4,2) | `2` | `√8≈2.83` | c1 |
| BL(0,0) | `√8≈2.83` | `2` | c2 |
| BR(4,0) | `√8≈2.83` | `2` | c2 |

→ 할당 변화 없음 → **수렴**. 최종 centroid `(2,2)`, `(2,0)`.

**k=2 결과**: `{TL, TR}` = 흰점 2개 / `{BL, BR}` = 검은점 2개 (행 단위로 분리).

**seed sensitivity (대조)**: 시드를 `(0,2)`, `(4,2)`처럼 **같은 윗행**에 찍으면 좌/우(열)로 갈려 `{TL,BL}` / `{TR,BR}`로 수렴 — 결과가 초기 시드에 좌우됨. 해결: 서로 먼 점을 시드로(휴리스틱), 또는 여러 번 재시도.

- 방법론: [06. Clustering]({{ site.baseurl }}/docs/lecture-notes/06-clustering/)

### Q2. HAC — single-link vs complete-link 차이와 계산

**문제**
- 클러스터 간 유사도 행렬이 주어지고, **single-link**와 **complete-link**로 병합 순서를 추적하고 **차이를 비교**.

**풀이 포인트**
- **Single-link**: 두 클러스터 유사도 = 가장 유사한 쌍, 갱신 `sim(AB,X) = max(sim(A,X), sim(B,X))` → chaining effect, 크고 느슨한 소수 클러스터.
- **Complete-link**: 유사도 = 가장 덜 유사한 쌍, 갱신 `sim(AB,X) = min(...)` → 작고 단단한 다수 클러스터.
- 매 단계 행렬에서 **가장 큰 유사도 쌍을 병합** → 갱신식으로 행렬 축소 → 반복.
**풀이 (구체 — 강의 A~F 예제)**

같은 유사도 행렬에서 갱신식만 바꾸면 병합 순서·결과가 달라짐.

- **Single-link** (`sim = max`): `AF(0.9)` → `(AF)E(0.8)` → `(AEF)B(0.8)` → `(ABEF)D(0.6)` → `(ABDEF)C(0.5)`.
  - 임계값 절단: 0.85 → `{AF}{E}{B}{D}{C}`, 0.65 → `{AFEB}{D}{C}`, 0.55 → `{AFEBD}{C}`.
- **Complete-link** (`sim = min`): `AF(0.9)` → `BE(0.7)` → `BCE(0.4)` → `BCDE(0.3)` → `(AF)(BCDE)(0.1)`.
  - 임계값 절단: 0.85 → `{AF}{B}{E}{C}{D}`, 0.65 → `{AF}{BE}{C}{D}`, 0.35 → `{AF}{BCE}{D}`.
- 차이: single-link은 chaining으로 한 덩어리가 길게 자라고(크고 느슨), complete-link은 작고 단단한 다수 클러스터.

- 채점 포인트: max/min 갱신식 명시 + 단계별 행렬 축소 + 두 방식 결과 성향 비교.
- 방법론: [06. Clustering]({{ site.baseurl }}/docs/lecture-notes/06-clustering/), [Clustering & Linkage]({{ site.baseurl }}/docs/concepts/clustering-and-linkage/)

## 07. Reinforcement Learning

### Q1. Q-learning 격자 — 경로별 Q 전파 계산

**문제**
- 강의 RL example과 같은 격자가 제시되고, **각 transition(`a12, a21, a22, ...`)에 즉각 보상 값이 주어짐** — 목표에만 100을 주는 형태가 아니라 대략 **4~20 범위의 값이 골고루** 배치됨.
- 주어진 보상으로 모든 `Q(s,a)`를 채우고, 특정 경로를 따라 학습 과정을 계산.
- 구체적으로 한 경로 **S4 → S1 → S2 → S5 → S6 → S3** 를 먼저 계산하고, **또 다른 경로** 하나를 더 계산해 **최종 Q값 변화**를 제시.

**풀이 포인트**
- 갱신식: `Q(s,a) = r(s,a) + γ·max_a′ Q(s′,a′)`, γ=0.5. 모든 Q는 0으로 초기화.
- episode마다 경로를 따라가며 각 step에서 갱신, `s = s′`로 이동.
- 즉각 보상이 매 transition에 있으면 각 `Q(s,a)`는 `그 행동의 보상 r + γ × 다음 상태의 최선 Q`로 채워지고, 여러 episode를 거치며 목표 쪽에서 앞으로 값이 안정화됨.
- 채점 포인트: 각 step의 `r + γ·max Q(s′,·)` 계산값 + 두 경로 누적 후 최종 Q 테이블.
- 방법론: [07. Reinforcement Learning]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/)

**예제 (dense reward 버전 — 직접 계산용. 예제임)**

상태 `S1`(시작) ~ `S6`(목표·종료), γ=0.5. 화살표는 가능한 행동, 각 행동에 즉각 보상 r(4~20)이 주어짐:

```
 S1 →  S2 →  S3
 ↓     ↓     ↓
 S4 →  S5 →  S6 (goal·종료)
```

| transition | r | | transition | r |
|---|---|---|---|---|
| S1→S2 | 8 | | S2→S5 | 10 |
| S2→S3 | 6 | | S4→S5 | 7 |
| S1→S4 | 5 | | S3→S6 | 12 |
| S5→S6 | 9 | | | |

갱신식 `Q(s,a) = r + γ·max_a′ Q(s′,a′)`, γ=0.5, 모든 Q=0 초기화. S6는 종료(`max Q = 0`).

**경로 1 학습: S1→S2→S5→S6** (방문 순서대로 갱신)

| episode | 갱신 step | 계산 | 결과 |
|---|---|---|---|
| 1 | S5→S6 | `9 + 0.5·0` | Q(S5,→S6)=9 |
| 1 | S2→S5 | `10 + 0.5·max Q(S5,·)=10+0.5·0` | Q(S2,→S5)=10 |
| 1 | S1→S2 | `8 + 0.5·max Q(S2,·)=8+0.5·0` | Q(S1,→S2)=8 |
| 2 | S2→S5 | `10 + 0.5·9` | Q(S2,→S5)=14.5 |
| 2 | S1→S2 | `8 + 0.5·10` | Q(S1,→S2)=13 |
| 3 | S1→S2 | `8 + 0.5·14.5` | Q(S1,→S2)=15.25 (수렴) |

> 한 episode 안에서 뒤쪽(목표 쪽) step부터 값이 생기고, episode를 반복하면 그 값이 앞쪽으로 전파됨.

**경로 2 학습: S1→S4→S5→S6 (+ S1→S2→S3→S6)** — 나머지 행동 채우기

- S4→S5: `7 + 0.5·max Q(S5,·) = 7 + 0.5·9 = 11.5`
- S1→S4: `5 + 0.5·max Q(S4,·) = 5 + 0.5·11.5 = 10.75`
- S2→S3: `6 + 0.5·max Q(S3,·) = 6 + 0.5·12 = 12`
- S3→S6: `12 + 0.5·0 = 12`

**최종 Q 테이블**

| 상태 | 행동 | Q | | 상태 | 행동 | Q |
|---|---|---|---|---|---|---|
| S1 | →S2 | **15.25** | | S2 | →S5 | **14.5** |
| S1 | →S4 | 10.75 | | S2 | →S3 | 12 |
| S4 | →S5 | 11.5 | | S3 | →S6 | 12 |
| S5 | →S6 | 9 | | | | |

**최적 정책** `π(s)=argmax_a Q(s,a)`: S1→S2(15.25) → S2→S5(14.5) → S5→S6(9). 즉 **S1→S2→S5→S6**, 가치 15.25. (S1→S4 경로는 10.75로 열위.)

> 위는 예제. 실제 시험 격자(상태 연결·보상값·경로)를 그대로 주면 그 숫자로 같은 절차를 적용해 칸별 Q를 채워줌.

## 08. Artificial Neural Network

### Q1. (5지선다) Backpropagation과 유사한 개념

**문제 (기억 기반)**
- "backpropagation과 유사한 내용/개념은?" 류의 **5지선다**.
- 기억나는 보기: **UCS**, **local search** (나머지 3개 미상).

**정답 (추정): UCS**
- UCS가 정답일 확률이 높음(확인된 견해).
- 가능한 근거: UCS(= Dijkstra)와 backpropagation 모두 값을 단계적으로 전파·갱신하는 **dynamic programming** 성격을 공유. UCS는 누적비용 `g(n)`을 전방으로, backprop은 오차 δ를 후방으로 전파.
- 참고: local search도 후보로 보일 수 있음 — backprop의 gradient descent는 weight 공간의 local search라 local minima에 빠질 수 있음. 다만 정답 견해는 **UCS** 쪽.

> 나머지 보기 3개나 정확한 문구가 기억나면 알려주면 확정.

### 그 외 꼬리문제 (추정 — 미확정)

- **Perceptron**으로 AND는 가능, **XOR는 불가**(linearly non-separable)인 이유를 식으로.
- **Backpropagation** 순전파·역전파 한 스텝 계산(sigmoid 미분 `y(1−y)`, δ, 가중치 갱신).

> 위는 추정. 실제 출제 문항이 기억나면 알려주면 확정.
