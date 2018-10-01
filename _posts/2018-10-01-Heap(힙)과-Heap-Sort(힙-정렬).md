---
layout: post
title: "Heap(힙)과 Heap Sort(힙 정렬)"
slug: "heap-and-heap-sort"
date: 2018-10-01 21:00:00 +0900
categories: blog/data_structure
---



# Heap(힙)과 Heap Sort(힙 정렬)

## Heap 이란?

- **최댓값 및 최솟값**을 찾아내는 연산을 빠르게 하기 위해 고안된 자료구조
- 부모 노드와 자식 노드 사이에 대소 관계가 성립
- **Max heap**(최대힙): 부모 노드가 자식 노드 보다 항상 큰 힙(root: 최댓값)
- **Min heap**(최소 힙): 부모 노드가 자식 노드 보다 항상 작은 힙(root: 최솟값)
- 보통 완전이진트리(Complete Binary Tree) 를 구조를 사용한다.

아래 그림은 Max-heap의 예시이다. 부모 노드가 항상 자식 노드보다 크다.

![Max heap](https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Max-Heap.svg/330px-Max-Heap.svg.png)



## Min Heap(최소 힙)의 구현

이진 트리는 배열로 만들 수 있으며 배열의 $$i$$ 번째 노드 부모는 $$i/2$$ , 자식은 $$2i$$ 와 $$2i+1$$ 이다. 이 이진트리에 부모/자식간 대소 관계가 성립하도록 하면 Heap이 된다. 여기서는 최소 힙을 예로 들어 구현하였다.

### Insertion

1. 가장 마지막 위치에 새로운 노드를 삽입한다.
2. 새 노드가 부모와의 대소 관계가 성립할 때 까지 둘의 위치를 바꾼다.



아래 예를 살펴보자.



1. 가장 마지막 위치에 새 노드 4 를 삽입한다.

2. 부모 노드 9와의 대소 관계를 살펴본다. 최소 힙 조건을 만족하지 않으므로 둘의 위치를 바꾼다.

3. 부모 노드 6과의 대소 관계를 살펴본다. 최소 힙 조건을 만족하지 않으므로 둘의 위치를 바꾼다.

4. 부모 노드 1과의 대소 관계를 살펴본다. 최소 힙 조건을 만족하므로 종료한다.



### Insertion 시간 복잡도

새 노드를 삽입하는데는 상수 시간이 걸리고, 트리를 재구성에 (부모와의 대소 관계를 확인) 최대 이진트리의 높이 만큼 $$O(\log{N})$$ 걸린다.



### Deletion

1. 루트 노드에 마지막 노드를 가져온다. 이전 루트 노드는 리턴 값이 된다.
2. 가져온 노드가 자식과의 대소 관계가 성립할 때 까지 교환한다.



아래의 예를 살펴보자.



1. 루트 노드 1은 리턴 값이 된다. 루트 노드에 마지막 노드 9를 가져온다.

2. 가져온 노드 9와 자식 4,자식 3 과 대소 관계를 살펴본다. 최소 힙 조건을 만족하지 않으므로 최소 힙을 만족할 수 있는 가장 작은 값 3과 바꾼다.

3. 가져온 노드 9와 자식 9, 자식 7과 대소 관계를 살펴본다. 최소 힙 조건을 만족하는 가장 작은 값 7과  바꾼다.

4. 자식이 더 이상 존재하지 않으므로 종료한다.



### Deletion의 시간 복잡도

루트 노드를 삭제하는데 상수 시간이 걸리고, 트리 재구성에 (마지막에서 가져온 노드와 자식간의 대소 관계 확인) 최대 트리의 높이 만큼 $$O(\log{N})$$ 걸린다.



## Heap과 Heap Sort

값을 오름차순으로 정렬하고 싶으면 Min heap(최소 힙) 을 구성하여 하나씩 빼내면 되고, 내림차순의 경우는 Max heap(최대 힙)을 구성하여 하나씩 뺴내면 된다. 따라서, Heap Sort의 시간 복잡도는 힙 구성에 $$\log{N} + \log{N-1} + ... + \log2 = N\log{N}$$ 의 시간복잡도를, 힙 삭제에 $$\log{N} + \log{N-1} + ... + \log2 = N\log{N}$$ 의 시간 복잡도를 가지므로 $$O(N\log{N})$$ 이다.



## 구현 코드

아래는 Min heap(최소 힙) 을 구현한 코드이다. **특징으로 배열의 index가 1이 아닌 0이 루트 노드이다.**

### C++ 코드

```c++
class Heap {
	vector<int> heap;
	
public:
	void insert(int data) {
		heap.push_back(data);
		int idx = heap.size() - 1;
		while (idx != 0 && heap[idx / 2 - ((idx % 2)^1)] > data) {
			heap[idx] = heap[idx / 2 - ((idx % 2) ^ 1)];
			idx = idx / 2 - ((idx % 2) ^ 1);
		}
		heap[idx] = data;
	}

	int pop() {
		int item = heap[0];
		heap[0] = heap[heap.size() - 1];
		heap.pop_back();
		int idx = 0;

		while (true) {
			int left = idx * 2 + 1;
			int right = idx * 2 + 2;
			int min_idx = idx;

			if (left < heap.size() && heap[left] < heap[min_idx]) {
				min_idx = left;
			}
			if (right < heap.size() && heap[right] < heap[min_idx]) {
				min_idx = right;
			}

			if (min_idx == idx) {
				break;
			}
			int temp = heap[idx];
			heap[idx] = heap[min_idx];
			heap[min_idx] = temp;
			idx = min_idx;
		}	
		return item;
	}

	int size() {
		return heap.size();
	}
};
```



#### Python 코드

```python
class Heap:
    def __init__(self):
        self.heap = []

    def insert(self, data):
        self.heap.append(data)
        idx = len(self.heap) - 1

        while idx != 0 and self.heap[int(idx/2) - ((idx%2)^1)] > data:
            self.heap[idx] = self.heap[int(idx/2) - ((idx%2)^1)]
            idx = int(idx/2) - ((idx%2)^1)

        self.heap[idx] = data

    def pop(self):
        item = self.heap[0]
        idx = 0
        self.heap[0] = self.heap[len(self.heap) - 1]
        self.heap.pop()

        while True:
            left = idx * 2 + 1
            right = idx * 2 + 2
            min_idx = idx

            if left < len(self.heap) and self.heap[left] < self.heap[min_idx]:
                min_idx = left

            if right < len(self.heap) and self.heap[right] < self.heap[min_idx]:
                min_idx = right

            if min_idx == idx:
                break

            temp = self.heap[idx]
            self.heap[idx] = self.heap[min_idx]
            self.heap[min_idx] = temp
            idx = min_idx

        return item
```

