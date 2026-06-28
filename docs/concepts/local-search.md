---
title: Local Search
layout: default
parent: 개념별 정리
nav_order: 2
permalink: /docs/concepts/local-search/
---

# Local Search
{: .no_toc }

경로가 아닌 목표 상태 자체가 답일 때.

상세 이론은 [03. Local Search]({{ site.baseurl }}/docs/lecture-notes/03-local-search/) 참고.

## 한눈에

| 알고리즘 | 나쁜 이동 허용 | 갇히는 곳 탈출 |
|---|---|---|
| Hill Climbing | X | random restart |
| Simulated Annealing | 확률적으로 O | 온도 T로 조절 |
| Local Beam | — | 상위 k개 정보 공유 |
| Genetic | 변이로 | crossover + mutation |

## 시험에서 묻는 것

- 지형(local max / plateau / ridge)별 약점과 해결책.
- simulated annealing 수용 확률 e^(ΔE/T).
- beam search ≠ k개 독립 hill climbing.
