---
title: "02. Informed & Local Search"
layout: default
parent: 강의별
nav_order: 2
permalink: /docs/lecture-notes/02-informed-and-local-search/
---

# 02. Informed & Local Search (정보 탐색 · 지역 탐색)
{: .no_toc }

휴리스틱으로 탐색을 안내하는 informed search와 경로를 버리고 현재 상태만 유지하는 local search를 다룬다.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 정보 탐색 (Informed Search)

무정보 탐색은 한계가 뚜렷하다. 8-puzzle은 평균 해 깊이 약 22, 분기계수 `b ≈ 3`이라 깊이 22를 전수 탐색하면 `3.1 × 10^10` 상태에 이른다. `d = 12`만 해도 IDS는 평균 약 360만 노드를 확장한다. 문제 특화 지식을 주입해 이 비용을 줄이는 것이 informed search다.

- **Best-first search**: 각 노드에 평가함수 `f(n)`을 두고 fringe를 `f` 오름차순으로 정렬해 가장 바람직해 보이는 노드부터 확장한다. `f`는 추정치이므로 사실상 "seemingly best-first"다.
- **휴리스틱 함수 `h(n)`**: `n`에서 목표까지의 (최적) 비용 추정치. `n`이 목표면 `h(n) = 0`. 대표 예는 직선거리(SLD). 문제 특화 지식을 탐색에 주입하는 통로다.
- **Greedy best-first search**: `f(n) = h(n)`. 목표에 가장 가까워 보이는 노드만 확장한다. 방문 상태를 기록하지 않으면 DFS처럼 루프에 빠져 완전성 X, 반례가 존재해 최적성 X, 시간·공간 `O(b^m)`.
- **A\* search**: `f(n) = g(n) + h(n)`. `g`는 시작에서 `n`까지의 실제 비용, `h`는 `n`에서 목표까지의 추정 비용, `f`는 `n`을 경유하는 전체 경로 비용의 추정치다.
- **Admissible 휴리스틱**: 모든 `n`에 대해 `h(n) ≤ h*(n)` (`h*`는 실제 최적 비용). 목표까지의 비용을 절대 과대평가하지 않는 낙관적 추정이며 직선거리가 대표 예다.

특수형 정리: UCS는 `f = g`, Greedy는 `f = h`, A\*는 `f = g + h`.

## A* 최적성과 휴리스틱

- **정리**: `h`가 admissible이면 tree-search A\*는 최적이다. 완전성은 `f ≤ f(G)`인 노드가 무한하지 않은 한 보장된다.
- **Optimally efficient**: 같은 휴리스틱에서 어떤 최적 알고리즘도 A\*보다 적은 노드를 확장할 수 없다. 단, 최악의 시간·공간은 여전히 지수적이다.
- **8-puzzle 휴리스틱**:
  - `h1` = 제자리에 없는 타일 수 (강의 예시 상태에서 `h1 = 8`).
  - `h2` = 각 타일의 목표 위치까지 맨해튼 거리 합 (예시: `3+1+2+2+2+3+3+2 = 18`).
- **Dominance**: 모든 `n`에서 `h2(n) ≥ h1(n)`이고 둘 다 admissible이면 `h2`가 `h1`을 지배 → `h2`가 탐색에 더 좋다.

평균 확장 노드 수 (8-puzzle 무작위 인스턴스 평균):

| 깊이 | IDS | A\*(h1) | A\*(h2) |
| --- | --- | --- | --- |
| d=12 | 3,644,035 | 227 | 73 |
| d=24 | 측정 불가 | 39,135 | 1,641 |

- **Relaxed problem으로 휴리스틱 만들기**: 행동 제약을 완화한 문제가 relaxed problem이고, 완화 문제의 최적해 비용은 원문제의 admissible 휴리스틱이 된다(제약이 적으니 비용이 같거나 작음). 8-puzzle에서 "타일이 아무 칸으로나 이동 가능"으로 완화하면 `h1`, "인접 칸으로 자유 이동"으로 완화하면 `h2`가 도출된다.

## 지역 탐색 (Local Search)

어떤 문제는 경로가 무의미하고 최종 상태만 중요하다(예: 8-queens). Local search는 현재 상태 하나만 유지하고 이웃으로만 이동하며 경로를 무시한다. 메모리를 거의 쓰지 않고, 크거나 연속적인 상태공간에서도 합리적 해를 자주 찾는다. 순수 최적화 문제에서는 모든 상태가 목적함수 값을 가지며 목표는 최대(또는 최소) 값을 갖는 상태 발견이다.

