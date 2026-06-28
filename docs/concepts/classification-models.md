---
title: Classification Models
layout: default
parent: 컨셉별
nav_order: 5
permalink: /docs/concepts/classification-models/
---

# Classification Models
{: .no_toc }

분류 모델 두 갈래: 규칙을 미리 학습해 트리로 굳히는 Decision Tree, 분류 시점까지 계산을 미루는 instance-based kNN. 학습 시점과 데이터 가정이 정반대다.

## 한눈에

- Decision Tree: 속성으로 데이터를 균질한 그룹으로 쪼개는 split rule의 집합. 학습 때 트리를 만들어 둠(eager).
- kNN: 새 인스턴스가 오면 벡터공간에서 가까운 k개 사례의 다수결로 분류. 학습은 거의 안 하고 분류 때 계산(lazy).

| 구분 | Decision Tree | kNN |
|---|---|---|
| 학습 시점 | eager — 학습 때 트리 구축 | lazy — 분류 때 거리 계산 |
| 분류 기준 | 노드별 purity 기반 split | 최근접 k개 majority vote |
| 데이터 가정 | 사전 가정 불필요, 수치형·범주형 모두 | 유클리드 공간의 점, 스케일링 필요 |
| 핵심 척도 | Information Gain, Gini | 거리(절대/유클리드) |
| 장점 | 이해 쉬움, 규칙으로 매핑 | 노이즈·복잡한 타깃에 강함 |
| 단점 | 타깃 범주형 1개, 수치형서 트리 복잡 | k 크면 분류 느림 |

## 핵심 포인트

- Purity가 트리의 전부. 한 그룹에 한 클래스만 있으면 pure, 섞이면 impure. best split은 부분집합 purity를 가장 크게 올리는 분할.
- 불순도 측정 두 가지. Entropy는 `Σ −pⱼ·log₂ pⱼ`(불순할수록 큼), `Information Gain = Entropy(S) − Σ (|Sᵥ|/|S|)·Entropy(Sᵥ)`로 Gain 최대 속성 선택. Gini는 `Σ pⱼ²`(클수록 순수)로 끼워 넣어도 됨.
- kNN은 instance-based / lazy. 함수를 전역으로 안 만들고 국소 근사만, 모든 계산을 분류 시점까지 미룸.
- 스케일링이 정확도 좌우. `x' = (x−μ)/σ`로 정규화하면 차원 크기 차이의 영향이 줄어 이웃 판정이 바뀜.
- 거리 가중 NN은 가까운 이웃에 더 큰 가중치(거리 역수/역제곱). k=전체로 두면 Shepard's method.

## 자세히

강의노트: [05]({{ site.baseurl }}/docs/lecture-notes/05-classification/)
