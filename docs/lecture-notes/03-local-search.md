---
title: "03. Local Search"
layout: default
parent: 강의 노트
nav_order: 3
permalink: /docs/lecture-notes/03-local-search/
---

# 03. Local Search (지역 탐색)
{: .no_toc }

경로가 아니라 **목표 상태 자체**가 답일 때, 현재 상태만 들고 이웃으로 이동하며 개선.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- 경로 비기억 → 메모리 적음, 큰/연속 공간에 적합.
- 목적 함수(objective)를 최대화/최소화하는 상태 탐색.
- **문제 지형**: global maximum, local maximum, plateau(평탄), ridge(능선), shoulder.

## 주요 알고리즘

- **Hill Climbing**: 항상 더 나은 이웃으로. local maximum/plateau에 갇힘.
  - 변형: stochastic, first-choice, **random-restart**(여러 시작점 → 사실상 완전).
- **Simulated Annealing**: 확률 e^(ΔE/T)로 나쁜 이동도 허용, T를 점차 낮춤. T→0이면 hill climbing. 스케줄 천천히 식히면 global optimum 확률 ↑.
- **Local Beam Search**: 상태 k개 동시 유지, 매 단계 후보 중 상위 k개 선택. (≠ k개 독립 실행)
- **Genetic Algorithm**: 선택 → 교차(crossover) → 변이(mutation). 적합도(fitness) 기반.

## 시험 포인트

- hill climbing이 갇히는 3가지 지형(local max, plateau, ridge)과 해결책.
- simulated annealing의 수용 확률 식과 T 역할.
- beam search가 random restart와 다른 점: 정보 공유.

## 관련 기출

- [기출문제 → Local Search]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [개념별: Local Search]({{ site.baseurl }}/docs/concepts/local-search/)
- [다음: 04. Adversarial Search]({{ site.baseurl }}/docs/lecture-notes/04-adversarial-search/)
