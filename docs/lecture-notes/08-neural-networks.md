---
title: "08. Artificial Neural Network"
layout: default
parent: 강의별
nav_order: 8
permalink: /docs/lecture-notes/08-neural-networks/
---

# 08. Artificial Neural Network (Perceptron · Backpropagation)
{: .no_toc }

생물 뉴런 구조를 모방한 Perceptron에서 출발해, XOR의 한계를 다층과 Backpropagation으로 극복하는 흐름을 정리한다.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 생물학적 영감

뇌와 컴퓨터는 처리 방식이 근본적으로 다르다.

| 구분 | 뇌 | 컴퓨터 |
| --- | --- | --- |
| 규모 | 100억 뉴런, 60조 시냅스 | 뉴런(10⁻³s)보다 빠른 소자(10⁻⁹s) |
| 처리 방식 | 분산·비선형·병렬 | 중앙·산술(선형)·순차 |

ANN은 뉴런 구조를 모사한다. Dendrites(입력 수신) → Nucleus(처리) → Axon terminals(출력 전파)의 흐름을 본떠, 기존 Symbolic AI로 풀기 어려운 문제에 접근한다. 이 신경망의 기본 unit이 Perceptron이다.

## Perceptron

- 구조: 복수의 입력 pᵢ와 가중치 wᵢ의 곱의 합 `Σwᵢpᵢ`가 임계치 θ를 기준으로 출력 y의 흥분 여부를 결정한다.
- Activation function: hard limiter / linear / sigmoid.

학습은 Widrow-Hoff rule(delta rule)을 따르는 지도학습이다.

1. 가중치 W와 임계치 θ를 작은 임의값으로 초기화
2. 입력 패턴 X와 목표 출력 d(t) 제시
3. 출력 계산: `y(t) = fₙ(ΣWᵢXᵢ − θ)`
4. 가중치 갱신: `Wᵢ(t+1) = Wᵢ(t) + α[d(t) − y(t)]·Xᵢ(t)`

출력이 목표와 같으면 가중치는 불변, 다르면 차이를 줄이는 방향으로 변경한다.

## XOR 문제

단층 Perceptron은 AND는 구현하지만 XOR은 구현하지 못한다.

- AND 가능(θ=0.5): `1·W0 + 1·W1 > 0.5`만 성립하면 되므로 W0=W1=0.3(또는 0.4)으로 충족.
- XOR 불가능 — linearly non-separable: `0 < 0.5`, `W1 > 0.5`, `W0 > 0.5`, `W0 + W1 < 0.5`를 동시에 만족하는 W는 존재하지 않는다. 직선 하나로 분리 불가하므로 단층 퍼셉트론은 간단한 XOR도 해결하지 못한다.

해결책은 다층(2~3개 layer)이다. 3층 Perceptron으로 어떤 문제도 근사적으로 해결 가능하며, 이것이 MLP와 Error Backpropagation의 기반이 된다.

## Backpropagation

- 정의: input layer와 output layer 사이에 하나 이상의 hidden layer를 두는 단방향 신경망. 단층의 선형분리 한계를 해결(XOR 구현)하고 일반적 연속함수 근사에 널리 쓰인다. 80년대 중반의 Error Backpropagation은 일반화된 델타 규칙(generalized delta rule)이다.
- 학습 원리: 목표값 d와 실제 출력 o의 오차제곱합 `E = ½Σ(d−o)²`를 Gradient Descent로 최소화한다. output layer의 오류로 hidden layer 가중치를 재계산하고, 이를 다시 input layer 쪽으로 역전파(backpropagation)하여 가중치를 재계산한다.

Sigmoid 사용 시 핵심 식은 다음과 같다.

