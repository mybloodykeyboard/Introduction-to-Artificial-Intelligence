---
title: "04. Adversarial Search"
layout: default
parent: 강의 노트
nav_order: 4
permalink: /docs/lecture-notes/04-adversarial-search/
---

# 04. Adversarial Search (적대 탐색 / 게임)
{: .no_toc }

상대가 있는 환경. 상대는 내 손해를 최대로 만들려 함 → minimax.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- **Minimax**: MAX는 값 최대화, MIN은 최소화. 완전 정보·제로섬·2인 게임 가정.
- 게임 트리를 끝까지 → 시간 O(b^m), 공간 O(bm). 현실에선 깊이 제한 + 평가 함수.

## Alpha-Beta Pruning

- 결과를 바꾸지 않으면서 탐색 가지치기.
- α = MAX가 보장받는 최선, β = MIN이 보장받는 최선. **α ≥ β면 가지치기**.
- 최적 순서면 O(b^(m/2)) → 같은 시간에 깊이 2배.
- **이동 순서(move ordering)** 가 효율의 핵심.

## 확장

- **평가 함수**: 비종료 노드 가치 추정. 특징의 가중합이 흔함.
- **기댓값 minimax (Expectimax)**: 확률 노드(주사위 등) 포함, 기댓값 계산.
- **horizon effect**, quiescence search 같은 실전 이슈.

## 시험 포인트

- alpha-beta로 가지치기되는 노드 직접 표시 (단골 출제).
- 같은 트리에서 minimax 값 계산 후 pruning 비교.
- move ordering이 최선/최악일 때 복잡도 차이.

## 관련 기출

- [기출문제 → Adversarial Search]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [개념별: Adversarial Search]({{ site.baseurl }}/docs/concepts/adversarial-search/)
- [다음: 05. Constraint Satisfaction]({{ site.baseurl }}/docs/lecture-notes/05-csp/)
