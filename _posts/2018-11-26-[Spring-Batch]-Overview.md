---
layout: post
title: "[Spring Batch] Overview"
slug: "spring_batch-overview"
date: 2018-11-26 16:00:00 +0900
categories: blog/spring
---



# [Spring Batch] Overview

## What is Batch Processing?

**Batch processing** 이란 일련의 자동화된 complex job 을 user interaction 없이 수행하는 것이다. 일반적으로 batch process는 **대용량의 data와 지속실행**이 특징이다.

Batch process는 다음 경우에 보통 사용된다:

- Time-based event
- Large datasets에 반복적으로 수행
- 트랜잭션 방식으로 data의 processing 과 validation



## What is Spring Batch?

Spring batch는 batch application을 제작하기 쉽도록 도와주는 **light weight framework** 이다. bulk processing 외에도 spring batch는 다음과 같은 기능을 제공한다:

- Logging 과 Tracing
- Transaction 관리
- Job processing 통계
- Job restart
- Skip and Resource management



## Feature of Spring Batch

- Flexibility - application의 processing 순서를 바꾸기 위해서는 XML 파일만 변경하면 된다.

- Maintainability - Spring batch job은 step들로 구성되어 있고, 각 step들은 분리, 테스트, 업데이트를 다른 step과 영향없이 진행할 수 있다.

- Scalability - Portioning technique들을 통해서 application을 parallel execution으로 scale하게 만들 수 있다. 

- Reliability - Application에 failure가 발생했을 때, job을 정확히 실패한 지점에서 다시 수행할 수 있다.

- Support for multiple file formats

- Multiple ways to launch a job




### Reference

- https://www.tutorialspoint.com/spring_batch/spring_batch_overview.htm