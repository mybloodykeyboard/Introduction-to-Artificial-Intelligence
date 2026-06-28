---
title: 압축정리
layout: default
parent: 정리노트
nav_order: 4
permalink: /docs/notes/compressed-summary/
---

# 압축정리
{: .no_toc }

비교표 · 공식 · 암기 포인트 압축본.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 공식

- A* 평가함수: `f(n) = g(n) + h(n)` (g=시작→n 실제비용, h=n→목표 추정, f=경유 전체비용 추정)
- Best-first 특수형: UCS `f=g`, Greedy `f=h`, A* `f=g+h`
- Admissible: 모든 n에서 `h(n) ≤ h*(n)` (과대평가 X, 낙관적)
- Entropy: `Entropy(S) = Σⱼ −pⱼ·log₂ pⱼ` (불순할수록 큼)
- Information Gain: `IG(S,A) = Entropy(S) − Σᵥ (|Sᵥ|/|S|)·Entropy(Sᵥ)` (최대 속성 선택)
- Gini: `Gini = Σⱼ pⱼ²` (클수록 순수, 루트 0.5²+0.5²=0.5 / 잎 0.1²+0.9²=0.82)
- Gini로 분할: `Gain = Σᵥ(|Sᵥ|/|S|)·Gini(Sᵥ) − Gini(S)` (Entropy 자리에 Gini만 교체)
- 절대 거리: `d_A(x,y) = Σ|xᵢ−yᵢ|` / 유클리드: `d_E(x,y) = √Σ(xᵢ−yᵢ)²`
- 정규화: `x' = (x−μ)/σ(x)`
- kNN 다수결: `t_q = (Σ 이웃 클래스값)/k`, `≥ 1/2`이면 양성
- centroid: `μ(c) = (1/|c|)·Σx`
- Q-learning: `Q(s,a) = r(s,a) + γ·max_a′ Q(s′,a′)` (γ∈[0,1] 보상 전파 계수)
- 학습 후 정책: `π(s) = argmax_a Q(s,a)`
- Perceptron 출력: `y = fₙ(Σ Wᵢ Xᵢ − θ)`
- Perceptron 갱신(Widrow-Hoff/delta): `Wᵢ(t+1) = Wᵢ(t) + α·[d(t)−y(t)]·Xᵢ(t)`
- Sigmoid: `y = 1/(1+e⁻ˣ)`, 미분 `∂y/∂x = y(1−y)`
- BP 출력층 오류: `δ_pk = (d_pk−O_pk)·O_pk(1−O_pk)`
- BP 은닉층 오류: `δ_pj = [Σₖ δ_pk·W_jk]·O_pj(1−O_pj)`
- BP 가중치 갱신: `W_jk(t+1)=W_jk(t)+η·δ_pk·O_pj`, `W_ij(t+1)=W_ij(t)+η·δ_pj·X_pi`
- BP 오차함수: `E = ½ Σ(d−o)²` (Gradient Descent로 최소화)
- 체스 평가함수: `Eval(s) = w1·f1(s) + … + wn·fn(s)`

기호: b=최대 분기계수, d=최적해 깊이, m=상태공간 최대 깊이, C\*=최적해 비용, ε=최소 단계비용.

## 비교표 모음

### 무정보 탐색 (완전성/시간/공간/최적성)

| 기준 | BFS | UCS | DFS | DLS | IDS |
|---|---|---|---|---|---|
| 완전성 | Yes | Yes | No | No | Yes |
| 시간 | `O(b^(d+1))` | `O(b^⌈C*/ε⌉)` | `O(b^m)` | `O(b^L)` | `O(b^d)` |
| 공간 | `O(b^(d+1))` | `O(b^⌈C*/ε⌉)` | `O(bm)` | `O(bL)` | `O(bd)` |
| 최적성 | Yes | Yes | No | No | Yes |
| Fringe | FIFO 큐 | 최소 g | LIFO 스택 | 스택+제한 L | 점증 DFS |

