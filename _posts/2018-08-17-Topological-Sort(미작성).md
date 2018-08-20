---
layout: post
title: "Topological Sort"
slug: "topological-sort"
date: 2018-08-17 22:46:00 +0900
categories: blog/algorithm
---



# Topological Sort(위상 정렬)

위상정렬을 가장 잘 설명해 줄 수 있는 예로 대학의 선이수과목(prerequisite) 구조를 예로 들 수 있다. 만약 특정 수강과목에 선수과목이 있다면 그 선수 과목부터 수강해야 한다. 이와 같이 선후 관계가 정의된 그래프 구조 상에서 선후 관계에 따라 정렬하기 위해 위상 정렬을 이용할 수 있다.  위상 정렬이 성립하기 위해서는 반드시 그래프의 순환이 존재하지 않아야 한다. 이 점에서 비순환 [유향 그래프](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%96%A5_%EA%B7%B8%EB%9E%98%ED%94%84)(directed acyclic graph) 이다. 
