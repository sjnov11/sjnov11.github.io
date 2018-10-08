---
layout: post
title: "플로이드-워셜 알고리즘(Floyd-Warshall Algorithm)"
slug: "floyd-warshall"
date: 2018-08-24 15:36:00 +0900
update: 2018-10-02 16:32:00 +0900
categories: blog/algorithm
---

# Floyd-Warsahll Algorithm

플로이드-워셜 알고리즘은 그래프에서 최단 경로를 구하는 알고리즘 중 하나이다.
다익스트라와 벨만-포드 알고리즘은 한 정점에서 다른 모든 정점 사이의 최단 경로를 구하는 것과 달리, 플로이드-워셜 알고리즘은 모든 정점 사이의 최단 경로를 구한다.



## Dynamic Programming

플로이드-워셜 알고리즘은 최단경로를 Dynamic Programming 형태의 문제로 정의한다. [위키백과](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%9B%8C%EC%85%9C_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98) 를 참조 하였다.

1에서 $$N$$ 까지 번호가 매겨진 정점들을 갖는 그래프가 있다고 하자.  $$i$$ 에서 $$j$$ 로 집합 $$\{1,2,...,k\}$$  의 정점들만을 경유지로 거치는 최단경로를 반환하는 함수 $$shortestPath(i,j,k)$$ 를 생각한다. 

각각의 정점 쌍에 대해서,  $$shortestPath(i,j,k)$$ 다음 중 최단거리 이다.

(1) $$k$$ 를 **통과하지 않는** 경로 ($$\{1,..,k-1\}$$ 에 있는 정점만 거칠 수 있다)

(2) $$k$$ 를 **통과 하는** 경로 ($$i$$ 에서 $$k$$ 로 가는 경로와 $$k$$ 에서 $$j$$ 로 가는 경로 모두 $$\{1,...,k-1\}$$ 에 있는 정점만 거칠 수 있다)

(1)에서 $$i$$ 에서 $$j$$ 까지 $$\{1,...,k-1\}$$ 의 정점만을 거쳐가는 경로 중 최선의 경로는 $$shortestPath(i,j,k-1)$$ 이다. 

(2)의 경우는 $$i$$ 에서 $$k$$ 까지 $$\{1,...,k-1\}$$ 정점만을 거쳐가는 최선의 경로와 $$k$$ 에서 $$j$$ 까지 $$\{1,...,k-1\}$$ 정점만을 거쳐서 가는 최선의 경로의 합 $$shortesPath(i,k,k-1) +shortestPath(k,j,k-1)$$ 이다. 

$$w(i,j)$$ 를 정점 $$i$$ 와 $$j$$ 간선의 가중치라고하면 $$shortestPath(i,j,k)$$ 는 다음과 같은 재귀식으로 나타낼 수 있다.
$$
shortestPath(i,j,0) = w(i,j)
$$

$$
shortestPath(i,j,k) =\\ \min \begin{pmatrix}(shortestPath(i,j,k-1),\\shortestPath(i,k,k-1) + shortestPath(k,j,k-1)) \end{pmatrix} \\
$$



## 직관적 해석

```c++
// dp배열은 그래프를 나타내는 인접행렬
for (int k = 1; k <= V; k++) {
    for (int i = 1; i <= V; i++) {
        for (int j = 1; j <= V; j++) {
            if (dp[i][j] > dp[i][k] + dp[k][j])
                dp[i][j] = dp[i][k] + dp[k][j];
        }
    }
}
```

코드에서 $$k$$ 는 거쳐가는 정점을, $$i$$ 와 $$j$$ 는 시작과 끝 정점을 의미한다.

$$i$$ 와 $$j$$ 의 반복문은 모든 정점 사이(간선)에 대해서 테스트 함을 말한다. 초기에는 간선들의 가중치가 들어있다.

