---
layout: post
title: "최소 신장 트리(Minimum Spanning Tree, MST) "
slug: "mst"
date: 2018-08-20 20:46:00 +0900
categories: blog/algorithm
---



# 최소 신장 트리(Minimum Spanning Tree, MST) 



## 최소 신장 트리란?

최소 신장 트리(Minimum Spanning Tree, MST) 란 그래프의 모든 정점(vertex) 를 포함하는 트리 중 간선의 가중치 합이 가장 작은 트리를 말한다.

![MST1.jpg](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/08/20/MST1.jpg?raw=true)

위의 그림에서 하나의 그래프는 여러 개의 신장 트리를 가질 수 있으며, 그 중 간선의 가중치 합이 가장 작은 것이 최소 신장 트리임을 보인다.





## 최소 신장 트리 구하기

주어진 그래프에서 최소 신장 트리 구하는 방법은

1) 크루스칼 알고리즘 (Kruskal's Algorithm)
2) 프림 알고리즘 (Prim's Algorithm) 

이 있다.

