---
layout: post
title: "[Spring Batch] Architecture"
slug: "spring_batch-architecture"
date: 2018-11-26 17:00:00 +0900
categories: blog/spring
---

# [Spring Batch] Architecture

Spring batch의 Architecture는 아래 그림과 같이 **Application, Batch Core, Batch Infrastructure** 로 구성되어 있다.

![architecture](https://www.tutorialspoint.com/spring_batch/images/architecture.jpg)

- Application - 모든 job과 Spring batch framework로 작성된 code
- Batch core - Batch job을 control하고 launch 하기 위한 API classes
- Batch Infrastructure - Application과 Batch core component들이 사용하는 reader, writer, services



## Components of Spring Batch

![component](https://www.tutorialspoint.com/spring_batch/images/components.jpg)



### Job

Spring batch에서 Job은 execute될 batch process이다. Job은 Step들로 구성되어 있다. Job은 XML file이나 Java class로 configure 한다.



### Step

Step은 job의 독립적인 각 부분을 말한다. Job을 정의하고 실행하는데 필수적인 요소이다. 

그림처럼, 한 Step은 ItemReader, ItemProcessor, ItemWriter로 구성되어 있다. Job은 한 개 이상의 Step으로 구성되어 있다.

#### Item Reader, Writer and Processor

**Item reader**는 특정 source에서 Spring Batch application으로 data를 읽어 들이고, 반대로 **Item writer**는 Spring Batch application에서 특정 source로 data를 쓴다. **Item Processor**는 Spring batch로 읽어들인 data를 가공하는 class이다.

Reader와 Writer가 없을 때, **tasklet**이 Spring Batch의 processor 역할을 한다. 



### Job Repository

Job repository는 Job Launcher, Job, Step 을 위한 CRUD operation을 제공한다. 만약, Spring Batch의 domain object들을 database에 유지시키고 싶지 않다면 in-memory version JobRepository를 사용하면 된다.

 

### Job Launcher

Job Launcher는 Spring Batch job을 주어진 parameter들로 실행시키는 interface이다.

