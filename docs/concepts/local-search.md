---
title: Local Search
layout: default
parent: 컨셉별
nav_order: 2
permalink: /docs/concepts/local-search/
---

# Local Search
{: .no_toc }

지역 탐색(local search)은 경로가 무의미하고 최종 상태만 중요한 문제(예: 8-queens)에 쓴다. 현재 상태 하나만 유지하고 이웃으로만 이동하므로 메모리를 거의 쓰지 않으며, 크거나 연속적인 상태공간에서도 합리적 해를 자주 찾는다.

## 한눈에

| 항목 | 내용 |
|---|---|
| hill-climbing | 값이 증가하는 방향으로만 이동하는 greedy local search. lookahead 없음 |
| 한계 | local maxima(지역 최적), plateau/shoulder(평지), ridge에서 정지 |
| 해결 | random restart(무작위 재시작) — 단순하지만 자주 효과적 |
| CSP용 | min-conflicts 휴리스틱(위반 제약 수 최소화) |

## 핵심 포인트

- hill-climbing은 바로 이웃만 보고 값이 오르는 쪽으로 이동, 정점에서 종료("짙은 안개 속 에베레스트 정상 찾기"). 지역 최적이 존재하면 suboptimal에 갇힌다.
- 8-queens(complete-state formulation): `h(n)`=서로 공격하는 퀸 쌍의 수(최소화, 해는 `h=0`). 단순 hill-climbing은 14%만 성공, 86%는 지역 최소에 갇힘. 성공 시 평균 4스텝, 실패 시 평균 3스텝.
- random restart 분석: 성공확률 `p=0.14`이면 99% 이상 성공하려면 `(0.86)^n < 0.01` → 약 31회 재시작, 기대 최대 스텝 약 `4×31 = 124`.
- CSP 지역 탐색: 완전 상태(제약 위반 허용)에서 시작, 충돌 중인 변수를 골라 min-conflicts(위반 제약 총수 최소화)로 값을 재할당하는 hill-climbing. n-queens를 `n=10,000,000` 규모에서도 거의 상수 시간에 높은 확률로 해결.

## 자세히

- [02 Informed and Local Search]({{ site.baseurl }}/docs/lecture-notes/02-informed-and-local-search/)
- [04 CSP]({{ site.baseurl }}/docs/lecture-notes/04-csp/)
