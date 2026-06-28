---
title: "07. Reinforcement Learning"
layout: default
parent: 강의별
nav_order: 7
permalink: /docs/lecture-notes/07-reinforcement-learning/
---

# 07. Reinforcement Learning (Q-learning)
{: .no_toc }

에이전트가 시행착오 속에서 외부 환경의 Reward로 Goal 도달법을 배우는 학습 — learn via experiences.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

기계학습의 3분류

- Supervised(지도): 목표값(label)이 제시된 데이터로 학습.
- Unsupervised(비지도): label 없는 데이터로 학습.
- Reinforcement(강화): 에이전트가 자신의 행동에 대한 보상(reward)을 받아 점차 효율적인 방식으로 행동을 강화.

ML의 핵심 = 표현(representation) + 일반화(generalization). 주어진 데이터의 표현 + 아직 주어지지 않은 데이터의 처리.

강화학습이란

반복 시도의 시행착오 속에서 외부 환경의 Reward를 통해 Goal에 도달하는 법을 배우는 학습. 에이전트는 환경을 지각(percept)하여 state·reward를 받고 action을 환경에 가함.

특징 3가지

1. Delayed reward — 선택한 Action이 적합한지 당장 알 수 없음.
2. 현재 Action의 결과를 정확히 몰라도 무관.
3. Life-long learning에 활용 가능.

## Q-Learning

RL 중 가장 널리 쓰이는 알고리즘. 현재 상태에서 선택 가능한 Action 중 임의의 것을 골라 실행하고 환경에서 Reward를 받음(시행착오). 학습 시점엔 평가가 미완이므로 당시 최적으로 보인 Action이 실제 최적이 아닐 수 있음. Reward를 전파하는 형태로 학습.

정의·갱신식

`Q(s,a) = r(s,a) + γ·max_a′ Q(s′,a′)`

Q는 상태 s에서 행동 a가 유리한 정도의 추정 효용함수 = 즉각 보상(immediate reward) + 다음 상태 s′의 잠재 보상 최대값(delayed reward). γ∈[0,1]은 보상 전파 계수.

학습 후 행동 선택

`π(s) = argmax_a Q(s,a)` — 매 스텝 Q를 최대화하는 행동으로 이동.

알고리즘

모든 (s,a)에 Q=0 초기화 후 수렴까지 반복.

1. 현재 s에서 가능한 임의 Action a 선택·실행.
2. immediate reward 수령.
3. 새 상태 s′ 감지.
4. `Q(s,a) = r + γ·max Q(s′,a′)`로 갱신.
5. s = s′.

## 계산 예제

6칸 격자, 목표 S6, γ=0.5, 목표 도달 보상 100.

Iteration 1 (S1 출발)

- S1→S2: `Q(S1,a12) = 0 + 0.5·max(0,0,0) = 0`
- S2→S3: `Q(S2,a23) = 0`
- S3→S6 (FINAL): `Q(S3,a36) = r = 100`

Iteration 2 (S2 출발)

- S2→S3: `Q(S2,a23) = 0 + 0.5·max(Q(S3,a32), Q(S3,a36)) = 0.5·100 = 50`
- S3→S6: `Q(S3,a36) = 100`

Iteration 3 (S4 출발)

- `Q(S4,a41) = 0` → `Q(S1,a12) = 0.5·50 = 25` → `Q(S2,a23) = 50`
- S3→S2: `Q(S3,a32) = 0.5·50 = 25` → S2→S5: `Q(S2,a25) = 0` → S5→S6: `Q(S5,a56) = r = 100`
- 보상이 목표에서 멀어질수록 γ배씩 감쇠하며 역방향으로 전파됨.

수렴 후 최종 Q 테이블

| Q값 | 해당 (s,a) | 목표까지 거리 |
| --- | --- | --- |
| 100 | Q(S3,a36), Q(S5,a56) | 1칸 (목표 인접) |
| 50 | Q(S2,a23), Q(S2,a25), Q(S4,a45) | 2칸 |
| 25 | Q(S1,a12), Q(S1,a14), Q(S3,a32), Q(S5,a52), Q(S5,a54) | 3칸 |
| 12.5 | Q(S2,a21), Q(S4,a41) | 4칸 |

## 시험 포인트

- 강화학습 정의: learn via experiences, 보상 신호로 행동을 점차 강화. 지도(label 있음)·비지도(label 없음)와 달리 명시적 정답 대신 reward로 학습.
- 특징 3가지: delayed reward / 현재 action 결과를 몰라도 무관 / life-long learning.
- 갱신식 `Q(s,a) = r(s,a) + γ·max_a′ Q(s′,a′)` 암기. immediate reward + delayed reward 구조.
- 학습 후 정책 `π(s) = argmax_a Q(s,a)`.
- 계산형: 목표 인접 상태부터 γ배씩 감쇠하며 시작 쪽으로 역전파. 목표 인접 100, 2칸 50, 3칸 25, 4칸 12.5.

## 기출로 보는 핵심 직관

기출 연결: 실제 시험은 위 예제처럼 목표에만 100을 주는 sparse reward 형태가 아니라, **모든 transition(`a12, a23, ...`)에 즉각 보상(대략 4~20)을 준** 격자에서 경로를 따라 Q를 채우는 dense reward 문제로 나옴. [26전기 기출]({{ site.baseurl }}/docs/notes/past-exams-26/)에 dense reward worked example(경로별 Q 전파 계산) 있음.

핵심 깨달음: `Q(s,a) = r + γ·max_a′ Q(s′,a′)` 는 **"이 행동의 즉각 보상 + 다음 상태에서 앞으로 받을 최선값의 할인"**. 모든 Q=0에서 시작해 episode를 반복하면, 한 episode 안에서는 목표 쪽(뒤) step부터 값이 생기고 episode를 거듭하며 그 값이 시작 쪽(앞)으로 **역방향 전파**됨. γ는 미래 보상 할인율로 목표에서 멀수록 영향이 작아짐.

sparse vs dense 차이:

- sparse(목표에만 100): 목표 인접 Q부터 γ배씩 감쇠하며 전파 — 100 → 50 → 25 → 12.5 (위 계산 예제).
- dense(매 transition 보상): 각 `Q(s,a) = r + γ·다음 상태 최선 Q`로 채워져, 경로별 누적값이 칸마다 보임. 즉각 보상이 매 step에 있으므로 목표 인접만 의미 있는 게 아니라 모든 칸이 채워짐.

학습 후 정책은 두 경우 모두 `π(s) = argmax_a Q(s,a)`.

## 더 보기

- 관련: [Reinforcement Learning 컨셉]({{ site.baseurl }}/docs/concepts/reinforcement-learning/)
- 이전: [06]({{ site.baseurl }}/docs/lecture-notes/06-clustering/)
- 다음: [08. Artificial Neural Network]({{ site.baseurl }}/docs/lecture-notes/08-neural-networks/)
