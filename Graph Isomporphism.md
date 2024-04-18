
# Isomorphic Graph

Isomorphic graph는 노드의 ordering의 차이로 인해 다르게 표현되지만 완전히 같은 구조를 가지고 있는 그래프.

그래프의 adjacency matrix를 A, node feature를 X라고 할 때, **다음 식을 만족하는 [permutation matrix](Permutation) P가 존재한다면 우리는 그래프 1과 2가 isomorphic**하다고 말할 수 있다.


$PA_1P^T = A_2$ and $PX_1 = X_2$

즉, 어떤 permutation을 통해 adjacency matrix를 같게 만들 수 있고, 이 permutation으로 node feature matrix까지 같게 만들 수 있다면 우리는 두 그래프를 isomorphic하다고 표현할 수 있는 것이다.


Graph isomorphism의 정의는 간단하다. 하지만 두 그래프가 isomorchic한지의 여부를 검증하는 것은매우 어려운 문제이다. 우리의 목적은, 위에서 언급한 graph isomorphism식을 만족하는 $P$를 찾는것이다. 이를 식으로 표현하면 다음과 같이 표현한 수 있다.

$min_{P\in\mathcal{P}}||PA_1P^T - A_2|| + ||PX_1 - X_2|| =^? 0.$

우리가 원하는 최적의 $P$는 위 식을 0으로 만들어 줄 것이다.
그러므로 우리는 graph isomorphism test 문제를 위 식을 minimize하는 P를 찾는 optimization 문제로 바꿀 수 있다. 즉, 모든 존재할 수 있는 permutation matrix에 대해서 위 식을 0으로 만들 수 있는 P를 찾는 것이다. 모든 permutation matrix를 전부 search해야 하기 때문에 이의 complexity는 O(|V|!)이다.

# 활용

Graph isomorphism은 graph representation의 성능을 평가할 때 사용되기도 한다. 즉 우리가 도출한 graph representation이 graph isomorphism을 test하는데 얼마나 잘 사용될 수 있는지를 평가해서 graph representation (z)의 성능을 평가하는 것이다. 이를 식으로 표현하면 다음과 같다.

$Z_{\mathcal{G_1}} = Z_{\mathcal{G_2}}$ if and only if $\mathcal{G_1}$ is isomorphic to $\mathcal{G_2}$.

즉, 두 그래프의 representation이 같은 것과 두 그래프가 isomorphism한 것이 서로 동치이면 우리는 그래프의 representation이 완벽하게 학습되었다고 평가한다.
