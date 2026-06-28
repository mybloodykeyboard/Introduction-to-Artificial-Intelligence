---
title: "08. Neural Networks"
layout: default
parent: 강의 노트
nav_order: 8
permalink: /docs/lecture-notes/08-neural-networks/
---

# 08. Artificial Neural Networks (인공 신경망)
{: .no_toc }

선형 결합 + 비선형 활성화의 층을 쌓아 함수를 근사.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- **퍼셉트론**: z = Σ w·x + b, 출력 = activation(z). 선형 분리만 가능 → XOR 불가.
- **다층 퍼셉트론(MLP)**: 은닉층으로 비선형 결정 경계.
- **활성화 함수**: sigmoid, tanh, **ReLU**(기울기 소실 완화).

## 학습

- **손실 함수**: 회귀=MSE, 분류=cross-entropy.
- **경사 하강법**: w ← w − η ∂L/∂w. (batch / mini-batch / SGD)
- **역전파(Backpropagation)**: 연쇄법칙으로 출력→입력 방향 기울기 계산.
- 과적합 대응: regularization, dropout, early stopping.

## 시험 포인트

- 퍼셉트론이 XOR 못 푸는 이유 + 은닉층으로 해결.
- 순전파 1회 손계산(가중치 주어짐).
- 역전파 기울기 한 단계 계산.
- 활성화 함수 비교, 기울기 소실.

## 관련 기출

- [기출문제 → Neural Networks]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [이전: 07. Reinforcement Learning]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/)
- [핵심 요약 (암기)]({{ site.baseurl }}/docs/study-guides/cheat-sheet/)
