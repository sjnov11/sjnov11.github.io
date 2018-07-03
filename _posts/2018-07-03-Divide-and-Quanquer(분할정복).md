---
layout: post
title: "Divide and Quanquer(분할정복)"
slug: "divide-quanquer"
date: 2018-07-03 21:59:00 +0900
categories: blog/Algorithm
---

# Divide and Quanquer

[6549번 히스토그램에서 가장 큰 직사각형]:https://www.acmicpc.net/problem/6549
[6549번 해설]:https://www.acmicpc.net/blog/view/12



![img](https://www.acmicpc.net/upload/images/histogram.png) 



### 아이디어

 우리에겐 각 직사각형의 높이가 주어져 있다. 직사각형의 넓이는 밑변*높이 이므로, 각 높이($$h_i$$) 에 대해서 가능한 최대 밑변$$(w_i)$$ 을 구한다면 현재 높이에 대한 최대 넓이를 구할 수 있다. 모든 직사각형 높이$$(\forall h_i)$$에 대해서 이를 수행하면 가능한 가장 큰 직사각형을 구할 수 있을 것이다. 



### 처음 든 생각

 각각의 직사각형에 대해서 만들 수 있는 최대 크기를 구한다. $$i$$ 번째 직사각형에 대해서, $$i$$ 의 좌우로 최대로 넓힐 수 있는 가로의 길이를 ($$w_i$$) 구한다. 예제에서 $$i=6$$ 일 때, $$w_i=2$$ 이고 따라서 이 경우의 넓이는 $$h_i*w_i = 6$$ 이 된다. 

 이 경우 직사각형의 갯수를 $$N$$ 이라 했을 때, 각 직사각형에서 다른 직사각형 모두와 비교를 해야하므로 $$O(N^2)$$ 의 시간복잡도를 갖는다. $$N=10^5$$ 이고, $$10^8$$ 이상은 제한시간인 1초를 초과할 것이므로 다른 방법을 알아보아야 한다.





### 분할 정복과 세그먼트 트리

#### 어떻게 풀 것 인가? 

 가장 높이가 작은 직사각형을 구한다. 그 $$k$$ 번째 직사각형과 높이를 각각 $$r_k$$ 와 $$h_k$$라고 하자. 처음의 경우, $$w_k = N$$ 이므로 최대 넓이는 $$h_k * N$$ 이 될 것이다. 그렇다면 다음으로 높이의 경우는 어떻게 할 것인가? 이 때 분할 정복 기법을 사용을 하는 것이다. 

 높이가 $$h_k$$ 인 경우에 대해서 최대 넓이를 구하였기 때문에 다음으로 낮은 높이에 대해서 최대 넓이를 구해본다. $$r_k$$ 를 제외하고 다음으로 작은 높이에 대해서 구한다. 이 때, $$r_k$$의 좌측 부분과 우측 부분은 이어질 수 없으므로 각각 sub problem 으로 분할이 된다. 마지막 높이에 대해서 넓이를 구할 때 까지 이를 계속 반복하면 모든 높이에 대해서 최대 넓이를 갖는 직사각형을 구하였음으로 이 중에서 가장 큰 넓이가 답이 된다.



#### 시간복잡도?

 수행시간을 계산해보자. 각 상태에서 높이가 최소인 직사각형을 구하는데 $$O(N)$$ 이 들며 (항상 제외한 나머지에 대해서 모두 탐색해야 한다), 각 상태에서 최소 높이의 직사각형을 하나씩 제거하므로, 총 직사각형의 갯수 $$N$$ 번 만큼 수행한다. 따라서, 이 경우의 시간복잡도는 $$O(N^2)$$ 이 된다. 



#### 다를게 없잖아?

 여기서 높이가 최소인 직사각형을 구하는 부분을 살펴보자. 연속적으로 이어진 직사각형에서 높이가 최소인 직사각형을 구하여야 한다. 순차적인 data 배열에서 구간의 답을 구할 때 효율적으로 사용되는 자료구조가 **segment tree**였다. **Segment tree** 를 활용하면 최소값을 $$O(\log N)$$ 으로 줄일 수 있으며, 결과적으로 최종 수행 시간을 $$O(N\log N)$$ 으로 줄일 수 있다.



```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <math.h>
#include <limits.h>
using namespace std;


int buildSegTree(vector<int> &arr, vector<int> &tree, int node, int start, int end) {
	if (start == end) {
		tree[node] = start;
		return start;
	}

	int left = buildSegTree(arr, tree, node * 2, start, (start + end) / 2);
	int right = buildSegTree(arr, tree, node * 2 + 1, (start + end) / 2 + 1, end);
	
	if (arr[left] > arr[right])
		return tree[node] = right;
	else
		return tree[node] = left;
}

int getMinHeightIdx(vector<int> &arr, vector<int> &tree, int node, int start, int end, int left, int right) {
	if (start > right || end < left)
		return 0;
	if (start >= left && end <= right)
		return tree[node];

	int left_idx = getMinHeightIdx(arr, tree, node * 2, start, (start + end) / 2, left, right);
	int right_idx = getMinHeightIdx(arr, tree, node * 2 + 1, (start + end) / 2 + 1, end, left, right);

	if (arr[left_idx] > arr[right_idx]) {
		return right_idx;
	}
	else {
		return left_idx;
	}
}

long long answer(vector<int> &arr, vector<int> &tree, int N, int left, int right) {
	int idx = getMinHeightIdx(arr, tree, 1, 1, N, left, right);
	long long result;
	result = (long long)arr[idx] * (long long)(right - left + 1);
	if (idx - 1 >= left)
		result = max(result, answer(arr, tree, N, left, idx - 1));
	if (idx + 1 <= right)
		result = max(result, answer(arr, tree, N, idx + 1, right));
	
	return result;	
}

int main() {
	ios::sync_with_stdio(false);
	//freopen("input3.txt", "r", stdin);
	while (true) {
		int N;
		cin >> N;
		if (N == 0)
			break;
		vector<int> arr(N + 1);
		arr[0] = INT_MAX;
		for (int i = 1; i <= N; i++) {
			cin >> arr[i];
		}

		int h = ceil(log2(N));
		int tree_size = (1 << (h + 1));
		vector<int> tree(tree_size + 1);
		buildSegTree(arr, tree, 1, 1, N);

		cout << answer(arr, tree, N, 1, N) << endl;
	}
	return 0;
}
```





### 스택 (Stack)

#### 어떻게 풀 것인가?

$$i$$ 번째 직사각형을 $$r_i$$, 그 높이를 $$h_i$$ 라고 하자. $$r_i$$ 에서 $$h_i$$ 를 높이로 하면서 가장 큰 직사각형을 찾기 위해서는 $$r_i$$ 의 왼쪽 직사각형 중 $$h_i$$ 보다 작은 높이를 갖는 $$r_{left}$$ 와 $$r_i$$ 의 오른쪽 직사각형 중 $$h_i$$ 보다 작은 높이를 갖는 $$r_{right}$$ 를 찾아야한다. 



 스택에 직사각형을 왼쪽에서 오른쪽으로 하나씩 차례로 넣는다. 만약 현재 스택의 $$top$$ 에 있는 직사각형$$(r_j)$$보다 추가할 직사각형$$(r_k)$$ 의 높이가 더 작다면 더 이상 직사각형을 오른쪽으로 확장시킬 수 없음을 의미한다. 따라서, $$h_{j}$$ 의 높이를 가진 직사각형의 우측 끝의 $$index$$ 는 $$k-1$$ 이고, 좌측 끝의 $$index$$ 는 $$top$$ 바로 아래의 직사각형($$r_i$$) 다음인 $$i+1$$ 이 된다.  

 우측 끝의 $$index$$ 가 $$k-1$$임은 아주 직관적이다. 좌측 끝의 $$index$$ 가 왜 다음 직사각형$$(r_i)$$ 의 다음 $$index$$ 인 $$i+1$$ 이 되는지는 조금  생각해보아야 한다.

  3가지 경우로 나누어 보자.

1. $$h_i < h_j$$  인 경우, $$h_j$$ 높이에 대하여 좌측으로 확장할 수 없음
2. $$h_i = h_j$$ 인 경우, $$h_j$$ 높이에 대하여 좌측으로 확장할 수 있으나, $$h_i$$ 에서 확장한 경우와 중복됨
3. $$h_i > h_j$$ 인 경우, 스택에 넣기 전에 처리됨으로 이 경우는 나올 수 없음

 헷갈리는 부분이 2번이었는데, $$h_i$$ 높이인 경우에서 처리가 됨으로 이 경우는 신경쓰지 않아도 된다.



 스택 수행은 정리하면 다음과 같다. 

1. 스택이 비어 있다면 현재 직사각형 $$index$$ 를 넣는다. 

2. 스택에 값이 있다면 $$top$$ 의 직사각형$$(r_j)$$ 의 높이$$(h_j)$$ 와 넣으려고 하는 직사각형$$(r_k)$$ 의  높이$$(h_k)$$ 를 비교한다. 

   1) $$h_j \leq h_k$$ 이면 스택에  $$k$$ 를 넣는다.

   2) $$h_j \gt h_k$$ 이면 $$h_j$$ 일 때, 최대 넓이를 구한다. 처음부터 다시 수행한다. 

    - **최대 가로길이를 구하는 방법은 다음과 같다.** 

      스택을 pop 하여 $$left$$ 의 $$index$$ 를 구한다. $$right\  index$$ 는 $$j-1$$ 이다. 

