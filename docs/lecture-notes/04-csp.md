---
title: "04. Constraint Satisfaction (CSP)"
layout: default
parent: 강의별
nav_order: 4
permalink: /docs/lecture-notes/04-csp/
---

# 04. Constraint Satisfaction Problems (CSP)
{: .no_toc }

상태를 변수와 도메인, 제약으로 구조화해 범용 알고리즘을 적용하는 탐색 방식.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

일반 탐색은 상태가 블랙박스다. CSP는 상태를 구조화한다.

- 상태 = 변수 Xi + 도메인 Di의 값.
- Goal test = 제약(constraints) 집합.
- 이 구조 덕분에 일반 탐색보다 강력한 범용(general-purpose) 알고리즘을 적용할 수 있다.

호주 지도 색칠 예:

- 변수: WA, NT, Q, NSW, V, SA, T
- 도메인: {red, green, blue}
- 제약: 인접 지역은 색이 달라야 함 (예: WA ≠ NT).

해(solution)의 조건:

- complete: 모든 변수에 값이 할당됨.
- consistent: 모든 제약을 만족함.

Constraint graph: 노드 = 변수, 간선 = 제약(binary CSP). 문제 구조를 활용하는 기반이 된다.

## 제약의 종류

| 종류 | 변수 수 | 예 |
| --- | --- | --- |
| Unary | 1개 | SA ≠ green |
| Binary | 2개 | SA ≠ WA |
| Higher-order | 3개 이상 | cryptarithmetic의 자릿수 제약 |

Cryptarithmetic 예: TWO + TWO = FOUR

- alldifferent(F, T, U, W, R, O)
- O + O = R + 10·X1 등 자릿수 carry 제약.

## Backtracking과 개선 기법

탐색 형식화:

- 초기상태: 빈 할당 { }
- Successor: 현재 할당과 충돌하지 않는 값을 미할당 변수에 할당.
- Goal test: complete & consistent.

할당은 교환 가능(commutative)하다. "WA=red 후 NT=green"과 "NT=green 후 WA=red"는 동일하므로, 노드마다 한 변수만 고려하면 잎(leaf) 수가 n!·d^n에서 d^n으로 축소된다.

Backtracking search = 단일 변수 할당의 DFS. CSP의 기본 무정보(uninformed) 알고리즘이며, 순수 backtracking은 n-queens 기준 n≈25까지 처리한다.

개선 기법 (3+1 세트):

| 기법 | 대상 | 핵심 |
| --- | --- | --- |
| MRV (Minimum Remaining Values) | 변수 선택 | 남은 합법 값이 가장 적은 변수부터 할당. 실패를 빨리 발견. |
| Degree heuristic | 변수 선택 (tie-break) | MRV 동률일 때, 남은 변수들에 제약을 가장 많이 거는 변수 선택. |
| LCV (Least Constraining Value) | 값 선택 | 남은 변수들의 선택지를 가장 적게 줄이는 값부터 시도. |
| Forward checking | 가지치기 | 할당마다 미할당 변수의 남은 합법 값 추적, 도메인이 비면 즉시 종료. |

세 휴리스틱(MRV + Degree + LCV)을 결합하면 1000-queens가 가능하다.

## 추적 예제

4-Queens를 forward checking으로 풀 때 도메인 변화. (해: X1=2, X2=4, X3=1, X4=3)

- X1=1 → X2={3,4}, X3={2,4}, X4={2,3}
  - X2=3 → X3 도메인 공백 → backtrack
  - X2=4 진행 후에도 모순 → X1=2로 backtrack
- X1=2 → X2={4}, X3={1,3}, X4={1,3,4}
  - X2=4 → X3={1}, X4={3}
  - X3=1 → X4={3} → 해 발견

채점 포인트: 할당마다 줄어드는 도메인을 기록하고, 빈 도메인에서 즉시 backtrack.

## Local search for CSP

- 완전 상태(모든 변수 할당, 제약 위반 허용)에서 시작, 연산자는 변수 값 재할당.
- 변수 선택: 충돌 중인 변수에서 무작위.
- Min-conflicts 휴리스틱: 위반 제약 수를 최소화하는 값 선택 = h(n)=위반 제약 총수의 hill-climbing.
- 무작위 초기 상태에서 n-queens를 n=10,000,000 규모에서도 거의 상수 시간에 높은 확률로 해결.

## 시험 포인트

- CSP 정의: 상태 = 변수 Xi + 도메인 Di, goal test = 제약 집합.
- 해의 두 조건 complete & consistent.
- Constraint graph: 노드=변수, 간선=binary 제약.
- 교환 가능성으로 잎 수가 n!·d^n → d^n으로 축소.
- 개선 기법 4가지(MRV / Degree / LCV / Forward checking)와 각각의 대상(변수 선택 vs 값 선택 vs 가지치기) 구분.
- 4-Queens forward checking 도메인 추적과 해 X1=2, X2=4, X3=1, X4=3.
- Min-conflicts로 n-queens를 1천만 규모까지 해결.

## 더 보기

관련: [CSP 컨셉]({{ site.baseurl }}/docs/concepts/csp/) / 이전: [03]({{ site.baseurl }}/docs/lecture-notes/03-adversarial-search/) / 다음: [05. Classification]({{ site.baseurl }}/docs/lecture-notes/05-classification/)
