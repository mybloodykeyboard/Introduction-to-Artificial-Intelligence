---
title: Clustering & Linkage
layout: default
parent: 컨셉별
nav_order: 6
permalink: /docs/concepts/clustering-and-linkage/
---

# Clustering & Linkage
{: .no_toc }

가장 흔한 unsupervised learning. flat 방식 k-means와 계층적 HAC가 양대 축이고, HAC는 클러스터 결합 기준(linkage)에 따라 결과 성향이 갈린다.

## 한눈에

- k-means: K개 시드 선택 → 가장 가까운 centroid에 할당 → centroid 갱신 → 수렴까지 반복. 단순하고 빠름.
- HAC(bottom-up): 문서 1개=1클러스터에서 시작해 가장 유사한 쌍을 계속 합쳐 1개 남을 때까지.
- linkage: 두 클러스터의 유사도를 어떻게 정의하느냐 — single / complete / group-average.

| 방식 | 두 클러스터 유사도 | 갱신식 | 성향 |
|---|---|---|---|
| Single-link | 가장 유사한 쌍 | `max(sim)` | 크고 느슨, chaining |
| Complete-link | 가장 덜 유사한 쌍 | `min(sim)` | 작고 단단, 다수 |
| Group average | 쌍별 평균 | 평균 | 중간 결합 |

## 핵심 포인트

- k-means 복잡도 `O(IKnm)` — 완전 선형. 거리 `O(m)`, 재할당 `O(Knm)`, centroid `O(nm)`, I회 반복.
- 시드 민감성. 나쁜 시드는 느린 수렴 또는 sub-optimal 결과. 해결은 (1) 기존 시드에서 가장 먼 문서 선택 (2) 여러 시드로 반복 — 본질이 hill-climbing.
- 종료조건 3가지(고정 횟수 / 클러스터 불변 / centroid 불변)에서 뒤 둘은 동치 아님. 할당이 같아도 centroid가 다를 수 있음.
- chaining effect: single-link은 멤버 하나만 가까워도 계속 흡수돼 소수의 크고 느슨한 클러스터를 만듦(불공평한 성장).
- complete-link은 모든 멤버가 해당 유사도 수준에서 서로 닮음을 보장 → IR에 더 적합하나 생성 비용이 큼.

## 자세히

강의노트: [06]({{ site.baseurl }}/docs/lecture-notes/06-clustering/)
