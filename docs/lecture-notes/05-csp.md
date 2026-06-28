---
title: "05. Constraint Satisfaction"
layout: default
parent: 강의 노트
nav_order: 5
permalink: /docs/lecture-notes/05-csp/
---

# 05. Constraint Satisfaction Problems (제약 충족)
{: .no_toc }

변수에 값을 할당하되 제약을 모두 만족시키는 문제. 상태 내부 구조를 활용.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- **구성**: 변수 X, 도메인 D, 제약 C.
- 예: 지도 색칠, N-Queens, 스도쿠, 시간표.
- **제약 그래프**: 노드=변수, 엣지=제약. 구조로 난이도 판단.

## 추론 (Inference)

- **Node consistency**: 단항 제약 만족.
- **Arc consistency (AC-3)**: 모든 호 (Xi,Xj)에 대해, Xi의 각 값에 대응하는 Xj 값이 존재.
- forward checking: 할당 시 이웃 도메인에서 충돌 값 제거.

## 탐색 (Backtracking) + 휴리스틱

- **MRV (Minimum Remaining Values)**: 남은 값 가장 적은 변수 먼저.
- **Degree heuristic**: 제약 많이 걸린 변수 먼저 (tie-break).
- **LCV (Least Constraining Value)**: 이웃 선택지 가장 적게 줄이는 값 먼저.

## 시험 포인트

- AC-3 실행 과정 추적 (도메인 줄어드는 순서).
- MRV/Degree/LCV 적용해서 다음 할당 변수·값 고르기.
- forward checking vs arc consistency 차이.

## 관련 기출

- [기출문제 → CSP]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [개념별: 탐색 방법]({{ site.baseurl }}/docs/concepts/search-methods/)
- [다음: 06. Logic & Knowledge]({{ site.baseurl }}/docs/lecture-notes/06-logic/)
