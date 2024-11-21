## 题目列表
- 1.位运算
- 2.N个阶乘求和的例子

### 1.位运算
```C++
#include <iostream>
#include <limits>

void Print(int num)
{
	for (int i = 31; i >= 0; --i)
	{
		std::cout << ((num & (1 << i)) ? "1" : "0");
	}

	std::cout << std::endl;
}

int main()
{
	// 整数最大值
	int maxValue = std::numeric_limits<int>::max();
	Print(maxValue);

	// 整数最小值
	int minValue = std::numeric_limits<int>::min();
	Print(minValue);
	Print(~minValue + 1);  // 和minValue是相同的
	Print(minValue >> 1);  // 右移补充符号位

	int a = 1, b = -1;
	Print(a);
	Print(~a);  // 取反
	Print(b);
	Print(~b);

	int c = 5;
	int d = (~c + 1);
	std::cout << c << std::endl;  // 5
	std::cout << d << std::endl;  // -5
	return 0;
}
```

### 2.N个阶乘求和的例子
```C++
#include <iostream>
long Factoral(long N);
long Method01(long N);  // O(n^2)
long Method02(long N);  // O(n)

using namespace std;

long main()
{
	long N = 25;
	cout << Method01(N) << endl;
	cout << Method02(N) << endl;
	return 0;
}

long Factoral(long N)
{
	long sum = 1;
	for (long i = 1; i <= N; ++i)
	{
		sum *= i;
	}

	return sum;
}

long Method01(long N)  // O(n^2)
{
	if (N <= 0)
	{
		return -1;
	}

	long sum = 0;
	for (long i = 1; i <= N; i++)
	{
		sum += Factoral(i);
	}

	return sum;
}

long Method02(long N)  // O(n)
{
	if (N <= 0)
	{
		return -1;
	}

	long cur = 1;
	long sum = 0;

	for (long i = 1; i <= N; i++)
	{
		cur *= i;
		sum += cur;
	}

	return sum;
}
```