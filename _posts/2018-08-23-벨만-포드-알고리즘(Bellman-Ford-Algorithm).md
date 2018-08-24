---
layout: post
title: "벨만-포드 알고리즘(Bellman-Ford Algorithm)"
slug: "bellman-ford"
date: 2018-08-23 19:46:00 +0900
categories: blog/algorithm
---

# Bellman-Ford Algorithm

벨만-포드 알고리즘은 최단 경로를 구하는 대표적인 알고리즘 중 하나이다. 
다익스트라와 마찬가지로 relaxation 과정을 통해 최단 경로를 구한다. relaxation이란, 점진적으로 더 정확한 값으로 변경해나가면서 마지막에 optimal solution 에 도달하는 것을 말한다. 최단 경로의 relaxation 과정은 다음과 같다.

> - 지금까지 나는 노드 $$v$$ 까지의 최단 거리를 dist[v] 라고 알고 있어. 근데 알고보니 $$u$$ 를 거쳐 $$v$$ 로 가는 dist[u] + weight(u,v) 가 더 짧은 거리인걸 알았고 dist[v] 를 이것으로 바꿀꺼야.
>
> [출처 : https://stackoverflow.com/questions/2592769/what-is-the-relaxation-condition-in-graph-theory]



처음에는 모든 거리 값을 "over-estimate" 하게 둔다. 그리고 새로 찾은 path의 길이가 더 짧은 거리라면 그 값으로 바꾼다. 다익스트라는 priority queue로 greedy 한 방식으로 다음 edge를 선택하여 relaxation을 수행하는 반면에, 벨만-포드 알고리즘은 모든 edge에 대하여 relaxation을 수행하는 차이가 있다



## 알고리즘 순서

1. 모든 정점(vertex)의 거리를 무한대 $$\infty$$ 로 둔다.
2. 시작 정점(source)의 거리를 0으로 둔다.
3. 모든 간선(edge)에 대해서 relaxation을 $$  (V-1)$$ 회 수행한다.
4. 모든 간선(edge)에 대해 relaxation을 한번 더 수행한다. 만약 거리의 update가 일어난다면, negative cycle이 존재한다.



### Psuedo-code

```c++
for v in V:
	v.distance = INFINITY
	v.parent = None
source.distance = 0
for i range(1, V-1):
	for (u, v) in E:
		relax(u, v)
for (u, v) in E:
	if v.distance > u.distance + weight(u, v):
		print "Negative cycle!"
```

```c++
relax(u, v):
	if v.distance > u.distance + weight(u, v):
		v.distance = u.distance + weight(u, v)
		v.parent = u
```





## 몇 번의 relaxation을 해야하는가?

다익스트라 알고리즘과 벨만-포드 알고리즘의 차이 중 하나가 바로 relaxation의 횟수이다. 다음 그래프를 살펴보자. 시작점은 A이다.



![img](https://upload.wikimedia.org/wikipedia/commons/thumb/8/85/Bellman-Ford_worst-case_example.svg/220px-Bellman-Ford_worst-case_example.svg.png)



다익스트라 알고리즘의 경우는 A에서 부터 차례대로 거리를 계산한다. Greedy 기법으로 한번의 relaxation으로 모든 거리가 optimal 함이 보장이 된다.

하지만 벨만-포드 알고리즘은 모든 간선에 대해서 relaxation을 한다. Relaxation을 왼쪽에서 오른쪽 순서로 진행한다면 한번의 relaxation으로 최단 거리를 구할 수 있지만, 오른쪽에서 왼쪽 순서로 진행한다면 총 4회의 relaxation을 수행을 하여야 한다. 이를 일반화 해보자.

$$s$$ 에서 $$t$$ 로 가는 가장 짧은 경로 $$s, v_1, v_2, ..., v_k, t$$ 가 있다고 하자. 이 최단 경로는 최대 $$V -1 $$ 개의 간선이 있을 수 있다. 첫번째 relaxation에서 간선 $$(s, v1)$$  이 relaxation 됨이 보장된다. 지금 당장은 $$v1$$ 이 어떤 정점인지는 알 수는 없지만, $$v1$$ 의 후보들 모두 정확한 거리가 계산되어 진다. 두번째 relaxation에서는 $$(v1,v2)$$ 가 보장되고, 이것이 끝까지 계속된다.
따라서, 최단경로가 최대 $$V-1$$ 개의 간선으로 이루어져 있으므로, $$V-1$$ 회 relaxation을 수행한다면 모든 정점의 거리가 정확히 계산된다.

[참고: https://cs.stackexchange.com/questions/6894/bellman-ford-algorithm-why-can-edges-be-updated-out-of-order/6914#6914]



## 시간 복잡도

모든 간선$$(E)​$$ 에 대해서 Relaxation을 총 $$V-1​$$ 회 수행한다. 따라서 시간 복잡도는 $$O(VE)​$$ 가 된다.