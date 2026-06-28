---
title: "05. Classification"
layout: default
parent: 강의별
nav_order: 5
permalink: /docs/lecture-notes/05-classification/
---

# 05. Classification (Decision Tree · kNN)
{: .no_toc }

이질적 집단을 분할 규칙으로 균질하게 나누는 Decision Tree와, 가장 가까운 사례의 다수결로 분류하는 kNN을 다룬다.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## Decision Tree — 정의와 표현

Decision Tree는 결정의 트리를 사용하는 분류·예측 지원 도구다. 구성 요소는 다음과 같다.

- 내부 노드(internal node) = 속성(attribute) 테스트
- 가지(branch) = 속성 값
- 잎(leaf) = 분류 값(class)

규칙 ↔ 트리 변환(PlayTennis): `if (Outlook=Sunny ∧ Humidity=Normal) ∨ (Outlook=Overcast) ∨ (Outlook=Rain ∧ Wind=Weak) then Yes else No`. 이 규칙은 Outlook을 루트로, Sunny 가지 밑에 Humidity, Rain 가지 밑에 Wind를 테스트하는 트리와 동일하다.

본질: 큰 이질적(heterogeneous) 집단을 단순한 분할 규칙의 연속 적용으로 타깃 변수에 대해 더 균질한(homogeneous) 작은 그룹들로 나누는 split rule의 집합이다.

유형: Binary tree(모든 분기가 2지선다) vs N-way/ternary tree(3지 이상 분기 존재).

## Best Split과 Purity

★ Decision Tree에서 가장 중요한 것은 각 노드에서의 최적 분할(best split) 선택이며, 그 평가 척도가 순도(purity)다.

- 최적 분할(best split) = 데이터를 가장 균질한 그룹들로 가장 잘 분리하는 분할. 균질(homogeneous)이란 그룹 내 모든 데이터가 같은 타깃 값을 갖는 것.
- purity = 분할 평가 척도. 한 그룹에 여러 클래스가 섞이면 impure, 한 클래스뿐이면 pure. 최적 분할은 부분집합들의 purity를 가장 크게 증가시키는 분할이다.
- 좋은 분할은 비슷한 크기의 노드를 만들고 아주 작은 노드를 만들지 않는다.

왜 가장 중요한가: 트리 전체가 "각 노드에서 어떤 속성으로 나눌 것인가"의 반복으로 구성되므로, purity 기반 분할 기준이 트리의 구조·크기·일반화 성능을 모두 결정한다. Information Gain·Gini 등 모든 학습 알고리즘은 결국 purity 측정의 구현이다.

## 불순도 측정

불순도(impurity) 측정은 Information Gain(Entropy 기반)과 Gini로 이뤄진다.

- `Entropy = Σ −pⱼ·log₂pⱼ` (pⱼ = 클래스 j의 비율). 불순할수록 크다.
- `Information Gain(S,A) = Entropy(S) − Σᵥ (|Sᵥ|/|S|)·Entropy(Sᵥ)`. 원집합 S와 속성 A로 나눈 부분집합 Sᵥ들의 불순도 차이. 매 노드에서 Gain이 최대인 속성을 분할 속성으로 선택한다.
- `Gini = Σ pⱼ²`. 클래스 비율 제곱의 합으로 클수록 순수하다. 균형 루트 노드는 `0.5²+0.5² = 0.5`(impure), 90:10 잎은 `0.1²+0.9² = 0.82`(거의 pure). Information Gain 식의 Entropy()를 Gini()로 바꿔 끼우기만 해도 분할 속성 결정이 가능하다.
- 기타: Information Gain Ratio(IG의 변형), Chi-square 검정 기반.

### 계산 예제: Transportation 데이터

데이터 10건(4 Bus · 3 Car · 3 Train). 전체 불순도 `Entropy = −0.4log0.4 − 0.3log0.3 − 0.3log0.3 = 1.571`.

| 속성 | Information Gain | 비고 |
|---|---|---|
| Gender | 0.125 | 3B1C1T / 1B2C2T로 분할 |
| Car Ownership | 0.534 | |
| Travel Cost | 1.210 | 최대 → 루트 노드로 선택 |
| Income Level | 0.695 | |

트리 완성 과정:

- 1차 반복: Travel Cost를 루트로 분할 → Expensive→Car(3C), Standard→Train(2T)은 pure class로 잎이 되고, Cheap(4B1T)만 추가 분할 필요.
- 2차 반복(Cheap 부분집합, Entropy=0.722): 사용한 Travel Cost 속성은 제거. Gender Gain=0.322(최대), Car Ownership=0.171, Income=0.171 → Gender 선택. Male→Bus(pure), Female은 추가 분할.
- 3차 반복: 남은 2건이 Car Ownership(0→Bus, 1→Train)과 Income 모두에서 pure로 갈리므로 둘 중 아무 속성이나 사용 가능.
- 최종 트리: Travel Cost → {Car, Train, Gender → {Bus, Car Ownership → {Bus, Train}}}.

## Decision Tree 장단점

장점 5:

