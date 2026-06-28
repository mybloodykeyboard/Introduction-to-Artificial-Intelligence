---
title: Adversarial Search
layout: default
parent: 개념별 정리
nav_order: 3
permalink: /docs/concepts/adversarial-search/
---

# Adversarial Search
{: .no_toc }

상대가 있는 게임 탐색.

상세 이론은 [04. Adversarial Search]({{ site.baseurl }}/docs/lecture-notes/04-adversarial-search/) 참고.

## 핵심만

- **Minimax**: MAX↑, MIN↓. 완전정보·제로섬·2인 가정.
- **Alpha-Beta**: α ≥ β면 가지치기. 최적 순서면 깊이 2배 효과.
- **Expectimax**: 확률 노드 기댓값.

## 시험 단골

- 게임 트리에서 minimax 값 채우기 → alpha-beta로 잘리는 노드 표시.
- move ordering 좋/나쁨에 따른 복잡도.
