---
layout: post
title: "Topological Sort(위상 정렬)"
slug: "topological-sort"
date: 2018-09-29 18:46:00 +0900
categories: blog/algorithm
---



# Topological Sort(위상 정렬)

## Topological Sort(위상 정렬)이란?

**위상 정렬**은 사이클이 없는 방향 그래프(**DAG**: Directed Acyclic Graph) 에서 방향성을 거스리지 않게 정점(vertex)들을 나열하는 알고리즘이다. 

위 그래프는 사이클이 없는 방향 그래프(DAG) 이다. 정점 4에 정점 1과 정점 2가 연결되어 있는데, 이는 1과 2가 4보다 먼저 나오도록 정렬해야함을 의미한다. 일반화 하면, 간선 ***(u, v)*** 가 있을 경우 정점 **u** 는 정점 **v** 보다 항상 먼저 나와야한다.

위상정렬을 가장 잘 설명해 줄 수 있는 예로 대학의 선이수과목(prerequisite) 구조를 예로 들 수 있다. 만약 특정 수강과목에 선수과목이 있다면 그 선수 과목부터 수강해야 한다. 이와 같이 **선후 관계가 정의된 그래프 구조 상에서 선후 관계에 따라 정렬하기 위해 위상 정렬을 이용**할 수 있다.

마지막으로 위상 정렬의 결과는 여러가지가 될 수 있다. 위의 예에서는 $$[1,2,3,4,5,6], [1,2,4,3,5,6], [1,2,4,6,3,5]$$ 등이 있다.



## Topological Sort(위상 정렬) 구현

### 1) Indegree 를 이용한 구현

Indegree 는 정점으로 들어오는 간선의 수를 말한다. Indegree 가 0인 정점은 우선해야하는 정점이 더이상 존재하지 않으므로 해당 노드를 위상 정렬할 수 있는 특징을 이용한다. 알고리즘 순서는 다음과 같다.

> 1. $$V$$ 개의 정점으로 이루어진 그래프의 Indegree 를 계산한다.
> 2. Indegree 가 0인 정점을 Queue에 넣는다.
> 3. 총 $$V$$ 회 아래의 루프를 수행한다.
>    1. Queue 에서 정점 하나를 뽑는다.
>    2. 해당 정점에서 나가는 간선을 제거한다. (Indegree 업데이트)
>    3. 업데이트 된 Indegree 가 0인 정점을 Queue에 추가한다.



만약 3에서 $$V$$ 회 수행 전에 Queue 가 Empty라면, DAG가 아님을 의미한다.



#### C++ 코드

```c++
vector<vector<int>> adj_list;
vector<int> indegree;
queue<int> q;
vector<int> result;

void addEdge(int u, int v) {
	adj_list[u].push_back(v);
	indegree[v]++;
}

bool topologicalSort(int V) {
	for (int v = 1; v <= V; v++) {
		if (indegree[v] == 0)
			q.push(v);
	}

	for (int i = 0; i < V; i++) {
		if (q.empty()) {
			printf("has Cycle\n");
			return false;
		}
		int cur = q.front();
		q.pop();
		result.push_back(cur);
		for (auto adj : adj_list[cur]) {
			indegree[adj]--;
			if (indegree[adj] == 0) {
				q.push(adj);
			}
		}		
	}
	return true;
}
```



#### Python 코드

```python
adj_list = [[] for i in range(V+1)]
indegree = [0 for i in range(V+1)]
queue = []
result = []

def addEdge(u, v):
    adj_list[u].append(v)
    indegree[v]+=1

def topologicalSort():
    for v in range(1, V+1):
        if indegree[v] == 0:
            queue.append(v)

    for i in range(V):
        if not queue:
            print("has Cycle")
            return False

        cur = queue.pop(0)
        result.append(cur)
        for adj in adj_list[cur]:
            indegree[adj] -= 1
            if indegree[adj] == 0:
                queue.append(adj)

    return True
```



### 2) DFS 를 이용한 구현

**DFS로 탐색이 종료되는 순서의 역순서가 위상정렬의 결과**가 된다. 예제를 DFS 탐색(작은 번호부터 탐색)하면 $$[5,3,6,4,1,2]$$ 순서로 탐색이 종료되고, 이 역순인 $$[2,1,4,6,3,5]$$ 가 위상정렬의 결과가 된다. 

단순하게, DFS 탐색시 선행되는 정점을 먼저 수행하므로 위상 정렬이 가능한 것처럼 보인다. 하지만, 여러 선행 정점을 갖고 있는 정점의 경우에 문제가 생긴다. 예를 들어, $$ V= \{1,2,3\}$$ 이고, $$E =\{(1,3), (2,3) \}$$ 인 그래프를 보자. 탐색 순서는 $$[1,2,3]$$  또는 $$ [2,1,3]$$ 이나 DFS의 결과는 $$[1,3,2]$$ 또는 $$[2,3,1]$$ 이다. 



#### C++ 코드

```c++
vector<vector<int>> adj_list;
vector<bool> visited;
vector<bool> finished;
stack<int> topological_sort;
bool hasCycle;

void addEdge(int u, int v) {
	adj_list[u].push_back(v);
}

void DFS(int v) {
	visited[v] = true;
	for (auto next : adj_list[v]) {
		if (!visited[next])
			DFS(next);
		else if (!finished[next])
			hasCycle = true;
	}
	finished[v] = true;
	topological_sort.push(v);
	
}
```



#### Python 코드

```python
adj_list = [[] for v in range(V+1)]
visited = [False for v in range(V+1)]
finished = [False for v in range(V+1)]
topological_sort =[]
hasCycle = False

def DFS(v):
    visited[v] = True
    for next in adj_list[v]:
        if not visited[next]:
            DFS(next)
        elif not finished[next]:
            hasCycle = True
    finished[v] = True
    topological_sort.append(v)
```

