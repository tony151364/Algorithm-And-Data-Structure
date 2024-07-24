
- [ ] 黑盒测试 

## 总结
- 左神：真正有魅力的是，你自己用一个自然智慧去想一个尝试，只有这个是最靠谱的。你背什么状态转移都没用，因为下一个新的题过来你就完蛋了。学会尝试是攻克动态规划最需要锻炼的能力。

- 先想base case

## 题目

### 题目一
```
假设有排成一行的N个位置，记为1~N，N一定大于或等于2。
开始时机器人在其中的M位置上（M一定是1~N中的一个）
如果机器人来到1位置，那么下一步只能往右来到2位置
如果机器人来到N位置，那么下一步只能向左走来到N-1位置
如果机器人来到中间位置，那么下一步可以往左走也可以往右走；
规定机器人必须走K步，最终能来到P位置（P也是1~N中的一个）的方法有多少种？
给定四个参数N、M、K、P,返回方法数
```

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int way1(int start, int step, int aim, int size);
int process1(int cur, int res, int aim, int N);

int way2(int start, int step, int aim, int size);
int process2(int cur, int res, int aim, int N, vector<vector<int>>& dp);

int way3(int start, int step, int aim, int size);

// 基础：递归版本
int way1(int start, int step, int aim, int size)
{
	if (start < 1 || start > size || step < 1 || aim < 1 || aim > size || size < 2)
	{
		return -1;
	}

	return process1(start, step, aim, size);
}

int process1(int cur, int res, int aim, int N)
{
	if (res == 0) {
		return cur == aim ? 1 : 0;
	}

	// rest > 0，还有步数要走
	if (cur == 1) {
		return process1(2, res - 1, aim, N);
	}

	if (cur == N) {
		return process1(N - 1, res - 1, aim, N);
	}

	return process1(cur - 1, res - 1, aim, N)
		+ process1(cur + 1, res - 1, aim, N);
}

// 优化1：从顶向下的动态规划，又称“记忆化搜索”
int way2(int start, int step, int aim, int size) 
{
	if (start < 1 || start > size || step < 1 || aim < 1 || aim > size || size < 2)
	{
		return -1;
	}

	// dp就是缓存表
	vector<vector<int>> dp(size + 1, vector<int>(step + 1, -1));

	return process2(start, step, aim, size, dp);
}

int process2(int cur, int res, int aim, int N, vector<vector<int>>& dp)
{
	if (dp[cur][res] != -1)
	{
		return dp[cur][res];
	}

	int ans = 0;

	if (res == 0) {
		ans = (cur == aim ? 1 : 0);
	}
	else if (cur == 1) {
		ans = process2(2, res - 1, aim, N, dp);
	}
	else if (cur == N) {
		ans = process2(N - 1, res - 1, aim, N, dp);
	}
	else
	{
		ans = process2(cur - 1, res - 1, aim, N, dp)
		    + process2(cur + 1, res - 1, aim, N, dp);
	}

	dp[cur][res] = ans;
	return ans;
}

// 优化2：动态规划版本（会撞墙的杨辉三角形）
// 动态规划是结果，不是原因。你的尝试就是你转移的东西。一步一步推出动态规划。
int way3(int start, int step, int aim, int size)
{
	if (start < 1 || start > size || step < 1 ||aim < 1 || aim > size || size < 2)
	{
		return -1;
	}

	vector<vector<int>> dp(size + 1, vector<int>(step + 1, 0));

	// 第一列
	dp[aim][0] = 1;

	// 其他列
	for (int rest = 1; rest <= step; ++rest)
	{
		// 第一行
		dp[1][rest] = dp[2][rest - 1];

		// 中间行
		for (int cur = 2; cur < size; ++cur) 
		{
			dp[cur][rest] = dp[cur - 1][rest - 1] + dp[cur + 1][rest - 1];
		}

		// 最后一行
		dp[size][rest] = dp[size - 1][rest - 1];
	}

	return dp[start][step];
}

int main()
{
	cout << "way1: " << way1(2, 6, 4, 5) << endl;
	cout << "way2: " << way2(2, 6, 4, 5) << endl;
	cout << "way3: " << way3(2, 6, 4, 5) << endl;
	return 0;
}
```

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