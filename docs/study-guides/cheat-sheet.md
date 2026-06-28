---
title: 핵심 요약 (암기)
layout: default
parent: 시험 대비
nav_order: 4
permalink: /docs/study-guides/cheat-sheet/
---

# 핵심 요약 (암기)
{: .no_toc }

시험장 들어가기 직전 1분 컷. 공식·정의만.

## 공식

- **A\***: f(n) = g(n) + h(n)
- **벨만 최적**: V*(s) = max_a Σ P(s'|s,a)[R + γV*(s')]
- **Q-learning**: Q(s,a) ← Q(s,a) + α[r + γ max_a' Q(s',a') − Q(s,a)]
- **TD**: V(s) ← V(s) + α[r + γV(s') − V(s)]
- **경사하강**: w ← w − η ∂L/∂w
- **퍼셉트론**: z = Σ w·x + b

## 한 줄 정의

- **Admissible**: 과대평가 안 함 (h ≤ 실제).
- **Consistent**: 삼각부등식 만족.
- **Alpha-Beta 컷**: α ≥ β.
- **MRV**: 남은 값 최소 변수 먼저.
- **off-policy**: 행동 정책 ≠ 학습 정책 (Q-learning).
- **Soundness**: 맞는 것만 도출 / **Completeness**: 맞는 건 다 도출.

## 자주 틀리는 포인트

- 완전성 ≠ 최적성.
- consistent ⇒ admissible (역 아님).
- beam search는 정보 공유 (≠ 독립 실행).
- 퍼셉트론은 XOR 못 풂 (은닉층 필요).

> 본인 시험 범위 핵심만 추려서 계속 갱신.
