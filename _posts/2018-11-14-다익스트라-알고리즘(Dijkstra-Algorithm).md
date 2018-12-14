---
layout: post
title: "다익스트라 알고리즘(Dijkstra Algorithm)"
slug: "dijkstra"
date: 2018-11-14 16:32:00 +0900
categories: blog/algorithm
---



# 다익스트라 알고리즘 (Dijkstra Algorithm)

다익스트라는 **한 정점에서 다른 정점으로의 최단거리**를 구할 때 쓰는 알고리즘입니다. 다익스트라는 다른 알고리즘보다 빠른 속도로 최단거리를 구할 수 있지만 **음수 간선이 존재하는 경우에는 사용할 수 없는 단점**이 있습니다. 이 경우에는 벨만-포드 알고리즘을 사용하는 것이 좋습니다. 



### 알고리즘

1. *ShortestPath*에 포함되는 정점 set과 그렇지 않은 정점 set을 나눕니다.
   초기에는 포함되는 정점 set은 비어있습니다. 

2. 주어진 Graph의 모든 정점에 대하여 출발점으로부터의 거리(*dist*)를 INFINITE로 초기화 시킵니다. 시작 정점은 0입니다.

3. 모든 정점이 *ShortestPath* set에 포함될 때까지 아래를 수행합니다.

   1. *ShortestPath*에 포함되지 않은 정점 중 가장 *dist* 값이 작은 정점 *u* 를 고릅니다.

   2. *u*를 *ShortestPath* 에 포함시킵니다.

   3. *u* 에 인접하고 있는 정점들(*v*)에 대해, *v* 의 현재 *dist* 보다 *u* 의 *dist* + *weight(u,v)* 가 크다면 *v* 의 *dist* 를 갱신해 줍니다.
      $$dist[v] = \min(dist[v], dist[u] + weight(u,v))$$


아래 코드에서는 *ShortestPath* 포함 여부를 *visited* 배열로 하고, Priority Queue 를 통해 *ShortestPath* 에 포함되지 않는 가장 *dist* 가 작은 정점을 고릅니다.



### C++ 코드

```c++
vector<int> dijkstra(int start) {
	vector<int> dist(V+1, INF);
	vector<bool> visited(V+1, false);
	priority_queue<pair<int, int>> pq;
	dist[start] = 0;
	pq.push(make_pair(0, start));

	while (!pq.empty()) {
		int u = pq.top().second;
		pq.pop();

		if (visited[u]) continue;
		visited[u] = true;

		for (auto adj : adj_list[u]) {
			int v = adj.first;
			int w = adj.second;

			if (dist[v] > dist[u] + w) {
				dist[v] = dist[u] + w;
				pq.push({ dist[v] * (-1), v });
			}

		}
	}
	return dist;
}
```

