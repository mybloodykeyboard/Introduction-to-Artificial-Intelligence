---
title: "06. Logic & Knowledge"
layout: default
parent: 강의 노트
nav_order: 6
permalink: /docs/lecture-notes/06-logic/
---

# 06. Logic & Knowledge (논리 · 지식 표현)
{: .no_toc }

지식을 논리식으로 표현하고, 추론으로 새 사실을 끌어내는 에이전트.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 핵심 개념

- **Knowledge Base (KB)**: 문장(sentence)의 집합.
- **Entailment (수반) α ⊨ β**: α가 참인 모든 모델에서 β도 참.
- **Soundness(건전성)**: 추론이 수반되는 것만 도출. **Completeness(완전성)**: 수반되는 건 모두 도출.

## 명제 논리 (Propositional Logic)

- 진리표, 논리적 동치, CNF 변환.
- **Resolution (분해)**: CNF에서 상보 리터럴 소거. refutation으로 완전.
- **Modus Ponens**, **Forward/Backward Chaining** (Horn clause에서 효율적).
- **DPLL**, WalkSAT 같은 SAT 풀이.

## 1차 논리 (First-Order Logic)

- 객체·관계·함수, ∀ ∃ 한정사.
- Unification, FOL resolution.

## 시험 포인트

- 진리표로 entailment 판정.
- CNF 변환 단계 + resolution refutation.
- forward vs backward chaining 차이와 적용.
- soundness/completeness 정의 구분.

## 관련 기출

- [기출문제 → Logic]({{ site.baseurl }}/docs/study-guides/past-exams/)

## 더 보기

- [다음: 07. Reinforcement Learning]({{ site.baseurl }}/docs/lecture-notes/07-reinforcement-learning/)