- Sigmoid: `y = 1/(1+e⁻ˣ)`, 미분 `∂y/∂x = y(1−y)`
- 출력층 오류: `δ_pk = (d_pk−O_pk)·O_pk(1−O_pk)`
- 은닉층 오류: `δ_pj = [Σₖ δ_pk·W_jk]·O_pj(1−O_pj)`
- 은닉-출력 갱신: `W_jk(t+1) = W_jk(t) + η·δ_pk·O_pj`
- 입력-은닉 갱신: `W_ij(t+1) = W_ij(t) + η·δ_pj·X_pi`

전체 절차는 W·θ 초기화 → 입력 X·목표 d 제시 → 은닉층 입력 `Σ X_pi·W_ij − θⱼ` 계산 → sigmoid로 O_pj → 출력층 입력·O_pk 계산 → gradient(δ) 계산 → 두 층 가중치 갱신 → 모든 학습쌍 반복, 오차합 E가 허용값 이하 또는 최대 반복 도달 시 종료다.

문제점: 상위층의 목표-출력 오류를 하위층으로 역전파하는 것은 생물학적 현상과 불일치한다(하위 뉴런은 상위 목표값을 모르는 게 일반적). 그럼에도 가장 보편적으로 쓰이는 ANN 학습법이다.

## 시험 포인트

- Q37. Perceptron 구조와 Widrow-Hoff rule(delta rule) 학습 절차: 입력 pᵢ·가중치 wᵢ의 곱의 합이 θ 기준으로 activation을 거쳐 y 결정. 학습은 ① W·θ 초기화 ② X·d(t) 제시 ③ `y(t) = fₙ(ΣWᵢXᵢ(t) − θ)` ④ `Wᵢ(t+1) = Wᵢ(t) + α[d(t)−y(t)]·Xᵢ(t)` — 목표와 같으면 불변, 다르면 차이를 줄이는 방향으로 갱신.
- Q38. 단층 Perceptron의 AND 가능·XOR 불가능: AND는 W0=W1=0.3(또는 0.4)으로 충족 가능, XOR은 `0 < 0.5`, `W1 > 0.5`, `W0 > 0.5`, `W0 + W1 < 0.5`를 동시에 만족하는 W가 없어 linearly non-separable. 해결은 2~3층 MLP, 3층 Perceptron으로 근사적 해결 가능하며 Backpropagation NN의 기반.
- Q39. Backpropagation NN 구조·학습 원리·문제점: 은닉층을 둔 단방향 신경망으로 `E = ½Σ(d−o)²`를 Gradient Descent로 최소화. `δ_pk = (d_pk−O_pk)·O_pk(1−O_pk)`로 은닉-출력 가중치를, 역전파한 `δ_pj`로 입력-은닉 가중치를 갱신(일반화된 델타 규칙, sigmoid 미분 `y(1−y)` 이용). 문제점은 역전파가 생물학적 현상과 불일치하나 가장 보편적인 학습법이라는 점.

## 기출로 보는 핵심 직관

기출 연결: "backpropagation과 유사한 개념"을 묻는 5지선다(보기에 UCS·local search). [26전기 기출]({{ site.baseurl }}/docs/notes/past-exams-26/) 참고.

핵심 깨달음 — **backpropagation을 보는 두 관점**:

1. **gradient descent = local search**: 오차 `E = ½Σ(d−o)²`를 weight 공간에서 줄여가는 탐색 → local minima에 빠질 수 있음(hill-climbing과 같은 결).
2. **dynamic programming = 값 전파**: 출력층 오차 δ를 뒤로 전파하며 각 층 값을 갱신 → **UCS(=Dijkstra)가 누적비용 g(n)을 전방 전파**하는 것과 같은 "이미 계산한 값을 재사용해 전파" 구조. 그래서 기출에서 UCS가 정답으로 묶임.

보강: perceptron은 직선 하나(선형 분리)라 XOR 불가 → hidden layer(다층)로 비선형 표현, backprop으로 학습. 이 흐름(한계→해결)이 ANN 단원의 큰 줄기다.

## 더 보기

관련: [Artificial Neural Network 컨셉]({{ site.baseurl }}/docs/concepts/artificial-neural-network/) / 이전: [07]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/)
