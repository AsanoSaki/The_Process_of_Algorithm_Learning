# 搜索-多源BFS

## 一、AcWing 173. 矩阵距离
【题目描述】

给定一个$N$行$M$列的$01$矩阵$A$，$A[i][j]$与$A[k][l]$之间的曼哈顿距离定义为：

$dist(A[i][j],A[k][l])=|i−k|+|j−l|$

输出一个$N$行$M$列的整数矩阵$B$，其中：

$B[i][j]=min_{1≤x≤N,1≤y≤M,A[x][y]=1}dist(A[i][j],A[x][y])$

【输入格式】

第一行两个整数$N,M$。

接下来一个$N$行$M$列的$01$矩阵，数字之间没有空格。

【输出格式】

一个$N$行$M$列的矩阵$B$，相邻两个整数之间用一个空格隔开。

【数据范围】

$1≤N,M≤1000$

【输入样例】
```
3 4
0001
0011
0110
```
【输出样例】
```
3 2 1 0
2 1 0 0
1 0 0 1
```

**【分析】**
****
题目要求我们求所有点到最近的为$1$的点的距离，第一种思路是以每个$1$为起点跑一遍$BFS$，维护每个为$0$的点的最小距离，这么做会$TLE$。

我们完全可以认为这道题是**多起点**问题，也就是说，我们直接将所有为$1$的点作为起点同时加入到状态队列之中，那么这道题目就解决了。

****
**【代码】**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>
using namespace std;

typedef pair<int, int> PII;
const int N = 1010;
char g[N][N];
int dis[N][N];
int n, m;
int dx[4] = { 0, 1, 0, -1 }, dy[4] = { 1, 0, -1, 0 };

void bfs()
{
	memset(dis, -1, sizeof dis);
	queue<PII> Q;
	for (int i = 0; i < n; i++)
		for (int j = 0; j < m; j++)
			if (g[i][j] == '1') Q.push({ i, j }), dis[i][j] = 0;
	while (Q.size())
	{
		auto t = Q.front();
		Q.pop();
		for (int i = 0; i < 4; i++)
		{
			int nx = t.first + dx[i], ny = t.second + dy[i];
			if (nx >= 0 && nx < n && ny >= 0 && ny < m && !~dis[nx][ny])
				Q.push({ nx, ny }), dis[nx][ny] = dis[t.first][t.second] + 1;
		}
	}
}

int main()
{
	ios::sync_with_stdio(false);
	cin >> n >> m;
	for (int i = 0; i < n; i++) cin >> g[i];
	bfs();
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < m; j++)
			cout << dis[i][j] << ' ';
		cout << endl;
	}
	return 0;
}
```