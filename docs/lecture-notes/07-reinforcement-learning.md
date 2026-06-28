---
title: "07. Reinforcement Learning"
layout: default
parent: 강의 노트
nav_order: 7
permalink: /docs/lecture-notes/07-reinforcement-learning/
---

# 07. Reinforcement Learning (강화학습)
{: .no_toc }

보상 신호로 시행착오를 통해 정책을 학습. 모델(전이·보상)을 모를 때.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 기반: MDP

- **구성**: 상태 S, 행동 A, 전이 P(s'|s,a), 보상 R, 할인율 γ.
- **벨만 방정식**: V*(s) = max_a Σ P(s'|s,a)[R + γV*(s')].
- **Value Iteration / Policy Iteration**: 모델을 **알 때** 최적 정책 계산.

## RL = 모델을 모를 때

- **Model-based**: 전이·보상 추정 후 MDP 풀이.
- **Model-free**:
  - **TD learning**: V(s) ← V(s) + α[r + γV(s') − V(s)].
  - **Q-learning (off-policy)**: Q(s,a) ← Q(s,a) + α[r + γ max_a' Q(s',a') − Q(s,a)].
  - **SARSA (on-policy)**: max 대신 실제 취한 a' 사용.

## 탐험 vs 활용

- **ε-greedy**, 낙관적 초기값.
- off-policy(Q-learning) vs on-policy(SARSA) 차이가 핵심.

## 시험 포인트

- 주어진 에피소드로 Q값/V값 업데이트 직접 계산.
- value iteration 1~2회 반복 손계산.
- Q-learning vs SARSA 업데이트 식 차이, on/off-policy 구분.
- γ, α 역할.

## 관련 기출

- [기출문제 → Reinforcement Learning]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [개념별: Reinforcement Learning]({{ site.baseurl }}/docs/concepts/reinforcement-learning/)
- [다음: 08. Neural Networks]({{ site.baseurl }}/docs/lecture-notes/08-neural-networks/)