BFS 최적성은 단계비용 동일 시. UCS는 ε>0·유한 분기 조건.

### A* admissible vs consistent

| 항목 | Admissible | Consistent(단조) |
|---|---|---|
| 조건 | `h(n) ≤ h*(n)` | `h(n) ≤ c(n,n′)+h(n′)` |
| 보장 | tree-search A* 최적 | graph-search A* 최적 |
| 함의 | consistent ⇒ admissible | 역(admissible⇒consistent)은 성립 X |

A*는 같은 휴리스틱에서 optimally efficient. 단 최악 시간·공간은 지수적.

### Greedy vs A*

| 항목 | Greedy | A* |
|---|---|---|
| f | `f=h` | `f=g+h` |
| 완전성 | No(방문기록 없으면 루프) | Yes |
| 최적성 | No | Yes (h admissible) |
| 복잡도 | `O(b^m)` | 최악 지수적 |

### 게임 탐색 (Minimax / Alpha-Beta)

| 항목 | Minimax | Alpha-Beta |
|---|---|---|
| 결과 | MAX 최적 전략 | minimax와 동일 |
| 시간 | `O(b^d)` | 최악 `O(b^d)` / 최선 `O(b^(d/2))` |
| 공간 | `O(bd)` | `O(bd)` |
| 효과 | — | 유효 분기계수 √b (체스 35→약 6), 같은 시간 약 2배 깊이 |

α=MAX가 확보한 최고값, β=MIN이 확보한 최저값. move ordering이 효율 좌우.

### Decision Tree vs kNN

| 항목 | Decision Tree | kNN |
|---|---|---|
| 학습 방식 | eager (트리 사전 구축) | lazy (분류 시점 계산) |
| 핵심 기준 | best split / purity | 거리 + 다수결 |
| 타깃 | 범주형, 1개 | 이산·실수 모두 |
| 사전 가정 | 불필요 | 거리 척도·스케일 필요 |
| 비용 | 학습 비쌈/분류 쌈 | 학습 쌈/분류 비쌈(k 큼) |

### HAC linkage (inter-cluster similarity)

| 방식 | 두 클러스터 유사도 | 갱신식 | 결과 성향 |
|---|---|---|---|
| Single-link | 가장 유사한 쌍 | `max(sim)` | 크고 느슨, chaining |
| Complete-link | 가장 덜 유사한 쌍 | `min(sim)` | 작고 단단, 다수 |
| Group average | 쌍별 평균 | 평균 | 중간 결합 |

IR에는 complete-link이 일반적으로 더 적합(생성 비용은 더 큼).

### 머신러닝 3분류

| 분류 | 데이터 | 학습 신호 |
|---|---|---|
| Supervised | label 있음 | 목표값 |
| Unsupervised | label 없음(raw) | 유사도/구조 |
| Reinforcement | 상호작용 | reward 신호 |

### Q-learning 격자 예제 (목표 S6, γ=0.5, 보상 100)

| 목표까지 거리 | Q값 |
|---|---|
| 인접(1칸) | 100 |
| 2칸 | 50 |
| 3칸 | 25 |
| 4칸 | 12.5 |

보상이 목표에서 멀어질수록 γ배씩 감쇠하며 역전파.

### 핵심 수치

| 항목 | 값 |
|---|---|
| 8-puzzle A* d=12 (IDS/h1/h2) | 3,644,035 / 227 / 73 |
| 8-puzzle A* d=24 (h1/h2) | 39,135 / 1,641 |
| IDS vs BFS 노드(b=10,d=5) | 약 123,450 vs 111,110 (11% 차) |
| 8-queens hill-climbing | 14% 성공, 86% 지역최소, 성공 시 평균 4스텝 |
| 8-queens random restart | (0.86)ⁿ<0.01 → 약 31회, 약 124스텝 |
| min-conflicts n-queens | n=10,000,000 거의 상수 시간 |
| k-means 복잡도 | `O(IKnm)` 선형 |

## 한 줄 정의

