---
title: 기출 모범답안
layout: default
parent: 정리노트
nav_order: 2
permalink: /docs/notes/model-answers/
---

# 기출 모범답안
{: .no_toc }

기출·예상 서술형 문항의 모범답안 + 채점 포인트 + 암기 한줄 요약.
{: .fs-5 .fw-300 }

> **출처**: 이 페이지의 문항·모범답안·채점 포인트·한줄 요약은 [passa8837의 인공지능 개론 정리 블로그](https://passa8837.github.io/Introduction-to-Artificial-Intelligence/doc/기출문제-모범답안.html) 내용을 거의 그대로 가져와 정리한 것임.

<details open markdown="block">
  <summary>목차</summary>
{: .text-delta }
- TOC
{:toc}
</details>

## 기출문제

### 1. Uninformed search 종류를 말하고 장단점을 적기

**모범답안**

Uninformed search는 문제에 대한 추가 정보나 휴리스틱 없이 상태공간의 구조만을 이용하여 탐색하는 방법이다. 강의에서 다룬 대표적인 uninformed search에는 **BFS, DFS, IDS, UCS**가 있다.

첫째, BFS(Breadth-First Search) 는 가장 얕은 깊이의 노드부터 먼저 확장하는 방법으로, 보통 큐(queue)를 사용한다. 장점은 해가 존재하면 찾을 수 있는 완전성이 있고, 모든 step cost가 같을 때는 가장 얕은 해를 먼저 찾으므로 최적성도 보장된다는 점이다. 단점은 같은 깊이의 노드를 대량으로 저장해야 하므로 공간복잡도와 시간복잡도가 매우 크다는 것이다.

둘째, DFS(Depth-First Search) 는 가장 깊은 노드를 먼저 확장하는 방법으로, 보통 스택(stack)이나 재귀를 사용한다. 장점은 한 번에 유지하는 노드 수가 적어 메모리 사용량이 작고 구현이 단순하다는 점이다. 단점은 무한 경로나 loop가 있으면 해를 찾지 못할 수 있어 완전하지 않을 수 있고, 또한 먼저 찾은 해가 최적해라는 보장이 없어 최적성도 없다.

셋째, IDS(Iterative Deepening Search) 는 깊이 제한을 둔 DFS를 깊이 0, 1, 2, ... 식으로 반복하는 방법이다. 장점은 DFS처럼 메모리를 적게 사용하면서도 BFS처럼 얕은 해를 먼저 찾는 성질을 가져, 해가 존재하면 찾을 수 있고 동일 step cost에서는 최적성도 기대할 수 있다는 점이다. 단점은 같은 상위 노드를 여러 번 다시 탐색하므로 반복 오버헤드가 발생한다는 것이다. 다만 일반적으로 이 오버헤드는 큰 수준의 비효율로 보이지는 않는다.

넷째, UCS(Uniform Cost Search) 는 깊이가 아니라 누적 경로 비용이 가장 작은 노드를 먼저 확장하는 방법이다. 장점은 step cost가 서로 다른 문제에서도, 각 step cost가 양수일 때 완전성과 최적성을 보장한다는 점이다. 단점은 비용이 작은 경로를 우선적으로 계속 확장해야 하므로 경우에 따라 많은 노드를 저장하고 비교해야 해서 시간과 메모리 부담이 크다는 것이다.

정리하면, uninformed search의 공통 장점은 문제 특화 지식이 없어도 적용 가능하다는 것이고, 공통 단점은 상태공간이 커지면 탐색 비용이 급격히 증가한다는 것이다.

**핵심 채점 포인트**
- uninformed search는 휴리스틱 없이 탐색한다는 정의를 먼저 써야 한다.
- 대표 알고리즘으로 BFS, DFS, IDS, UCS를 제시하면 안정적이다.
- 각 방법별로 확장 기준, 완전성, 최적성, 시간/공간 측면을 함께 적어야 한다.

**암기 한줄**
> BFS는 얕은 해 우선, DFS는 깊은 해 우선, IDS는 DFS 메모리와 BFS 성질의 절충, UCS는 비용이 가장 작은 경로 우선 탐색이다.

### 2. Informed search 종류를 말하고 장단점을 적기

**모범답안**

Informed search는 문제에 대한 추가 정보인 휴리스틱(heuristic) 을 사용하여 목표에 더 유망한 노드를 우선적으로 탐색하는 방법이다. 즉, uninformed search보다 적은 수의 노드를 보고도 해에 도달하도록 설계된 탐색이다. 강의에서 다룬 대표적인 informed search는 Greedy best-first search 와 A* search이다.

첫째, Greedy best-first search 는 현재 노드에서 목표까지의 추정 비용인 `h(n)` 만을 사용하여 가장 목표에 가까워 보이는 노드를 먼저 확장한다.

`f(n) = h(n)`

장점은 목표 방향으로 빠르게 움직이기 때문에 실제 탐색 속도가 빠를 수 있고, 적절한 휴리스틱이 있으면 불필요한 탐색을 많이 줄일 수 있다는 점이다. 단점은 지금까지 실제로 든 비용 `g(n)` 을 고려하지 않기 때문에 비최적 해를 선택할 수 있고, 경우에 따라 loop나 비효율적인 경로로 빠질 수 있다는 점이다.

둘째, A* search는 시작점부터 현재 노드까지의 실제 비용 `g(n)` 과 현재 노드에서 목표까지의 추정 비용 `h(n)` 을 함께 고려한다.

`f(n) = g(n) + h(n)`

장점은 Greedy보다 더 균형 있게 탐색하며, 휴리스틱이 admissible 즉 실제 최적 비용을 과대추정하지 않을 때 최적성을 보장한다는 점이다. 또한 좋은 휴리스틱을 사용하면 uninformed search보다 훨씬 적은 노드를 확장할 수 있다. 단점은 여전히 많은 후보 노드를 frontier에 저장해야 하므로 메모리 사용량이 크고, 휴리스틱이 부정확하면 성능 향상이 크지 않을 수 있다는 점이다.

또한 informed search 전체의 관점에서 보면, 장점은 문제 특화 정보를 사용하므로 탐색 효율을 높일 수 있다는 것이고, 단점은 좋은 휴리스틱을 설계해야 하며 휴리스틱의 품질에 성능이 크게 좌우된다는 것이다.

**핵심 채점 포인트**
- informed search는 휴리스틱 사용이 핵심이다.
- 대표 알고리즘으로 Greedy best-first search, A* 를 적는 것이 좋다.
- Greedy는 `h(n)` 만, A*는 `g(n) + h(n)` 이라는 차이를 분명히 써야 한다.
- A*는 **admissible heuristic 아래에서 optimal**하다는 점을 쓰면 답안 완성도가 높다.

**암기 한줄**
> Greedy는 현재 목표에 가까워 보이는 노드를 빨리 고르지만 최적성을 보장하지 않고, A*는 실제 비용과 추정 비용을 함께 고려해 더 안정적으로 최적해를 찾는다.

### 3. Local search 종류 하나를 말하고 특징, 장단점을 적기

**모범답안**

Local search는 시작 상태에서 목표 상태까지의 경로(path) 자체보다, 현재 상태를 조금씩 개선하여 좋은 상태 자체를 찾는 데 초점을 두는 탐색 방법이다. 따라서 경로 정보는 중요하지 않고, 현재 상태와 그 이웃 상태만을 주로 고려한다. 대표적인 local search 방법으로 Hill Climbing 이 있다.

Hill Climbing은 현재 상태에서 평가값이 더 좋은 이웃 상태로 계속 이동하는 방법이다. 즉, 주변 상태 중 더 나은 방향으로만 움직이며 점진적으로 해를 개선한다. 이 방법은 optimization 문제에서 자주 사용되며, 큰 상태공간에서도 비교적 단순하게 적용할 수 있다.

Hill Climbing의 장점은 첫째, 구현이 매우 단순하다는 점이다. 둘째, 한 번에 현재 상태와 이웃만 보면 되므로 메모리 사용량이 매우 작다. 셋째, 빠르게 괜찮은 해를 구할 수 있어 큰 문제에서 실용적이다.

반면 단점도 분명하다. 첫째, local optimum 에 도달하면 더 좋은 이웃이 없어서 멈추지만, 그것이 전체 최적해(global optimum)라는 보장은 없다. 둘째, plateau 나 ridge 같은 지형에서는 탐색이 정체될 수 있다. 셋째, 따라서 일반적으로 완전성과 최적성을 보장하지 않는다.

이러한 한계를 보완하기 위해, 서로 다른 초기 상태에서 여러 번 다시 시작하는 random restart hill climbing 을 함께 사용하는 경우가 많다.

**핵심 채점 포인트**
- local search는 경로보다 현재 상태 자체의 개선에 관심이 있다는 점을 먼저 적는다.
- 예시 알고리즘으로 Hill Climbing 을 제시한다.
- 장점은 단순성, 적은 메모리, 빠른 근사해 탐색이다.
- 단점은 local optimum, plateau, ridge, 그리고 최적성/완전성 미보장이다.

**암기 한줄**
> Hill climbing은 현재 상태를 계속 개선하는 간단한 local search이지만, local optimum에 쉽게 갇히므로 random restart 같은 보완이 필요하다.

### 4. Clustering 방법 중 single-link clustering과 complete-link clustering에 대해 적고 장단점을 적기

**모범답안**

Single-link clustering과 complete-link clustering은 계층적 군집화(hierarchical clustering), 그중에서도 agglomerative clustering 에서 두 cluster 사이의 거리를 정의하는 방법이다. Agglomerative clustering은 처음에 각 데이터를 하나의 cluster로 두고, 가장 가까운 두 cluster를 반복적으로 합쳐 가며 최종적으로 트리 형태의 군집 구조를 만든다.

먼저 single-link clustering 은 두 cluster 사이의 거리 또는 유사도를 계산할 때, 가장 가까운 두 점만을 기준으로 삼는 방법이다. 거리 기준으로 쓰면 다음과 같이 표현할 수 있다.

`d_single(A, B) = min_{x in A, y in B} d(x, y)`

즉, 두 cluster 사이에 한 쌍이라도 매우 가까운 점이 있으면 두 cluster는 가깝다고 본다. 장점은 길게 늘어진 형태나 비구형(non-spherical) 구조의 군집도 잘 찾을 수 있다는 점이다. 또한 비교적 유연하게 cluster를 연결한다. 단점은 가까운 점들이 연쇄적으로 이어지면서 긴 사슬처럼 합쳐지는 chaining 현상이 발생하기 쉽고, 그 결과 군집의 응집도가 떨어질 수 있다는 점이다. 또한 잡음이나 이상치에 영향을 받을 수 있다.

다음으로 complete-link clustering 은 두 cluster 사이의 거리를 계산할 때, 가장 먼 두 점을 기준으로 삼는다.

`d_complete(A, B) = max_{x in A, y in B} d(x, y)`

즉, 두 cluster를 합치려면 cluster 내부의 모든 점들이 비교적 서로 가까워야 한다는 뜻이다. 장점은 응집력 있고 compact한 cluster 를 만들기 쉽고, single-link처럼 chaining이 심하게 나타나지 않는다는 점이다. 단점은 합병 기준이 보수적이므로 길게 늘어진 자연스러운 군집을 잘 못 잡을 수 있고, 먼 점 하나나 이상치 때문에 cluster 간 거리가 크게 계산되어 합병이 지나치게 늦어질 수 있다.

정리하면, single-link는 가장 가까운 점 기준이므로 유연하지만 chaining 문제가 있고, complete-link는 가장 먼 점 기준이므로 더 조밀한 군집을 만들지만 보수적이라는 차이가 있다.

**핵심 채점 포인트**
- 두 방법 모두 hierarchical agglomerative clustering의 linkage 기준이라는 점을 먼저 적는다.
- single-link는 가장 가까운 점쌍, complete-link는 가장 먼 점쌍 기준이다.
- single-link의 단점으로 chaining 을 반드시 적는 것이 좋다.
- complete-link의 장점으로 compact cluster 형성을 쓰면 답안이 좋아진다.

**암기 한줄**
> Single-link는 가장 가까운 점만 보고 합쳐서 길게 이어진 군집이 생기기 쉽고, complete-link는 가장 먼 점까지 고려해 더 조밀한 군집을 만든다.

### 5. Uninformed Search와 Informed Search의 중대한 차이점을 쓰고, 각각 아는 대로 서술하기

**모범답안**

Uninformed Search와 Informed Search의 가장 중요한 차이는 heuristic 정보를 사용하는지 여부이다. Uninformed Search는 목표까지 얼마나 가까운지에 대한 추가 정보를 쓰지 않고, 문제에서 주어진 상태공간과 비용 정보만으로 노드를 확장한다. 대표적으로 BFS, DFS, IDS, UCS가 있다. BFS는 얕은 노드를 먼저 보아 단위 비용에서는 최단 단계 해를 찾기 좋지만 메모리가 크고, DFS는 메모리는 적지만 무한 경로나 비최적해 문제가 있다. IDS는 DFS를 depth limit로 반복하여 BFS의 얕은 해 우선 성질과 DFS의 메모리 장점을 절충하고, UCS는 누적 비용이 가장 작은 노드를 먼저 확장해 비용이 다른 문제에서 최소 비용 해를 찾는 데 적합하다.

반면 Informed Search는 heuristic을 사용하여 더 유망한 노드를 먼저 확장한다. Greedy best-first search는 목표까지의 추정 비용 `h(n)`만 보고 선택하므로 빠르게 목표 쪽으로 갈 수 있지만 최적성을 보장하기 어렵다. A* search는 `f(n) = g(n) + h(n)`을 사용하여 지금까지의 실제 비용과 앞으로의 추정 비용을 함께 고려한다. 그래서 admissible heuristic이 주어지면 optimal solution을 찾을 수 있다. 정리하면 Uninformed Search는 정보가 적어 일반적이지만 탐색량이 커질 수 있고, Informed Search는 heuristic을 통해 탐색 효율을 높이지만 heuristic의 품질에 크게 의존한다.

**핵심 채점 포인트**
- 핵심 차이: heuristic 사용 여부
- Uninformed Search 대표: BFS, DFS, IDS, UCS
- Informed Search 대표: Greedy best-first, A*
- Greedy는 `h(n)`, A*는 `g(n) + h(n)`
- uninformed는 일반적이지만 비효율 가능, informed는 효율적일 수 있지만 heuristic 의존

**암기 한줄**
> Uninformed Search는 heuristic 없이 BFS/DFS/IDS/UCS처럼 정해진 확장 기준으로 탐색하고, Informed Search는 heuristic을 사용해 유망한 노드를 먼저 보며 Greedy는 `h(n)`, A*는 `g(n) + h(n)`을 쓴다.

### 6. Local search에 대해서 설명하기

**모범답안**

Local search는 목표까지의 전체 경로를 저장하고 확장하는 방식이 아니라, 현재 상태 하나를 중심으로 이웃 상태를 비교하며 더 좋은 상태로 이동하는 탐색 방법이다. 이 방법은 path보다 state 자체의 품질이나 목적함수 값을 중요하게 본다. 대표적인 예는 hill climbing이며, 현재 상태보다 더 좋은 이웃이 있으면 그쪽으로 이동하는 과정을 반복한다.

Local search의 장점은 메모리를 적게 사용하고, 상태공간이 매우 크거나 경로 자체보다 좋은 해를 찾는 것이 중요한 optimization 문제에 잘 맞는다는 점이다. N-Queens 같은 문제처럼 완전한 경로보다 현재 배치의 품질을 개선하는 문제가 대표적이다. 단점은 local optimum, plateau, ridge에 빠져 전역 최적해를 찾지 못할 수 있다는 점이다. 이를 완화하기 위해 여러 초기 상태에서 다시 시작하는 random restart를 사용할 수 있다.

**핵심 채점 포인트**
- 현재 상태 중심 탐색
- path보다 objective/state quality를 중시
- hill climbing 예시
- 장점: 메모리 효율, optimization 적합
- 단점: local optimum, plateau, ridge
- 보완: random restart

**암기 한줄**
> Local search는 현재 상태만 유지하면서 이웃 상태 중 더 좋은 상태로 이동하는 탐색이다. 메모리가 적게 들고 optimization 문제에 적합하지만, local optimum이나 plateau에 갇힐 수 있어 random restart로 보완한다.

### 7. Adversarial Search에 대해 설명하고, Informed Search와의 중대한 차이점 쓰기

**모범답안**

Adversarial Search는 상대가 존재하는 게임 환경에서 최선의 행동을 고르는 탐색이다. 일반적인 설정은 두 플레이어가 번갈아 행동하고, 한쪽의 이득이 다른 쪽의 손해가 되는 zero-sum game이다. 대표 방법인 minimax는 상대도 최적으로 행동한다고 가정한다. MAX는 자신의 utility를 최대화하려 하고, MIN은 MAX의 utility를 최소화하려 하므로, terminal state나 depth limit에서 계산한 값을 위로 올려 루트에서 최선의 수를 결정한다.

Informed Search와의 중대한 차이는 문제의 목적과 평가 기준이다. Informed Search는 heuristic을 사용하여 목표 상태까지의 path를 더 효율적으로 찾는 방법이다. Greedy best-first search는 `h(n)`을, A*는 `g(n) + h(n)`을 사용하여 어떤 node를 먼저 확장할지 정한다. 즉 Informed Search의 heuristic은 목표까지의 거리나 비용을 추정하는 정보이다.

반면 Adversarial Search는 목표까지의 단일 경로를 찾는 문제가 아니라, 상대가 내 선택에 반응하는 상황에서 전략을 고르는 문제이다. 평가값도 목표까지의 거리라기보다 게임의 승패, utility, 또는 position의 유리함을 나타낸다. 따라서 Informed Search가 어떤 경로가 목표에 가까운가를 묻는다면, Adversarial Search는 상대가 최적으로 대응해도 어떤 행동이 가장 유리한가를 묻는다고 정리할 수 있다.

**핵심 채점 포인트**
- adversarial search: 상대 존재, turn-taking, zero-sum, minimax
- minimax: MAX/MIN과 utility backup
- informed search: heuristic 기반 path search
- 핵심 차이: path finding vs strategy decision
- 평가 기준 차이: distance/cost estimate vs utility/game outcome

**암기 한줄**
> Adversarial Search는 상대가 있는 게임에서 minimax처럼 상대의 최적 대응까지 고려해 utility가 좋은 전략을 고르고, Informed Search는 heuristic으로 목표까지의 경로를 효율적으로 찾는다.

## 예상문제

아래 문항들은 실제 기출과 비슷한 형식으로 추가 구성한 예상문제와 모범답안이다.

### 예상 1. Minimax algorithm을 설명하고, alpha-beta pruning의 원리와 장점을 적기

**모범답안**

Minimax algorithm은 두 명의 플레이어가 번갈아 수를 두는 zero-sum game 에서 사용하는 탐색 알고리즘이다. 이 알고리즘은 상대 역시 항상 최적으로 행동한다고 가정한다. 따라서 MAX 노드는 자신의 이득을 최대화하려 하고, MIN 노드는 MAX의 이득을 최소화하려 한다.

작동 원리는 게임 트리의 말단 노드 또는 깊이 제한 지점에서 utility 값 또는 evaluation 값을 구한 뒤, 그 값을 위로 올려 보내는 것이다. MIN 노드에서는 자식들 중 최소값을 선택하고, MAX 노드에서는 자식들 중 최대값을 선택한다. 이렇게 루트까지 값을 전파하면 현재 상태에서 어떤 수를 두는 것이 최선인지 결정할 수 있다.

Alpha-beta pruning은 minimax의 계산량을 줄이기 위한 가지치기 기법이다. 여기서 alpha는 현재까지 MAX가 확보할 수 있는 최선값, beta는 현재까지 MIN이 허용하는 상한값으로 생각할 수 있다. 탐색 도중 어떤 노드가 이미 기존의 `alpha` 와 `beta` 관계 때문에 최종 선택에 영향을 줄 수 없다는 것이 분명해지면, 그 아래 서브트리는 더 이상 탐색하지 않고 잘라낸다.

이 방법의 장점은 minimax와 같은 결정을 유지하면서도 탐색해야 하는 노드 수를 크게 줄일 수 있다는 점이다. 즉 정답은 그대로 두고 계산량만 줄인다. 특히 좋은 move ordering이 주어질수록 pruning 효과가 더 커진다. 단, 노드 정렬이 나쁘면 가지치기 효과가 작을 수 있다.

**핵심 채점 포인트**
- minimax는 상대도 최적으로 행동한다는 가정을 둔다.
- MAX는 최대값, MIN은 최소값을 선택하며 값을 위로 올린다.
- alpha-beta pruning은 결과를 바꾸지 않고 불필요한 서브트리를 제거한다.
- 장점은 탐색량 감소와 계산 효율 향상이다.

**암기 한줄**
> Minimax는 상대도 최적으로 둔다고 가정해 최선 수를 고르고, alpha-beta pruning은 같은 답을 유지한 채 영향 없는 가지를 덜 본다.

### 예상 2. Constraint Satisfaction Problem을 정의하고, backtracking과 forward checking을 설명하기

**모범답안**

Constraint Satisfaction Problem(CSP)은 변수들의 집합, 각 변수에 들어갈 수 있는 도메인(domain), 그리고 반드시 만족해야 하는 제약 조건(constraints) 으로 이루어진 문제이다. 즉 일반 탐색 문제처럼 목표 상태를 직접 찾기보다, 각 변수에 값을 할당하여 모든 제약을 만족시키는 해를 찾는 것이 목적이다. 대표적인 예로 map coloring, N-Queens 문제가 있다.

Backtracking은 CSP를 해결하는 가장 기본적인 방법이다. 변수 하나를 선택하고 가능한 값을 넣어 본 뒤, 제약을 위반하지 않으면 다음 변수로 진행한다. 만약 어느 시점에서 제약을 만족하는 값이 더 이상 없으면 이전 단계로 되돌아가 다른 값을 시도한다. 즉 depth-first search 기반의 되돌아가기 방식이라고 볼 수 있다.

Forward checking은 현재 어떤 값을 할당했을 때, 앞으로 남은 변수들의 도메인에 어떤 영향이 생기는지를 미리 검사하는 방법이다. 예를 들어 지금의 할당 때문에 미래 변수의 가능한 값이 모두 사라진다면, 그 아래 탐색은 어차피 해가 없으므로 바로 backtracking할 수 있다. 따라서 forward checking은 실패를 더 일찍 발견하게 해 탐색 비용을 줄이는 역할을 한다.

정리하면, CSP는 변수-도메인-제약으로 표현되는 문제이고, backtracking은 값을 하나씩 넣어 보며 막히면 되돌아가는 기본 알고리즘이며, forward checking은 미래의 실패를 미리 감지하여 불필요한 탐색을 줄이는 보조 기법이다.

**핵심 채점 포인트**
- CSP는 변수, 도메인, 제약으로 정의된다고 적어야 한다.
- backtracking은 값 할당 후 막히면 되돌아가는 DFS 기반 방법이다.
- forward checking은 남은 변수 도메인을 미리 검사해 조기 실패를 감지한다.
- 일반 탐색 문제와 달리 제약 만족이 중심이라는 점을 넣으면 좋다.

**암기 한줄**
> CSP는 변수에 값을 넣어 제약을 만족시키는 문제이고, backtracking은 막히면 되돌아가며, forward checking은 미래 실패를 미리 본다.

### 예상 3. kNN classifier를 설명하고 특징, 장단점을 적기

**모범답안**

kNN(k-Nearest Neighbors) classifier는 새로운 데이터가 들어왔을 때, 기존 학습 데이터 중에서 가장 가까운 k개의 이웃을 찾고 그 이웃들의 다수결로 클래스를 결정하는 분류 방법이다. 즉 미리 복잡한 모델을 만드는 것이 아니라, 분류가 필요한 시점에 저장된 데이터를 참조하여 판단한다. 이런 이유로 kNN은 instance-based learning 또는 lazy learning 으로 설명된다.

kNN의 특징은 분류 기준이 데이터 간의 거리(distance) 또는 유사도(similarity) 에 크게 의존한다는 점이다. 따라서 어떤 거리 척도를 쓰는지, 각 속성이 정규화되어 있는지, 그리고 `k` 값을 어떻게 정하는지가 매우 중요하다. `k` 가 너무 작으면 noise에 민감하고, 너무 크면 주변 구조를 흐리게 볼 수 있다.

장점은 알고리즘이 매우 단순하고 직관적이며, 별도의 복잡한 학습 단계가 거의 필요 없다는 점이다. 또한 데이터가 충분하고 적절한 거리 척도를 사용하면 꽤 좋은 성능을 낼 수 있다. 단점은 새로운 데이터를 분류할 때마다 학습 데이터를 많이 비교해야 하므로 예측 시 계산량이 크다는 점이다. 또한 `k` 값, 거리 함수, 정규화 여부, 이상치와 고차원 데이터에 민감하다는 한계가 있다.

**핵심 채점 포인트**
- kNN은 가까운 k개의 이웃의 다수결로 분류한다.
- lazy learning 또는 instance-based learning이라는 성격을 적으면 좋다.
- 장점은 단순성, 학습 부담이 작음이다.
- 단점은 예측 시 계산량, k/거리 척도/정규화에 대한 민감성이다.

**암기 한줄**
> kNN은 가까운 예제들을 보고 다수결로 분류하는 lazy learning이며, k와 거리 정의에 민감하고 예측 시 계산량이 크다.

### 예상 4. Clustering과 classification의 차이를 말하고, k-means clustering의 기본 절차와 한계를 적기

**모범답안**

Clustering은 정답 label이 없는 데이터를 서로 비슷한 것끼리 묶는 unsupervised learning 방법이다. 반면 classification은 이미 label이 붙은 학습 데이터를 바탕으로 새로운 데이터의 클래스를 예측하는 supervised learning 방법이다. 즉 clustering은 숨겨진 구조를 찾는 것이 목적이고, classification은 정해진 범주로 분류하는 것이 목적이라는 차이가 있다.

k-means clustering은 대표적인 flat clustering 알고리즘이다. 먼저 `K` 개의 cluster 수를 정하고 초기 centroid를 선택한다. 그 다음 각 데이터를 가장 가까운 centroid에 할당한다. 이후 각 cluster에 속한 데이터들의 평균으로 centroid를 다시 계산한다. 그리고 다시 각 데이터를 가까운 centroid에 재할당하는 과정을 반복하여, 더 이상 cluster assignment가 크게 변하지 않으면 종료한다.

이 알고리즘의 장점은 구현이 단순하고 계산이 비교적 빠르다는 점이다. 따라서 큰 데이터에 적용하기도 쉽다. 하지만 한계도 있다. 첫째, `K` 값을 미리 정해야 한다. 둘째, 초기 seed에 따라 결과가 달라질 수 있다. 셋째, local optimum에 머무를 수 있다. 넷째, 구형 또는 centroid 중심의 군집에는 잘 맞지만 길게 늘어진 구조나 이상치가 많은 데이터에는 적합하지 않을 수 있다.

**핵심 채점 포인트**
- clustering은 label 없음, classification은 label 있음이라는 차이를 먼저 쓴다.
- k-means 절차는 초기 centroid 선택 -> 할당 -> centroid 재계산 -> 반복이다.
- 장점은 단순성, 속도다.
- 한계는 K를 미리 정해야 함, seed 민감성, local optimum, 비구형 군집에 약함이다.

**암기 한줄**
> Clustering은 label 없이 묶고, classification은 label을 예측한다. k-means는 centroid 할당과 재계산을 반복하지만 K와 초기 seed에 민감하다.

### 예상 5. Reinforcement learning이 supervised learning과 다른 점을 설명하고, Q-learning의 기본 아이디어를 적기

**모범답안**

Reinforcement learning은 agent가 환경과 상호작용하면서 행동을 하고, 그 결과로 받는 reward 를 바탕으로 어떤 행동이 좋은지 학습하는 방법이다. supervised learning은 입력에 대해 정답 label이 주어진 데이터를 사용해 학습하지만, reinforcement learning에서는 보통 정답 행동이 직접 주어지지 않는다. 대신 여러 행동을 시도해 보고 장기적으로 더 큰 보상을 주는 정책(policy)을 학습한다는 점이 다르다.

또한 supervised learning은 보통 한 번의 예측 정확도를 높이는 데 초점이 있지만, reinforcement learning은 순차적 의사결정이 중요하다. 현재 행동이 미래 상태와 미래 보상에 영향을 주므로, 즉시 보상뿐 아니라 장기 누적 보상을 고려해야 한다.

Q-learning은 강화학습의 대표적인 알고리즘으로, 상태 `s` 에서 행동 `a` 를 했을 때의 가치를 `Q(s, a)` 로 두고 이를 반복적으로 갱신한다. 핵심은 현재 얻은 보상과 다음 상태에서 기대할 수 있는 최대 미래 보상을 함께 반영하는 것이다.

`Q(s, a) <- Q(s, a) + alpha * [r + gamma * max_{a'} Q(s', a') - Q(s, a)]`

여기서 `alpha` 는 학습률, `gamma` 는 미래 보상에 대한 discount factor이다. 학습이 충분히 이루어지면 각 상태에서 `Q` 값이 가장 큰 행동을 선택하여 정책을 결정할 수 있다. 즉 Q-learning의 핵심은 상태-행동 가치표를 점점 정확하게 만들어 최선 행동을 고르는 것이다.

**핵심 채점 포인트**
- RL은 label이 아니라 reward로 학습한다.
- supervised learning과 달리 순차적 의사결정과 장기 보상이 중요하다.
- Q-learning은 `Q(s, a)` 를 갱신하는 방식이다.
- 정책은 보통 가장 큰 Q값의 행동 선택으로 결정된다.

**암기 한줄**
> 강화학습은 정답 label 대신 reward로 정책을 배우고, Q-learning은 상태-행동 가치를 갱신해 가장 큰 Q값의 행동을 선택한다.

### 예상 6. Perceptron의 기본 구조를 설명하고, XOR 문제에서 single-layer perceptron의 한계와 multilayer perceptron의 필요성을 적기

**모범답안**

Perceptron은 여러 입력값 `x_1, x_2, ..., x_n` 에 각각의 가중치(weight)를 곱한 뒤 이를 합하고, bias를 더한 다음 activation function을 통과시켜 출력을 내는 기본적인 인공신경망 단위이다.

`y = f(sum_i w_i * x_i + b)`

여기서 weight는 각 입력의 중요도를 조절하고, bias는 결정 경계를 이동시키는 역할을 한다. activation function은 단순 선형결합 결과를 최종 출력으로 변환해 주는 역할을 한다.

하지만 single-layer perceptron은 linearly separable 한 문제만 해결할 수 있다. 즉 하나의 직선이나 초평면으로 데이터를 나눌 수 있어야 한다. XOR 문제는 입력 공간에서 정답 클래스가 한 직선으로 분리되지 않으므로, single-layer perceptron 하나만으로는 해결할 수 없다. 이것이 단층 perceptron의 대표적인 한계이다.

이 한계를 극복하기 위해 multilayer perceptron(MLP) 이 필요하다. hidden layer를 두면 입력을 더 복잡한 비선형 형태로 변환할 수 있어 XOR 같은 문제도 표현할 수 있다. 그리고 backpropagation은 출력 오차를 뒤에서 앞으로 전파하여 각 weight를 gradient descent 방식으로 갱신하는 학습 알고리즘이다. 따라서 MLP와 backpropagation은 단층 perceptron이 해결하지 못하는 복잡한 비선형 문제를 학습하기 위해 필요하다.

**핵심 채점 포인트**
- perceptron은 가중합 + bias + activation 구조다.
- weight, bias, activation의 역할을 간단히 설명해야 한다.
- XOR는 linearly separable하지 않다는 점을 써야 한다.
- hidden layer와 backpropagation이 필요한 이유를 연결해야 한다.

**암기 한줄**
> Perceptron은 가중합과 bias를 activation에 넣는 unit이고, XOR는 직선 하나로 나눌 수 없어 hidden layer와 backpropagation이 필요하다.
