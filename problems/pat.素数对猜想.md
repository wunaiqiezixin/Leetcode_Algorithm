# 1007 素数对猜想
[题目链接](https://pintia.cn/problem-sets/994805260223102976/exam/problems/type/7?problemSetProblemId=994805317546655744)

---
### 🧠本题就是为筛法求素数而出的
>定义一个bool类型的数组，先全部初始化为true  
>从下标2开始遍历数组，如果对应的值为true，就将它的倍数的下标的值全设为false  
>这样我们就求出了数组下标范围内的所有素数  
>最后在从下标3开始两步两步的遍历数组，如果遍历到的值和它后两位的值都是true，则素数对加1
### 代码实现
### ***C:***
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<stdbool.h>
bool* isPrime;
int main(void)
{
	int N;
	scanf("%d", &N);
	//动态分配内存
	isPrime = (bool*)malloc(sizeof(bool) * N + 1);
	//将数组的值全设为true
	int i;
	for (i = 2; i <= N; i++)
	{
		isPrime[i] = true;
	}
	//遍历数组，如果遇到的下标对应的值为true，就把其下标的倍数的值都设为false
	for (i = 2; i <= N; i++)
	{
		if (isPrime[i])
		{
			int j;
			for (j = 2 * i; j <= N; j += i)
				isPrime[j] = false;
		}
	}
	//再次遍历数组，统计素数对的个数
	int total = 0;
	for (i = 3; i < N - 1; i += 2)
	{
		if (isPrime[i] && isPrime[i + 2])
			total++;
	}
	printf("%d", total);
	//释放内存
	free(isPrime);
}
```