- **Admissible**: 목표까지 비용을 절대 과대평가하지 않는 낙관적 휴리스틱 (`h ≤ h*`).
- **Dominance**: 모든 n에서 `h2 ≥ h1`이고 둘 다 admissible이면 h2가 h1 지배 → 탐색에 더 좋음.
- **Alpha-beta cut**: 어떤 노드 값이 조상의 α·β 기준으로 선택 불가 확정 시 남은 가지 prune (β≤α이면 중단).
- **MRV**: 남은 합법 값이 가장 적은 변수부터 할당(실패 조기 발견).
- **Degree heuristic**: MRV 동률 시 남은 변수에 제약을 가장 많이 거는 변수 선택.
- **LCV**: 남은 변수 선택지를 가장 덜 줄이는 값부터 시도.
- **Forward checking**: 할당 시 미할당 변수 도메인 추적, 비면 즉시 백트랙.
- **Min-conflicts**: 위반 제약 수를 최소화하는 값 선택 = 위반 총수의 hill-climbing.
- **Lazy learning**: 전역 함수 미리 안 만들고 국소 근사, 계산을 분류 시점까지 지연(kNN).
- **Cluster hypothesis**: 같은 클러스터 문서는 정보요구에 유사하게 반응(recall 개선).
- **Relaxed problem**: 행동 제약 완화 문제, 그 최적해 비용이 원문제의 admissible 휴리스틱.
- **Optimally efficient**: 같은 휴리스틱에서 A*보다 적은 노드를 확장하는 최적 알고리즘 없음.
- **Off-policy(Q-learning)**: 행동(임의 선택·시행착오)과 평가(max Q) 정책이 다름.
- **On-policy**: 행동 정책과 학습·평가 정책이 동일.
- **Best split / purity**: 데이터를 가장 균질(homogeneous)하게 분리, purity 증가 최대 분할.
- **Voronoi diagram**: 거리 가중 최근접 이웃 분류가 만드는 결정 표면 예.
- **Shepard's method**: K=전체 인스턴스 수로 둔 거리 가중 NN(분류 오버헤드 큼).
- **Chaining effect**: single-link에서 소수의 크고 느슨한 클러스터로 불공평 성장.

## 자주 틀리는 포인트

- **완전성 ≠ 최적성**: 완전(해를 찾음)해도 최적(최소비용)은 별개. DFS는 둘 다 No, IDS는 둘 다 Yes.
- **consistent ⇒ admissible (역은 X)**: consistent면 admissible이지만, admissible이라고 consistent인 건 아님.
- **k-means 종료조건 동치 아님**: "모든 클러스터 불변"과 "모든 centroid 불변"은 동치 아님 — 할당 같아도 centroid 다를 수 있음.
- **XOR 단층 불가**: XOR은 linearly non-separable, 단층 퍼셉트론(직선 1개) 분리 불가. AND는 가능(θ=0.5, W0=W1=0.3). 해결은 다층(2~3층).
- **single-link chaining**: single-link은 한 멤버만 임계 넘으면 병합 → 크고 느슨. complete-link은 모든 멤버가 임계 넘어야 → 작고 단단.
- **BFS 최적성 조건부**: BFS 최적성은 단계비용 동일할 때만. 비용 다르면 UCS 필요.
- **Greedy 불완전**: 방문기록 없으면 루프, admissible해도 비최적.
- **A* 최적성은 tree-search 기준**: graph-search 최적은 consistent 필요.
- **Information Gain은 최대 선택, Entropy는 작을수록 순수 / Gini는 클수록 순수** (방향 반대 주의).
- **kNN 스케일링 영향**: 정규화 전후로 분류 결과가 바뀔 수 있음(큰 축이 이웃 판정 지배).
- **delayed reward**: Q-learning은 학습 시점 평가 미완 → 당시 최적 action이 실제 최적 아닐 수 있음.
- **backprop 생물학적 불일치**: 상위 오류를 하위로 역전파하는 것은 실제 뉴런과 불일치(하위는 상위 목표 모름).
