---
title: "06. Clustering"
layout: default
parent: 강의별
nav_order: 6
permalink: /docs/lecture-notes/06-clustering/
---

# 06. Clustering (k-means · 계층적 군집화)
{: .no_toc }

객체 집합을 유사한 것끼리 그룹화하는 비지도 학습 — k-means(flat)와 HAC(계층적)를 다룬다.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- 클러스터링 = 객체 집합을 유사한 객체들의 클래스로 그룹화. 같은 클러스터 내 문서는 유사하게, 다른 클러스터의 문서는 비유사하게 묶음.
- 가장 흔한 비지도 학습(unsupervised learning).
  - 비지도: 분류(label)가 주어지지 않은 raw data로부터 학습.
  - 지도: 분류가 주어진 데이터로부터 학습.
- 응용: Yahoo! 토픽 계층, Google News 자동 군집, 문서 컬렉션 시각화(contour map), 검색결과 주제별 내비게이션.
  - IR recall 개선 — cluster hypothesis: 같은 클러스터의 문서는 정보요구에 유사하게 반응. 질의에 맞은 문서 D와 같은 클러스터의 문서도 반환(`car` 질의가 `automobile` 문서도 반환).
- 문서 클러스터링의 이슈
  - 표현·유사도: 벡터공간 vs 불리언 모델, cosine 유사도 vs 유클리드 거리. 이상적으론 의미적 유사성이나 실무는 통계적 유사성(term vector + cosine/Euclidean).
  - 클러스터 수: 사전 고정 vs 데이터 주도. 너무 크거나 작은 trivial cluster는 회피 — 내비게이션 시 추가 클릭만 낭비하고 문서 집합을 좁히지 못함.

## K-means

flat 알고리즘. centroid(무게중심) `μ(c) = (1/|c|)·Σx` 기반. 재할당은 각 클러스터 centroid까지의 유클리드 거리 기준.

알고리즘 4단계

1. K개 무작위 문서를 시드로 선택.
2. 각 문서를 가장 가까운 시드의 클러스터에 할당.
3. 각 클러스터의 centroid로 시드 갱신.
4. 수렴(또는 종료조건)까지 2~3 반복.

종료 조건 3가지

- 고정 횟수.
- 모든 클러스터 불변.
- 모든 centroid 위치 불변.

두 번째와 세 번째는 동치가 아님(No) — 할당이 같아도 centroid가 다른 경우가 존재.

수렴 근거: K-means는 EM(Expectation Maximization)의 특수 사례이고 EM은 안정 상태 수렴이 증명됨. 실제 반복 횟수는 보통 예상보다 훨씬 작음.

시간 복잡도 `O(IKnm)` — 완전 선형. 거리 계산 O(m)(m=차원), 재할당 O(Knm), centroid 계산 O(nm), I회 반복.

초기 시드 민감성

- 나쁜 시드는 느린 수렴 또는 sub-optimal 수렴 유발. 예: B,E 시드 → {ABC}{DEF}, D,F 시드 → {ABDE}{CF}.
- 해결책
  1. 휴리스틱으로 좋은 시드 선택 — 이미 선택된 시드들에서 가장 먼 문서를 다음 시드로.
  2. 서로 다른 시드로 여러 번 시도 — 본질이 hill-climbing이기 때문.

## 계층적 군집화 (HAC)

- Divisive(top-down): 전체 컬렉션을 하나의 클러스터로 보고 k-means 같은 flat 알고리즘으로 재귀적으로 쪼갬.
- Agglomerative(bottom-up, HAC): 단독 클러스터에서 시작해 가장 유사한 쌍을 합쳐 올라감.

HAC 4단계

1. N개 문서 각각을 단독 클러스터로.
2. 모든 쌍별 클러스터 간 유사도 계산.
3. 가장 유사한 쌍을 결합해 새 클러스터 형성.
4. 클러스터가 1개 남을 때까지 2로 반복.

## Linkage 비교

inter-cluster similarity 3가지 기준.

