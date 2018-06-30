# Josephus problem

[참조]  www.geeksforgeeks.org/josephus-problem-set-1-a-on-solution 



 이 문제는 유대 역사가 Josephus 의 이야기에서 유래되었다고 한다. 당시 유대인 지도자 Josephus와 그의 추종자들은 로마의 지배에 맞서 싸웠지만, 위기에 처하자 동굴로 숨어들어 명예롭게 자결하기로 하였다.

>   *"Josephus 와 그의 병사 총 41명은 원형으로 둘러 앉아 남은 사람이 없을 때 까지 세번째 사람마다 죽이기로 했다. Joshphus는 막상 죽음에 직면하자 살고싶은 마음에서 마지막에 살아남을 수 있는 자리를 찾아내, 살아남은 다음 로마병사들에게 항복했다고 한다."*

 이 문제를 일반화 시킨다면 다음과 같다.

 *n명의 사람이 원형으로 둘러앉아 있고, k번째 사람이 죽임을 당한다. 이 때, 마지막까지 살아남을 수 있는 사람의 index 를 구하여라*. 

 이 문제의 해는 *n*명의 사람이 있을 때와, 한 명이 죽임을 당하고 난 *n-1*명의 사람이 있을 때를 비교하여, 점화식을 세우면 쉽게 해결할 수 있다.

![josephus_1.JPG](https://github.com/sjnov11/sjnov11.github.com/blob/master/_img/2018/06/17/josephus_1.JPG?raw=true) 

 *n* 과 *k* 가 주어졌을 때, 마지막에 살아남는 사람의 *index* 를 *Josephus(n, k)* 라고 하자. 위의 그림에서 *n=6* 이고 *k=3* 으로 주어졌다. 첫번째 *k*번째 사람을 죽이고 나서의 모습은 우측 그림과 같은 모습이다. 여기서 다시 count 하는 시작점을 0으로 두게 된다면, 빨간색 숫자의 경우에서 마지막으로 살아남는 사람의 *index*는 *Josephus(n-1, k)* 이다. 즉, *n=5* 인 경우에서 마지막으로 살아남는 사람의 *index* 에다가 *n=6*에서 한 사람을 죽인 다음 바뀐 위치를 더해주면(이 경우 k=3) 답을 쉽게 구할 수 있다. 

$$
Josephus(n, k) = (Josephus(n-1, k) + k) \ mod\  n \\
Josephus(1,k) = 0
$$
 *n=1* 일 때는 마지막 사람만이 존재하므로 *index = 0* 이 되며, *k*만큼 더해주었을 때 *n*보다 클 경우는 circular 형태이므로 모듈러 연산을 통해 그 위치를 바꾸어준다. 즉, 더했을 때, *n=6*보다 크다면, 6으로 나누어 준 값의 나머지가 정답이 된다.

 이 알고리즘은 순차적으로 한번씩 연산을 하므로 $$O(n)$$이 된다.



## SW Expert Arcademy #4411

[문제 #4411 덕환이의 카드뽑기]  https://www.swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWNcL9nKpbEDFAV8



 *Josephus* 문제를 이용해서 푼다면 매우 쉽게 해결할 수 있다. 이 때, *n*의 숫자와 *k*의 숫자가 매우 크므로, top-down 방식으로는 stack overflow가 발생할 수 있으므로, bottom-up 방식으로 진행하였다.



```c++
#include<iostream>
using namespace std;

long long josephus(long long n, long long k) {
	//if (n == 1) {
	//	return 0;
	//}
	////cout << "n: " << n << "/ k: " << k << endl;
	//return (josephus(n - 1, k) + (k + 1)) % n;
	
	long long result = 0;

	for (long long i = 2; i <= n; i++) {
		result = (result + (k + 1)) % i;
	}

	return result;
}


int main(int argc, char** argv)
{
	int test_case;
	int T;
	
	cin >> T;
	/*
	여러 개의 테스트 케이스가 주어지므로, 각각을 처리합니다.
	*/
	for (test_case = 1; test_case <= T; ++test_case)
	{
		long long N, K;
		cin >> N >> K;
		cout << "#" << test_case << " " << (josephus(N, K) + 1) % (N + 1) << endl;
	}

	return 0;//정상종료시 반드시 0을 리턴해야합니다.
}
```

