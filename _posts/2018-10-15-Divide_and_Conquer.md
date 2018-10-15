---
layout: post
title: "Divide and Conquer"
slug: "divide_and_conquer"
date: 2018-10-15 20:30:00 +0900
categories: blog/algorithm
---

# Divide and Conquer

## 1. Binary Search

```c++
int binarySearch(vector<int> nums, int target) {
	int l = 0;
	int r = nums.size() - 1;

	while (l <= r) {
		int m = (l + r) / 2;
		if (nums[m] == target) return m;
		else if (nums[m] < target) {
			l = m + 1;
		}
		else {
			r = m - 1;
		}
	}
	return -1;
}
```

**Binary Search** 는 탐색의 범위를 절반으로 줄여가면서 target을 찾는다. 탐색의 종료 조건을 잘 확인하자. l 이 r과 같은 경우는 탐색하는 범위가 1인 경우이고, 이 경우도 마찬가지로 확인해 주어야한다.



## 2. Merge Sort

```c++
// Conquer 과정
void merge(vector<int>& nums, int l, int m, int r) {
	int idx = 0;
	int lcur = l, lend = m;
	int rcur = m + 1, rend = r;
	int size = r - l + 1;

	vector<int> temp(size);
	while (lcur <= lend && rcur <= rend) {
		if (nums[lcur] < nums[rcur]) temp[idx++] = nums[lcur++];
		else temp[idx++] = nums[rcur++];
	}
	while (lcur <= lend) {
		temp[idx++] = nums[lcur++];
	}
	while (rcur <= rend) {
		temp[idx++] = nums[rcur++];
	}

	for (int i = 0; i < size; i++) {
		nums[l + i] = temp[i];
	}
}

// Divide 과정
void mergeSort(vector<int>& nums, int l, int r) {
	if (l >= r) return;
	int m = (l + r) / 2;
	mergeSort(nums, l, m);
	mergeSort(nums, m + 1, r);
	merge(nums, l, m, r);
}
```

**Merge Sort** 는 sorting 할 범위를 최대한 작게 만든 다음, 범위를 차례로 늘여가면서 sorting 하는 방법이다. 범위를 줄여나갈 때, 현재 배열의 크기가 1보다 작으면(l $$\geq$$ r) 더이상 나눌 수 없으므로 종료한다. 핵심은 *merge* 함수인데, 파라미터 l, m, r로 merge 하려고 하는 두 개의 sorting된 array를 가져와서 병합한다. 



## 3. Quick Sort

```c++
int partition(vector<int> &nums, int left, int right) {
	// 시작점은 피벗을 어떤것으로 잡느냐에 따라 다르다.
	int i = left, j = right;
	int pivot = nums[i++];

	while (true) {		
		// 좌측에서는 피벗보다 큰 것을 고른다
		while (i <= right && nums[i] <= pivot) {
			i++;
		}
		// 우측에서는 피벗보다 작은 것을 고른다
		while (j >= left && nums[j] > pivot) {
			j--;
		}
		if (i >= j) break;
		swap(&nums[i], &nums[j]);
	} 
	swap(&nums[left], &nums[j]);
	return j;
}

void quickSort(vector<int> &nums, int l, int r) {
	if (l < r) {
		int m = partition(nums, l, r);
		quickSort(nums, l, m - 1);
		quickSort(nums, m + 1, r);
	}
}
```

**QuickSort** 는 피벗을 하나 잡고 피벗의 좌측에는 피벗보다 작은 수를, 피벗의 우측에는 피벗보다 큰 수를 두는 *partion* 을 반복한다. *partion* 을 수행하면, 피벗의 위치는 정렬된 위치에 놓이므로 이를 제외하고 좌측, 우측에 대해서 quickSort를 재귀적으로 호출해주면 된다. 