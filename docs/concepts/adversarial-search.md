---
title: Adversarial Search
layout: default
parent: 컨셉별
nav_order: 3
permalink: /docs/concepts/adversarial-search/
---

# Adversarial Search
{: .no_toc }

적대적 탐색(adversarial search)은 상대가 있는 게임을 다룬다. 일반 탐색의 해가 목표까지의 경로라면, 게임의 해는 상대의 모든 응수에 대한 수를 지정하는 전략(strategy)이다. 트리 크기가 `O(b^d)`로 커지므로(체스 약 `10^154`) 유한 시간 내 최적 결정이 핵심이다.

## 한눈에

| 항목 | 내용 |
|---|---|
| 게임 4가정 | 2인 교대 턴 / 결정적(deterministic) / 제로섬(zero-sum) / 완전정보(perfect information) |
| minimax | MAX 노드는 자식 최대값(↑), MIN 노드는 자식 최소값(↓) |
| alpha-beta | `α ≥ β`이면 prune. minimax와 동일 결과 보장 |
| 복잡도 | minimax `O(b^d)` / alpha-beta 최선 `O(b^(d/2))` |

## 핵심 포인트

- 게임의 4요소: 초기 상태 / Successor 함수(합법 수) / Terminal test(종료 판정) / Utility 함수(승 +1, 패 −1, 무 0).
- minimax는 무결점(infallible) 상대를 가정해 MAX의 최적 전략을 계산. MAX 노드는 자식의 최대값, MIN 노드는 자식의 최소값을 취한다. 시간 `O(b^d)`, 공간 `O(bd)`.
- MIN이 비최적으로 두면 MAX의 결과는 minimax 값보다 나빠지지 않으며 오히려 좋아질 수 있다.
- alpha-beta: `α`=MAX가 확보한 최고값, `β`=MIN이 확보한 최저값. `α ≥ β`가 되면 남은 가지를 prune해 같은 결과를 더 적은 탐색으로 얻는다.
- 최선의 수 순서(move ordering)면 유효 분기계수가 `√b`로 줄어(`O(b^(d/2))`) 같은 시간에 약 2배 깊이를 탐색(체스 35 → 약 6).
- 평가함수(evaluation function): 비종료 상태의 가치 추정, 보통 (내 점수) − (상대 점수). 체스는 특징들의 선형 가중합 `Eval(s) = w1·f1(s) + … + wn·fn(s)`.

## 자세히

- [03 Adversarial Search]({{ site.baseurl }}/docs/lecture-notes/03-adversarial-search/)
