---
layout: post
title: "[Spring Batch] Application"
slug: "spring_batch-application"
date: 2018-11-26 18:00:00 +0900
categories: blog/spring
---

# [Spring Batch] Application

Spring Batch는 기본적으로 아래 파일들로 구성되어 있다:

- Configuration file 
- Tasklet/Processor
- Java class Bean (with setters, getters)
- Mapper class 
- Launcher class

![application](https://www.tutorialspoint.com/spring_batch/images/application.jpg)



## Configuration File

Configuration file은 아래와 같다:

1. **Reader** 와 **Writer** (+ **Processor**의 wrapper)
2. **Job** 과 **Step** 
3. **Job Launcher, Job Repository, Data Source, Transaction Manager** 

일반적으로 1과 2를 job으로, 3을 context로 분리해서 둔다.



## Mapper Class

Mapper class는 reader에 맞는 interface를 implement 한다. Reader로 부터 data를 읽고 Java bean 템플릿에 setter로 값을 설정한다.



## Java Bean Class

Data를 표현하는 java class로 setter와 getter로 만들어진다. Data를 표현하는 helper class라고 생각하면 이해가 쉽다. 한 component(reader, writer, processor) 에서 다른 component로 data를 넘길 때, 이 java class의 form으로 넘겨준다.



## Tasklet/Processor

Spring Batch Application의 processing code를 담는다. Processor는 Java Bean Class 형태의 data를 읽고, 처리하고, 처리한 data를 return하는 class이다.



## Launcher Class

Spring Batch Application을 실행하는 class이다.







### 



### 