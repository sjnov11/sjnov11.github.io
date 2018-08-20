---
layout: post
title: "Union-Find 자료구조와 Undirected Graph의 Cycle Detection"
slug: "union_find"
date: 2018-08-20 22:56:00 +0900
categories: blog/algorithm
---



# Union-Find 자료구조와 Undirected Graph의 Cycle Detection



## Union-Find 이란?

Union-Find는 서로소 집합(disjoint set)을 관리하는 자료구조이다. Union-Find는  이름처럼 다음 두 가지 연산을 가지고 있다.

1) **Find(A)**: 요소(element) A가 속한 집합을 반환한다.
2) **Union(A, B)**: 요소 A가 속한 집합과 요소 B가 속한 집합을 합친다.

다음의 예제에서 Find(1) == Find(2) 이고, Union(1, 3) 또는 Union(2, 3) 의 결과는 아래와 같다.

![union-find_1.jpg](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/08/20/union-find_1.jpg?raw=true)



## Union-Find 구현

배열을 이용한 구현 방법과 트리를 이용한 구현 방법이 있다. 배열로 구현한 경우 Find를 $$O(1)$$ 시간으로 처리할 수 있지만, Union의 경우는 $$O(N)$$ 의 시간이 걸린다. 반면, 트리로 구현한 경우 Find와 Union 모두 $$O(logN)$$ 시간이 걸린다.



### 1) 배열을 이용한 구현 방법

#### (1) 초기화

Union-Find의 초기 상태는 각 요소들이 모두 서로소 집합인 상태이다. 아래와 같이 요소들이 속하는 집합을 자기 자신으로 두면 된다.

```c++
//code by RiKang, weeklyps.com
struct union_find{
    vector<int> set_num;  
    void init(int n){ 
        set_num.resize(n + 1);
        for(int i = 1; i <= n; i++)
            set_num[i] = i;
    }
}uf;
```

#### (2) Find

Find는 set_num 배열의 index를 참조하여 구할 수 있다. ($$O(1)$$)

```c++
int find(int elem1) {
    return set_num[elem1];
}
```

#### (3) Union

Union은(elem1을 기준) elem2가 속한 집합의 모든 원소를 elem1의 집합으로 옮기면 된다. ($$O(N)$$)

```c++
void union (int elem1, int elem2) {
    int remove_set = find(elem2);
    for (int i = 1; i < set_num.size(); i++) {
        if (set_num[i] == remove_set)
        	set_num[i] = find(elem1);
    }
}
```



### 2) 트리를 이용한 구현

배열과는 달리 이 방법에서는 집합의 번호를 루트의 번호로 대신한다. 각 요소들의 부모를 저장해두면 루트까지 타고 올라갈 수 있으므로, 배열에서 요소들이 집합 번호를 담고 있던 것(set_num) 과 달리 요소들이 부모 요소를 찾아갈 수 있도록 parent 배열을 만든다. 이 parent 가 결국 트리의 adj_list를 만드는 것과 같다.

![union-find_2.jpg](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/08/20/union-find_2.jpg?raw=true)

#### 

#### (1) 초기화

배열을 이용한 구현과 같다. 초기 상태에서 부모가 없으므로 자기 자신을 부모로 둔다. (혹은 -1을 두어도 좋다)

```c++
//code by RiKang, weeklyps.com
struct union_find{
    vector<int> parent;  
    void init(int n){ 
        parent.resize(n + 1);
        for(int i = 1; i <= n; i++)
            parent[i] = i;
    }
}uf;
```

#### (2) Find

parent가 자기자신(혹은 -1)이 나올 때 까지 부모를 계속 탐색한다. ($$O(\log{N})$$)

```c++
int find(int elem1) {
    if (parent[elem1] == elem1) return elem1;
    return find(parent[elem1]);
}
```

#### (3) Union

Union은(elem1을 기준) elem2의 루트를 elem1의 루트의 자식으로 달아주면 된다. ($$O(\log{N})$$) 

```c++
void union (int elem1, int elem2) {
    int r1 = find(elem1);
    int r2 = find(elem2);
    parent[r2] = r1;
}
```

Union시 최악의 경우 LinkedList 형태처럼 줄줄이 달릴 수가 있다. 두 집합 중 높이가 낮은 트리를 높은 트리 아래에 넣어두면 최악의 경우를 피할 수 있다. 

#### 

## Undirected Graph의 Cycle Detection

아무 node도 연결되지 않은 상태를 초기 상태로 두자. 각 node들은 자기 자신으로만 이루어져 있는 그래프가 될 것이다. 각각의 독립된 그래프를 disjoint set으로 보자. edge로 연결하게 되면 두 독립된 그래프가 하나의 그래프가 된다. 즉, 두 집합이 하나의 집합으로 Union 된다. edge로 연결할 때 두 node가 같은 그래프 집합에 있다면, Cycle이 발생하는 것이다. 

```c++
bool isCycle (Graph graph) {
	for (int edge : edge_list) {
		// edge : (u, v)
		int n1 = edge.first;
		int n2 = edge.second;
    	if (find(n1) == find(n2)) return true;
    	union(n1, n2);
	}
	return false;
}
```

