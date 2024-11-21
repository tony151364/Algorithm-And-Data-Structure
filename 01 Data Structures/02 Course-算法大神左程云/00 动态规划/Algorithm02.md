
### 题目二
```
给定一个整型数组arr，代表数值不同的纸牌排成一条线玩家A和玩家B依次拿走每张纸牌
规定玩家A先拿，玩家B后拿
但是每个玩家每次只能拿走最左或最右的纸牌玩家A和玩家B都绝顶聪明
请返回最后获胜者的分数。
```

- 方法一：递归版
```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;
int f(vector<int> arr, int L, int R);
int g(vector<int> arr, int L, int R);

// 根据规则，返回获胜者的分数
int way1(vector<int> arr)
{
	if (arr.empty())
	{
		return 0;
	}

	int first = f(arr, 0, arr.size() - 1);
	int second = g(arr, 0, arr.size() - 1);

	if (first > second)
	{
		cout << "先手胜出：";
	}
	else if (first < second)
	{
		cout << "后手胜出：";
	}
	else
	{
		cout << "平局" << endl;
	}
	
	return std::max(first, second);
}

// arr[L..R], 先手获得的最好分数
int f(vector<int> arr, int L, int R)
{
	if (L == R)
	{
		return arr[0];
	}
	else
	{
		int p1 = arr[L] + g(arr, L + 1, R);
		int p2 = arr[R] + g(arr, L, R - 1);
		return std::max(p1, p2);  // 最大值
	}
}

// arr[L..R], 后手获得的最好分数
int g(vector<int> arr, int L, int R)
{
	if (L == R)
	{
		return 0;
	}
	else
	{
		int p1 = f(arr, L + 1, R);  // 对手拿走了L位置的数
		int p2 = f(arr, L, R - 1);  // 对手拿走R位置的数
		return std::min(p1, p2);  // 最小值
	}
}

int main()
{
	std::vector<int> arr = { 5, 7, 4, 5, 8, 1, 6, 0, 3, 4, 6, 1, 7 };
	cout << way1(arr) << endl;
	return 0;
}
```

### 1 斐波那契数列
```
题目描述：
斐波那契数列又称为“兔子数列”：0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55 ...
定义
  F(n) = 0, F(1) = 1
  F(n) = F(n-1) + F(n-2), n >= 2
```
```C++
int F(int n)
 {
    if (n == 0)
        return 0;
    else if(n == 1 || n == 2)
        return 1;
    else
        return F(n-1) + F(n-2);
}

```
- 这个形式有个缺点——存在大量的重复运算，导致时间复杂度为2<sup>n</sup>，F(100)时程序可能就运行不了了。
- 给出两种形式解决这个问题：
```C++
// 方式一：动态规划（记忆化递归法）
int Fibonacci(int n, vector<int>& memo) {
    if (n == 0)
        return 0;
    if (n == 1 || n == 2)
        return 1;
    
    // 如果已经计算过，直接返回保存的值
    if (memo[n] != -1)
        return memo[n];
    
    // 否则，计算并保存结果
    memo[n] = Fibonacci(n - 1, memo) + Fibonacci(n - 2, memo);
    return memo[n];
}

int main() {
    int n = 100;
    vector<int> memo(n + 1, -1);  // 初始化memo数组
    cout << "Fibonacci(" << n << ") = " << Fibonacci(n, memo) << endl;
    return 0;
}
// 时间复杂度优化为 O(n)，每个数字只计算一次，空间复杂度也是O(n)
```
```C++
// 方式二、递推法（迭代法）直接通过迭代从前往后计算斐波那契数列，
int Fibonacci(int n) {
    if (n == 0)
        return 0;
    if (n == 1 || n == 2)
        return 1;

    int prev1 = 1, prev2 = 1, current = 0;
    for (int i = 3; i <= n; i++) {
        current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    return current;
}

int main() {
    int n = 100;
    cout << "Fibonacci(" << n << ") = " << Fibonacci(n) << endl;
    return 0;
}

// 时间复杂度也是O(n)，而且空间复杂度可以优化到 O(1)，只需常数空间来保存前两项。
```