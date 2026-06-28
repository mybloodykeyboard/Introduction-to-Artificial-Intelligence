---
title: Reinforcement Learning
layout: default
parent: 개념별 정리
nav_order: 4
permalink: /docs/concepts/reinforcement-learning/
---

# Reinforcement Learning
{: .no_toc }

MDP 기반, 모델을 모를 때 보상으로 학습.

상세 이론은 [07. Reinforcement Learning]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/) 참고.

## 핵심만

| 구분 | 모델 | 대표 알고리즘 |
|---|---|---|
| MDP 풀이 | 안다 | Value/Policy Iteration |
| Model-free, on-policy | 모름 | SARSA |
| Model-free, off-policy | 모름 | Q-learning |

## 시험 단골

- Q-learning vs SARSA 업데이트 식 차이 (max vs 실제 a').
- 에피소드 주고 Q값 손계산.
- value iteration 1~2회 반복.
