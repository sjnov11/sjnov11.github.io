---
layout: post
title: "크루스칼 알고리즘 (Kruskal's Algorithm) 과 프림 알고리즘 (Prim's Algorithm)"
slug: "kruskal_prim"
date: 2018-08-20 23:46:00 +0900
categories: blog/algorithm
---



# 크루스칼 알고리즘 (Kruskal's Algorithm) 과 프림 알고리즘 (Prim's Algorithm)

크루스칼 알고리즘과 프림 알고리즘은 Undirected Graph 에서 최소 신장 트리 (MST) 를 구하는 알고리즘이다. 기본적으로 Greedy 알고리즘으로, 항상 최소 가중치의 간선을 선택한다. 다만 크루스칼 알고리즘은 간선을 중심으로 최소 간선으로 연결하면서 사이클 여부를 확인하고, 프림 알고리즘은 정점들을 중심으로 정점에서 나가는 간선 중 최소 간선을 선택한다.



## 크루스칼 알고리즘

### 알고리즘 순서

1) 그래프의 모든 간선(edge) 를 끊어 둔다.
2) 간선들을 가중치가 낮은 순서로 정렬한다. 
3) 현재 가장 낮은 가중치를 갖는 간선을 선택한다. 
4) 선택한 간선의 두 정점(vertex) 이 사이클이 형성하지 않는다면 두 정점을 연결한다. 
5) 선택한 간선을 제외한다. 
6) V-1개의 간선이 선택되었으면 종료, 아니면 다시 3)부터 수행한다.

![kruskal.jpg](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/08/20/kruskal.jpg?raw=true)

4) 에서 Cycle detection은 Union-Find 알고리즘을 이용한다.



### 시간 복잡도

2)에서 간선들의 정렬에 $$O(E\log{E})$$ 의 시간복잡도를, 
3)에서 Cycle detection에 $$O(\log{V})$$ 를 최대 $$E$$ 번 수행하므로, $$O(E\log{V})$$ 의 시간복잡도를 갖는다. 여기서 $$E$$ 는 최대 $$V^2$$ 개 있을 수 있기 때문에, $$O(E\log{V})$$ 이다.

![prim.jpg](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/08/20/prim.jpg?raw=true)



## 프림 알고리즘

### 알고리즘 순서

1) 임의의 정점(vertex) 을 선택하여 MST에 포함시킨다.
2) MST에 있는 정점과 MST에 없는 정점 사이의 간선(edge) 중 가중치가 최소인 간선을 찾는다.
3) 찾은 간선의 두 노드 중 MST에 없는 노드를 MST에 포함시킨다.
4) 모든 노드가 T에 포함될 때 까지 2)와 3)을 반복한다.

```c++
int prim(int start) {
	priority_queue<pair<int, int>> q;
	q.push(make_pair(0, start));

	int ret = 0;
	while (!q.empty()) {
		int cur = q.top().second;
		int cur_w = q.top().first;
		q.pop();

		if (visited[cur]) continue;
		visited[cur] = true;
		ret += cur_w*(-1);
		for (auto next : adj_list[cur]) {
			int v = next.first;
			int w = next.second;

			q.push(make_pair(w*(-1), v));
		}
	}
	return ret;
}
```



### 시간 복잡도

하나의 정점(vertex) 에서 나가는 이진 힙(priority_queue) 에는 최대 $$V$$ 개의 간선 정보가 있을 수 있다. 따라서, 최악의 경우 $$V$$ 번 pop을 하여야 다음 방문할 정점을 구할 수 있으므로, 이진 힙에서 방문하지 않은 정점을 고를 때, 최대 $$O(V\log{E})$$ 의 시간이 걸린다. 크루스칼과 마찬가지로 $$E \leq V^2$$ 이므로 $$O(V\log{V})$$ 이다. 

현재 정점과 연결된 다음 정점의 간선 정보를 이진 힙에 넣는 부분(while문에 중첩된 for문)에서는 모든 간선에 대해서 한번씩 수행된다. 따라서 시간 복잡도는 $$O(E\log{E})$$ 이고, $$E\leq V^2$$ 이므로 $$O(E\log{V})$$ 이다.

따라서, 총 시간복잡도는 $$O(V\log{V}) + O(E\log{V})$$ 이므로 $$O(E\log{V})$$ 가 된다.

### 문제

#1922: https://www.acmicpc.net/problem/1922