3. 모든 직사각형 $$index$$ 를 넣을 때 까지 처음부터 다시 수행한다.

4. 모든 직사각형을 넣었다면 스택에 남아있는 직사각형 $$index$$ 에 대해서 **최대 넓이를 구한다.** 이 때 가로의 우측 끝은 끝까지 갔으므로 $$n-1$$  이다.



#### 시간복잡도?

 이 경우의 수행시간은 stack에 모든 직사각형을 넣고 빼는 것이 전부이므로 $$O(N)$$ 의 시간복잡도를 갖는다.



```c++
#include <iostream>
#include <stack>
#include <vector>
#include <limits.h>
#include <algorithm>
using namespace std;

int main() {
	while (true) {
		int n;
		cin >> n;
		if (n == 0) break;
		vector<int> h(n);
		for (int i = 0; i < n; i++) {
			cin >> h[i];
		}

		stack<int> s;
		long long Answer = 0;
		for (int i = 0; i < n; i++) {
			while (true) {
				if (s.empty())
					s.push(i);
				else {
					int top_idx = s.top();
					if (h[top_idx] > h[i]) {
						int right = i - 1;
						int left;
						s.pop();
						if (s.empty())
							left = 0;
						else
							left = s.top() + 1;
						Answer = max(Answer, (long long)(right - left + 1) * (long long)h[top_idx]);
					}
					else {
						s.push(i);
						break;
					}
				}
			}
		}

		int right = n - 1;
		while (!s.empty()) {
			int height = h[s.top()];
			int left;
			s.pop();
			if (s.empty())
				left = 0;
			else
				left = s.top() + 1;

			Answer = max(Answer, (long long)height * (long long)(right - left + 1));

		}
		cout << Answer << endl;
	}
}
```

