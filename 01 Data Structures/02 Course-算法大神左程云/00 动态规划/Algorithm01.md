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