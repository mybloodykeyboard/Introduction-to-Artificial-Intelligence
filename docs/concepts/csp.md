---
title: Constraint Satisfaction
layout: default
parent: 컨셉별
nav_order: 4
permalink: /docs/concepts/csp/
---

# Constraint Satisfaction
{: .no_toc }

제약 만족 문제(CSP, Constraint Satisfaction Problem)는 상태를 변수 `Xi`와 도메인 `Di`의 값으로, goal test를 제약(constraints) 집합으로 정의한다. 상태가 블랙박스인 일반 탐색과 달리 구조가 드러나 있어 더 강력한 범용(general-purpose) 알고리즘을 적용할 수 있다.

## 한눈에

| 기법 | 역할 |
|---|---|
| Backtracking | 단일 변수 할당의 DFS. CSP의 기본 무정보 알고리즘 |
| MRV | 남은 합법 값이 가장 적은 변수부터 할당 — 실패 조기 발견 |
| Degree heuristic | MRV 동률일 때 다른 변수에 제약을 가장 많이 거는 변수 선택 |
| LCV | 다른 변수의 선택지를 가장 적게 줄이는 값부터 선택 |
| Forward checking | 미할당 변수의 합법 값을 추적, 도메인이 비면 즉시 중단 |

## 핵심 포인트

- CSP의 해 = complete(모든 변수 할당) + consistent(모든 제약 만족)한 할당. 호주 지도 색칠이 대표 예(인접 지역 색 다르게, `WA ≠ NT`).
- 제약 종류: Unary(변수 1개, `SA ≠ green`) / Binary(2개, `SA ≠ WA`) / Higher-order(3개 이상, cryptarithmetic의 자릿수 제약).
- 할당은 교환 가능(commutative)하므로 노드마다 한 변수만 고려 → 잎이 `n!·d^n`에서 `d^n`으로 축소.
- MRV + Degree + LCV + Forward checking을 결합하면 1000-queens도 풀린다. MRV/Degree는 변수 선택, LCV는 값 선택, Forward checking은 필연적 실패의 조기 감지를 담당.
- Min-conflicts(지역 탐색): 완전 상태(제약 위반 허용)에서 위반 제약 수를 최소화하는 값으로 재할당. 무작위 초기 상태에서 n-queens를 `n=10,000,000` 규모에서도 거의 상수 시간에 해결.

## 자세히

- [04 CSP]({{ site.baseurl }}/docs/lecture-notes/04-csp/)