1. 이해하기 쉬움
2. 비즈니스 규칙 집합으로 자연스럽게 매핑
3. 다양한 실제 분류·예측 문제에 적용 가능
4. 데이터에 대한 사전 가정 불필요
5. 수치형·범주형 속성 모두 처리

단점 4:

1. 타깃(분류) 속성이 범주형이어야 함
2. 타깃 속성 1개(one class)로 제한
3. 알고리즘이 때때로 불안정(local search와 유사)
4. 수치 속성 데이터셋의 트리는 매우 복잡해질 수 있음

## kNN — 원리와 특징

kNN은 속성 벡터 공간에서 가장 가까운 k개 사례(exemplar)의 다수결(majority vote)로 분류한다. k=1이면 최근접 이웃의 클래스를 그대로 부여한다.

- Instance-based / Lazy learning: 분류 함수를 전역적으로 미리 만들지 않고 국소적으로만 근사하며, 모든 계산을 실제 분류 시점까지 미룬다.
- 모든 인스턴스는 n차원 유클리드 공간의 점(벡터 `<a1,…,an>`). 분류는 새 인스턴스 도착 시까지 지연. 벡터 비교로 분류하며, 타깃 함수는 이산·실수 모두 가능.

거리 척도:

- 절대 거리: `d_A(x,y) = Σ|xᵢ−yᵢ|`
- 유클리드 거리: `d_E(x,y) = √Σ(xᵢ−yᵢ)²`
- 범주형 속성 처리: 잘 정의된 숫자 값으로 변환. 예: Outlook={Rain=0, Overcast=1, Sunny=2}, Humidity={Normal=0, High=1} → `<Sunny,Hot,High,Weak> = <2,2,1,0>`.

## kNN — 스케일링과 계산 예제

속성 스케일링(정규화): `x' = (x−μ)/σ` (μ=평균, σ=표준편차). 각 차원 크기 차이가 거리 계산을 왜곡해 큰 축이 이웃 판정을 지배하는 효과를 완화하며, 스케일링은 kNN 분류 정확도를 자주 향상시킨다.

- 1-NN 예: X축 크기 6, Y축 크기 1인 공간에서는 "아마 white"였던 질의점이, 두 축을 정규화하면 "확실히 black"으로 바뀐다. 스케일이 이웃 판정을 좌우한다.

### 계산 예제: PlayTennis (K=3)

질의 {Sunny, Cool, High, Strong} → 변환·스케일 값 {1.18, −1.18, 1, 1}.

1. 범주형→숫자 변환 후 스케일링.
2. 각 Day와의 유클리드 거리 계산 — 예: Day1과는 `√((1.18−1.18)²+(1.18−(−1.18))²+(1−1)²+((−1)−1)²) = 3.09`.
3. 최근접 3개 = Day2(No), Day7(Yes), Day11(Yes).
4. 다수결: `t_q = (0+1+1)/3 = 2/3 ≥ 1/2` → PlayTennis = YES.

Distance-Weighted NN과 Shepard's Method:

- 거리 가중 NN: 이웃에 질의로부터의 거리에 반비례하는 가중치(보통 거리의 역수 또는 역제곱)를 부여. 가까운 이웃일수록 결과에 더 큰 영향 — 직관적으로 타당.
- Shepard's method: K=전체 인스턴스 수로 두면 모든 인스턴스가 이웃이 되어 모두가 결과에 기여. 단, 분류 시간 오버헤드가 크다.
- Voronoi diagram: 거리 가중 최근접 이웃 분류가 만드는 결정 표면(decision surface)의 예.

## 시험 포인트

- ★ Decision Tree에서 가장 중요한 것은 best split이며 평가 척도는 purity. 트리 구조·크기·성능을 모두 결정하기 때문.
- `Entropy = Σ −pⱼ·log₂pⱼ`, `Information Gain(S,A) = Entropy(S) − Σᵥ (|Sᵥ|/|S|)·Entropy(Sᵥ)`, `Gini = Σ pⱼ²` 세 식 암기.
- Transportation 예제: Travel Cost가 Gain 1.210으로 최대 → 루트. 반복 시 사용한 속성 제거.
- Information Gain 식의 Entropy()를 Gini()로 대체만 해도 분할 결정 가능.
- 장점 5 / 단점 4 개수 정확히. 단점: 타깃 범주형·1개 제한, 불안정, 수치 데이터에서 복잡.
- kNN은 instance-based / lazy learning. 학습은 거의 비용 없고 분류가 시간 소모적.
- 스케일링 `x' = (x−μ)/σ` 필요 이유: 큰 축이 이웃 판정을 지배하는 것 방지.
- K=3 추적 4단계: 변환·스케일링 → 거리 계산 → top-k 선정 → majority voting.
- Distance-Weighted NN(거리 역수 가중), Shepard's method(K=전체), Voronoi diagram 구분.

## 더 보기

- 관련: [Classification Models]({{ site.baseurl }}/docs/concepts/classification-models/)
- 이전: [04]({{ site.baseurl }}/docs/lecture-notes/04-csp/)
- 다음: [06. Clustering]({{ site.baseurl }}/docs/lecture-notes/06-clustering/)
