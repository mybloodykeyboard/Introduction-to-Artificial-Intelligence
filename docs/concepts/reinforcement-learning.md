---
title: Reinforcement Learning
layout: default
parent: 컨셉별
nav_order: 7
permalink: /docs/concepts/reinforcement-learning/
---

# Reinforcement Learning
{: .no_toc }

label도 raw data도 아닌 reward로 배우는 세 번째 갈래. 에이전트가 시행착오로 보상을 받아 행동을 강화하며, Q-learning이 대표 알고리즘이다.

## 한눈에

- Supervised: label 있는 데이터로 학습 / Unsupervised: label 없는 raw data로 학습.
- Reinforcement: 에이전트가 action에 대한 reward를 받아 점점 효율적으로 행동을 강화 — learn via experiences.
- Q-learning: 임의 action을 실행해 reward를 받고, 그 reward를 상태들로 전파해 효용함수 Q를 학습.

## 핵심 포인트

- 강화학습 3특징: (1) delayed reward — 당장 적합한지 모름 (2) 현재 action 결과를 정확히 몰라도 무관 (3) life-long learning에 활용.
- Q 갱신식: `Q(s,a) = r + γ·max Q(s′,a′)`. 즉각 보상(immediate) + 다음 상태의 잠재 보상 최대값(delayed). `γ∈[0,1]`은 보상 전파 계수.
- 학습 후 행동 선택은 정책 `π(s) = argmax Q(s,a)` — 매 스텝 Q 최대화 행동으로 이동.
- 알고리즘: 모든 (s,a)에 Q=0 초기화 → 임의 action 실행 → reward 수령 → s′ 감지 → Q 갱신 → s=s′ 반복.
- 보상 전파: 목표에서 멀어질수록 `γ`배씩 감쇠하며 역방향으로 퍼짐. 학습 시점엔 평가 미완이라 당시 최적이 실제 최적이 아닐 수 있음.

## 자세히

강의노트: [07]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/)
