---
layout: post
title: "SW#5215 knap-sack-problem"
slug: "SW#5215"
date: 2018-08-17 20:46:00 +0900
categories: blog/algorithm
---



# SW #5215 햄버거 다이어트

문제를 읽자마자 knap-sack problem이 떠올라 Dynamic programming을 적용하려 하였으나 sub-problem에 대한 개념이 부족하여 찾아보았다 (어떻게 sub-problem을 나누어야 할 지 감을 잡지 못했다).  



### Knapsack Problem

> $$N$$개의 item에 대하여 각각 무게와 가치가 주어졌을 때, 용량 $$W$$의 knapsack에 가치가 최대가 되도록 item을 담는 방법을 구하는 문제.
>
> *참고 : https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/*

Dynamic programming 을 적용하려면 문제의 최적해가 무엇인지 생각해보고,  1) 최적해의 부분해를 구할 수 있는 지, 2) 부분해의 overlapping 을 확인한다.

Knapsack Problem의 최적해는 단순하게 모든 item들의 조합으로 구할 수 있다(각 item이 최적해에 들어가느냐 안 들어가느냐의 경우로 $$N$$개의 item이 있다면 $$O(2^N)$$ 의 경우의 수가 존재). 조합들 중, 무게가 W가 넘지 않으면서 가치가 최대가 되는 경우가 최적해가 된다.



**1) Optimal Substructure**

각 item은 다음 2가지의 경우를 갖는다: 최적해에 들어가느냐, 안들어가느냐.
최적해는 다음 두 경우 중 최대가 되는 경우이다.

1) $$n-1$$ 개의 item들로 $$W$$ 를 채우는 경우 ($$n$$ 번째 item이 안 들어가는 경우)
2) $$n$$ 번째 item이 포함되고, $$n-1$$ 개의 item들로 $$W-w[n]$$  를 채우는 경우 ($$n$$ 번째 item이 들어가는 경우)

따라서, 최적해를 위와 같이 2개의 부분문제로 나눌 수 있다. 
(나는 위에서 item들의 index로 포함되는지 안 포함되는지의 Substructure를 나누는 것을 생각하지 못하였다.  $$n$$ 개의 item에서 남은 item을 $$n-1$$ 개로 줄일 때, $$n$$ 번째 item의 사용 여부를 결정한다면 각 item이 사용되는지 안 되는지 나눌 수 있다. )



**2) Overlapping Subproblems**

1)에서 구한 Substructure 가 반복되는지 확인하면 된다. 예를 들어서, $$n$$ 번째 item 의 무게를 10, $$n-1$$ 번째 item의 무게를 20, $$n-2$$ 번째 item의 무게를 10이라고 하자. 다음 경우에서 같은 Subproblem이 생김을 확인할 수 있다.

1) $$n$$ 번째 item 선택. $$n-1$$ 번째 item 미선택. $$n-2$$ 번째 item 선택.
2) $$n$$ 번째 item 미선택. $$n-1$$ 번째 item 선택. $$n-2$$ 번째 item 미선택.

두 경우 모두 남은 item이 $$n-3$$ 개로 같고, 무게 또한 $$W-20$$ 으로 같다.