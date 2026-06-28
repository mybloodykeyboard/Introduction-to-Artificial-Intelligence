---
title: Artificial Neural Network
layout: default
parent: 컨셉별
nav_order: 8
permalink: /docs/concepts/artificial-neural-network/
---

# Artificial Neural Network
{: .no_toc }

뉴런 구조를 모방한 perceptron이 기본 단위. 단층은 XOR도 못 풀지만, hidden layer를 더한 MLP와 backpropagation이 그 한계를 깬다.

## 한눈에

- Perceptron: 입력 `pᵢ`와 가중치 `wᵢ`의 곱의 합 `Σwᵢpᵢ`가 임계치를 넘으면 출력 흥분. activation은 hard limiter / linear / sigmoid.
- 한계: AND는 되지만 XOR은 linearly non-separable이라 단층으로 불가능.
- 해결: hidden layer를 둔 MLP + backpropagation으로 일반 연속함수 근사까지 가능.

## 핵심 포인트

- XOR이 안 되는 이유: `0<0.5`, `W1>0.5`, `W0>0.5`, `W0+W1<0.5`를 동시에 만족하는 W가 없음 → 단층 퍼셉트론의 결정적 한계.
- 3층 perceptron이면 어떤 문제도 근사 가능. perceptron은 MLP와 Error Backpropagation의 기반.
- Backprop 학습 원리: 오차제곱합 `E=½Σ(d−o)²`를 gradient descent로 최소화. 출력층 오류로 은닉층 가중치를 재계산하고 input 쪽으로 역전파.
- Sigmoid 핵심 식: `y=1/(1+e⁻ˣ)`의 미분이 `y(1−y)`. 출력층 `δ_pk=(d−O)·O(1−O)`, 은닉층 `δ_pj=[Σ δ_pk·W]·O(1−O)`.
- 문제점: 상위층 오류를 하위층으로 역전파하는 건 생물학적 현상과 불일치(하위 뉴런은 상위 목표값을 모름). 그래도 가장 보편적인 ANN 학습법.

## 자세히

강의노트: [08]({{ site.baseurl }}/docs/lecture-notes/08-neural-networks/)
