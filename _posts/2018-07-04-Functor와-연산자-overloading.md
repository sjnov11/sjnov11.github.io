---
layout: post
title: "Functor와 연산자 overloading"
slug: "segment-tree"
date: 2018-07-04 17:59:00 +0900
categories: blog/cpp
---

[Functor]:https://www.geeksforgeeks.org/functors-in-cpp/
[Function operator]:https://www.tutorialspoint.com/cplusplus/function_call_operator_overloading.htm



 Functor 와 연산자 overloading 에 관하여 찾아보게 된 계기는 set 컨테이너를 사용 중 사용자 정의 comparator를 만들다 찾아보게 되었다. set 의 템플릿 파라미터로 넘겨줄 comparator는 두 값을 비교하는 **Functor** 를 넘겨 주어야 한다.

```c++
template < class T,                        // set::key_type/value_type
           class Compare = less<T>,        // set::key_compare/value_compare
           class Alloc = allocator<T>      // set::allocator_type
           > class set;
```

 Functor를 구성하는 연산자 overloading, ***operator()*** 에 궁금하여 찾아보다가 기존의 ***operator +*** 같은 연산자의 일종임을 확인하였다. 즉, 함수 호출 시에 function call operator인 ***operator()*** 가 호출이 되는 것이다. functor 에서는 이를 overloading 하여 함수의 인자를 받아온다.



## Functor

 **Functor** 는 함수 또는 함수포인터처럼 사용될 수 있는 객체이다 (**Functors** are objects that can be treated as though they are a function or function pointer.)



```c++
// A Functor
class increment
{
private:
    int num;
public:
    increment(int n) : num(n) {  }
 
    // This operator overloading enables calling
    // operator function () on objects of increment
    int operator () (int arr_num) const {
        return num + arr_num;
    }
};
```

 위와 같은 functor, ***increment*** 를 예로 보자. increment(10)은 increment.operator()(10) 과 똑같이 수행한다.