$$k$$ 반복문에서 기존까지의 ($$\{1,...,k-1\}$$ 거치는 경우를 포함하여) 모든 정점 사이의 거리보다 $$k$$ 를 거쳐가는 게 더 짧은지를 확인한다. 



다음의 그래프를 살펴보자.



![floyd-warshall.png](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/08/24/floyd-warshall.png?raw=true)



우리는 거쳐갈 정점에 대해서 모두 탐색을 해야한다. 그래프의 초기 거리는 다음과 같다.


$$
A^0=\begin{bmatrix} 0 & 3 & \infty & 7 
				\\ 8 & 0 & 2 &\infty
				\\ 5 & \infty & 0 & 1
				\\ 2 & \infty & \infty & 0\end{bmatrix}
$$


$$ A^0$$ 는 그래프를 인접행렬로 나타낸 것과 같으며, 한 정점에서 중간에 다른 정점을 거치지 않고 바로 다른 정점을 가는 길이이다.

이번엔 정점 1을 거치는 경우를 추가해서 나타내보자. 정점 1을 거치는 경우가 안 거치는 경우보다 짧다면 update 시켜준다. 결과, 다음과 같은 인접행렬이 나온다.


$$
A^1 = 	\begin{bmatrix} 
		0 & 3 & \infty & 7\\
		8 & 0 & 2 & 15\\
		5 & 8 & 0 & 1\\
		2 & 8 & \infty & 0
		\end{bmatrix}
$$


정점 2를 거치는 경우를 추가해서 나타내면 다음과 같다. 이 행렬은 정점 1 또는 정점 2를 거치는 경우와 거치지 않고 바로 가는 경우 모두를 포함한다.


$$
A^2 = 	\begin{bmatrix} 
		0 & 3 & 5 & 7\\
		8 & 0 & 2 & 15\\
		5 & 8 & 0 & 1\\
		2 & 5 & 7 & 0
		\end{bmatrix}
$$

계속해서 정점 3을 거치는 경우, 정점 4를 거치는 경우를 추가해서 나타내면 다음과 같다.


$$
A^3 = 	\begin{bmatrix} 
		0 & 3 & 5 & 6\\
		7 & 0 & 2 & 3\\
		5 & 8 & 0 & 1\\
		2 & 5 & 7 & 0
		\end{bmatrix}
\ \ \ \ 
A^4 = 	\begin{bmatrix} 
		0 & 3 & 5 & 6\\
		5 & 0 & 2 & 3\\
		3 & 6 & 0 & 1\\
		2 & 5 & 7 & 0
		\end{bmatrix}
$$




모든 정점을 거치는 경우를 계산한 $$A^4$$ 가 우리가 구하고자 하는 정답이 된다.

위 과정을 다시 살펴보자. $$A^k$$ 는 1부터 $$k$$ 까지의 정점을 거칠 수 있을 때의 최단 거리를 나타낸다. 이는 $$A^{k-1}$$ 인 1부터 $$k-1$$ 까지 정점을 거칠 수 있을 때의 최단 거리를 이용해서 구할 수 있다. 식으로 나타내면 다음과 같다.


$$
A^k[i][j] = \min(A^{k-1}[i][j], A^{k-1}[i][k] + A^{k-1}[k][j])
$$


이전의 sub problem의 optimal solution 을 통해 최종 optimal solution을 구하므로 dynamic programming의 형태이다. 위 문제에서 $$shortestPath(i, j, k) $$ 는 3개의 파라미터를 가지고 있지만, $$k$$ 는 직전의 $$k-1$$ 만을 사용하므로 2차원 배열로 충분히 해결할 수 있다.



## Pseudo-code

```c++
// dp배열은 그래프를 나타내는 인접행렬
for (int k = 1; k <= V; k++) {
    for (int i = 1; i <= V; i++) {
        for (int j = 1; j <= V; j++) {
            if (dp[i][j] > dp[i][k] + dp[k][j])
                dp[i][j] = dp[i][k] + dp[k][j];
        }
    }
}
```