| 방식 | 두 클러스터의 유사도 | 갱신식 | 결과 성향 |
|---|---|---|---|
| Single-link | 가장 유사한 쌍 | `max(sim)` | 크고 느슨, chaining |
| Complete-link | 가장 덜 유사한 쌍 | `min(sim)` | 작고 단단, 다수 |
| Group average | 쌍별 평균 | 평균 | 중간 정도 결합 |

Single-link 추적 예제 (A~F, 갱신식 `sim(AF,X)=max(sim(A,X),sim(F,X))`)

- AF:0.9 → (AF)E:0.8 → (AEF)B:0.8 → (ABEF)D:0.6 → (ABDEF)C:0.5.
- 임계값 절단: 0.85 → {AF,E,B,D,C}, 0.65 → {AFEB,D,C}, 0.55 → {AFEBD,C}.

Complete-link 추적 예제 (갱신식 `sim(AF,x)=min(sim(A,x),sim(F,x))`)

- AF(0.9) → BE(0.7) → BCE(0.4) → BCDE(0.3) → 최종 (AF)(BCDE)(0.1).
- 임계값 절단: 0.85 → {AF,B,E,C,D}, 0.65 → {AF,BE,C,D}, 0.35 → {AF,BCE,D}.

비교분석

- Single-link: chaining effect로 소수의 크고 느슨한 클러스터(불공평한 성장). 멤버는 같은 클러스터의 최소 한 멤버와 임계 유사도를 넘으면 됨.
- Complete-link: 모든 멤버가 명시된 유사도 수준에서 서로 닮음을 보장 → 다수의 작고 단단한 클러스터.
- IR에는 일반적으로 complete-link이 더 적합하나 생성 비용이 더 큼.

## 시험 포인트

- 비지도 vs 지도 구분과 cluster hypothesis로 recall 개선 설명.
- K-means centroid 식 `μ(c) = (1/|c|)·Σx`와 4단계 절차.
- 종료조건 3가지, 특히 "클러스터 불변"과 "centroid 불변"은 동치가 아님(No).
- 복잡도 `O(IKnm)` — 완전 선형, 각 항(O(m)/O(Knm)/O(nm)/I회) 근거.
- 초기 시드 민감성 + 해결책 2가지(가장 먼 문서 휴리스틱, 다회 시도/hill-climbing).
- Linkage 추적 문제는 갱신식(max/min) 명시 + 단계별 행렬 축소 + 임계값별 절단이 채점 포인트.

## 기출로 보는 핵심 직관

기출 연결: ① k=2 k-means에서 **초기 시드 위치에 따라 수렴 결과가 달라짐**(centroid 계산 → 수렴), ② single-link vs complete-link 병합 추적·비교. [26전기 기출]({{ site.baseurl }}/docs/notes/past-exams-26/) 참고.

핵심 깨달음 (k-means)

- 본질이 **hill-climbing이라 초기 시드에 결과가 좌우됨(seed sensitivity)**.
- centroid는 할당된 점들의 **평균**으로 이동(`μ(c)=(1/|c|)·Σx`), 할당이 더 안 바뀌면 수렴.
- 같은 4점이라도 시드를 어디 찍느냐로 행 분리/열 분리가 갈림 → 좋은 시드(서로 먼 점)·여러 번 재시도로 완화.

핵심 깨달음 (linkage)

| 방식 | 갱신식 | chaining | 결과 성향 |
|---|---|---|---|
| Single-link | `max`(가장 가까운 쌍) | 발생 | 길고 느슨한 덩어리 |
| Complete-link | `min`(가장 먼 쌍) | 억제 | 작고 단단 |

- 갱신식 한 글자(`max` ↔ `min`) 차이가 결과 성향을 가름.

## 더 보기

관련: [Clustering & Linkage]({{ site.baseurl }}/docs/concepts/clustering-and-linkage/) / 이전: [05]({{ site.baseurl }}/docs/lecture-notes/05-classification/) / 다음: [07. Reinforcement Learning]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/)
