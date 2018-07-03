---
layout: post
title: "세그먼트 트리(Segment Tree)"
slug: "segment-tree"
date: 2018-07-03 16:59:00 +0900
categories: blog/Algorithm
---

# Segment Tree

 Segment tree란 **순차적으로 저장된 data(배열)의 연속된 구간에 대한 답(정보)을 빠르게 구할 수 있는 자료구조**이다. 핵심적인 아이디어는 순차적인 data들을 반으로 나누어 구간을 설정한다. 설정된 구간에는 구간에 대한 정보를 담고 있으며, 구간이 사라질 때 까지 이를 recursive 하게 반복한다. data 구간을 계속해서 반으로 나누어 나가기 때문에, 이진트리의 형태의 자료구조를 떠올릴 수 있겠다. 즉, 구간에 대한 해답을 찾는 것은 이진트리 탐색이며 $$O(\log N)$$ 의 탐색시간이 소요된다.



![segment_tree](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/07/03/segment_tree.JPG?raw=true)



### 단순 기존 방식과 Segment tree의 비교

 그렇다면 왜 Segment tree가 기존의 구간 탐색보다 더 효율적인 것일까? 

 만약 구간 $$[i,j]$$ 에 대해서 탐색을 한다고 하자. 기존의 배열에선 index $$i$$ 에서 부터 $$j$$ 까지 순차적으로 탐색을 하면서 구간에 대한 정보를 업데이트 해 나가야 한다. 이 경우 탐색의 최대의 경우는 모든 원소 데이터를 찾아야하므로 $$O(N)$$ 의 시간이 소요된다. 

 다른 방법으로 기존의 배열을 사용하는 대신 구간에 대한 정보를 담는 배열을 만드는 것이다. 이 경우는 구간에 대한 답(정보)을 미리 담아두었기 때문에 빠르게 답을 구할 수 있다. 그러나 기존 data를 업데이트 하는 경우, 구간에 대한 정보도 업데이트를 해야하는 overhead가 존재한다. 



 한 예를 들어 보겠다. $$int$$ 타입의 배열 $$A[k]$$ 가 있고 $$[i,j]$$ 구간의 합을 구하고 싶다. 위 두 가지 방법으로 연속적인 구간의 합을 구하여보자. 

 첫번째 방법으로는 각 배열의 $$i$$부터 $$j$$ index를 순차적으로 돌면서 그 값을 더해주는 것이다. 

 두번째 방법은 배열 정보를 담을 때, 구간의 합에 대한 정보 또한 동시에 담아둔다. $$S[k]$$ 를 $$[0,k]$$ 구간까지 합이라고 한다면 $$S[k] = S[k-1] + A[k]$$ 가 되며, 구간 $$[i,j]$$ 의 합은 $$S[j] -S[i-1]$$ 이 된다. 만약, 배열의 정보가 바뀌게 된다면 $$S$$의 정보도 바꾸어 주어야 하는데, 첫번째 $$A[0]$$가 바뀌게 된다면 모든 $$k$$ 에 대하여 $$S[k]$$ 를 업데이트 해야하므로 $$O(N)$$ 의 시간이 소요된다.



 그렇다면 Segment tree의 경우는 어떻게 될까? 다음 segment tree를 살펴보자.

 먼저 Segment tree에서 구간의 정보를 찾는 경우를 살펴보자. 구간 $$[0,2]$$ 에 대한 답을 찾으려면 다음과 같이 탐색하면 된다.



![segment_tree5.JPG](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/07/03/segment_tree2.JPG?raw=true) 



 마찬가지로 $$[0,3]$$ 에 대한 정보를 찾고 싶다면 다음과 같이 탐색하면 된다.

![segment_tree3.JPG](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/07/03/segment_tree3.JPG?raw=true) 

 찾고자 하는 구간에 대한 정보가 모두 찾아질 때까지 tree를 탐색하면 되므로 $$O(\log N)$$ 의 시간이 소요된다.



 만약 기존 data 가 업데이트 되어 구간에 대한 정보(답)가 바뀌었을 때는 어떻게 될까? 마찬가지로 tree를 탐색하면서 구간에 대한 정보를 업데이트 해주면 된다. 다음 예는 $$A[3]$$ 이 업데이트 되었을 때 segment tree가 업데이트 되는 모습이다.



![segment_tree4.JPG](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/07/03/segment_tree4.JPG?raw=true) 



 마찬가지로 tree 탐색이므로 $$O(\log N)$$ 의 시간이 소요된다.



### 결론

 Segment tree는 **순차적으로 저장된 data(배열)의 연속된 구간에 대한 답(정보)을 빠르게 구하고 업데이트 할 수 있는 자료구조**이다. *1)반복적으로 주어진 구간에 대해 하나씩 탐색하는 경우* 와 비교하면 구간에 대한 답을 구하는 시간이 각각 $$O(N)$$ 과 $$O(\log N)$$ 으로 segment tree의 경우가 더 빠르며, *2)구간의 답을 미리 구해놓은 경우* 와 비교 했을 때 구간의 정보를 구하는 시간은 같았지만, 기존 data가 잦은 변경이 일어날 경우 segment tree가 더 빠른 시간내에 갱신함을 알 수 있다.

*"구간의 정보를 구할 때, 미리 구간에 대한 정보를 담아두면 빠르게 답을 구할 수 있다. 만약 data가 자주 변하는 상황이라면 segment tree 를 생각해 보자."*