**Hill-climbing (언덕 오르기)**: 값이 증가하는 방향으로 계속 이동하다 정점에서 종료하는 greedy local search. 바로 이웃만 보고 lookahead가 없다("짙은 안개 속에서 에베레스트 정상 찾기").

한계 3가지:

1. **Local maxima**: 지역 최적에서 정지.
2. **Plateau / shoulder**: 평지에서 정지.
3. **Ridge**: 능선에서 정지.

지역 최적이 존재하면 suboptimal 해에 머문다. 해결책은 **random restart**(무작위 재시작)로, 단순하지만 자주 효과적이다.

**8-queens 예제 (complete-state formulation)**:

- 상태: 8개 퀸이 모두 보드 위에 있는 구성. Successor: 한 퀸을 같은 열의 다른 칸으로 이동. `h(n)` = 서로 공격하는 퀸 쌍의 수(최소화, 해는 `h = 0`).
- 성능: **14% 성공**, **86% 지역최소**에 갇힘. 성공 시 평균 4스텝, 실패 시 평균 3스텝 (~1,700만 상태 공간).
- Random restart 분석: 성공확률 `p = 0.14` → 99% 이상 성공하려면 `(0.86)^n < 0.01` → `n ≈ 31회`. 기대 최대 스텝 ≈ `4 × 31 = 124`.

## 시험 포인트

- Greedy(`f = h`)와 A\*(`f = g + h`)의 평가함수 차이, 완전성·최적성 비교. Greedy는 불완전·비최적·`O(b^m)`, A\*는 admissible `h`에서 완전·최적·optimally efficient. 단 둘 다 최악 시간·공간은 지수적.
- Admissible 정의 `h(n) ≤ h*(n)`(과대평가 금지)와 A\* 최적성의 관계. 직선거리 예.
- 8-puzzle `h1`(=8), `h2`(=18) 계산과 dominance 근거, `d = 12` 확장 노드 수(IDS 3,644,035 / h1 227 / h2 73).
- Relaxed problem이 admissible 휴리스틱을 생성하는 원리와 8-puzzle 사상(`h1`, `h2`).
- Hill-climbing 한계 3가지(local maxima, plateau, ridge)와 random restart. 8-queens 14%/86% 수치, `(0.86)^n < 0.01` → 약 31회 → 약 124 스텝 계산.

## 기출로 보는 핵심 직관

- **기출 연결**: 8-puzzle 트리가 그림으로 제시되고, `h1`(misplaced tiles)·`h2`(Manhattan)을 적용해 A\*가 **어떤 순서로 확장**하는지, **어느 휴리스틱이 더 좋은지** 비교하는 형태로 나옴. 확장 순서는 제시된 트리에 종속되지만, 각 노드를 `f = g + h`로 매기고 `f` 최소부터 꺼내면 됨. [26전기 기출]({{ site.baseurl }}/docs/notes/past-exams-26/) 참고.
- **핵심 깨달음**: `f = g + h`는 **"이미 쓴 비용(`g`) + 앞으로 쓸 추정(`h`)"**. admissible(`h(n) ≤ h*(n)`, 과대평가 안 함)이면 A\*는 최적해를 절대 건너뛰지 않음. 같은 admissible 휴리스틱 중 **`h` 값이 클수록(= dominate) 목표로 더 집중 → 더 적게 확장**. `h2(n) ≥ h1(n)`이라 h2(Manhattan)가 h1(misplaced)을 지배 → h2가 우월(`d = 12`에서 73 vs 227).
- **직관 보강**: `h(n) = 0`이면 `f = g`라 A\*는 UCS와 같아짐. 반대로 `g`를 무시하고 `f = h`만 보면 Greedy이고 비최적. relaxed problem(제약 완화)의 최적비용이 admissible 휴리스틱이 되는 이유는, 제약을 빼면 비용이 같거나 작아져 원문제를 절대 과대평가할 수 없기 때문.

## 더 보기

관련: [Search Methods]({{ site.baseurl }}/docs/concepts/search-methods/), [Local Search]({{ site.baseurl }}/docs/concepts/local-search/) / 이전: [01]({{ site.baseurl }}/docs/lecture-notes/01-uninformed-search/) / 다음: [03. Adversarial Search]({{ site.baseurl }}/docs/lecture-notes/03-adversarial-search/)
