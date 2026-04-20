# 103. 高山流水
[题目链接](https://kamacoder.com/problempage.php?pid=1175)

---
### ***C++***
```CPP
#include<iostream>
#include<vector>
using namespace std;
 
int dir[4][2] = {1, 0, -1, 0, 0, 1, 0, -1};
 
/* 从边界点(x, y)逆流而上，标记所有路径 */
void dfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y)
{
    if (!visited[x][y])
        visited[x][y] = true;
    for (int i = 0; i < 4; ++i)
    {
        int dx = x + dir[i][0];
        int dy = y + dir[i][1];
        if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
            continue;
        if (!visited[dx][dy] && grid[x][y] <= grid[dx][dy])
        {
            visited[dx][dy] = true;
            dfs(grid, visited, dx, dy);
        }
    }
}
 
int main(void)
{
    int N, M;
    cin >> N >> M;
    int i, j;
    vector<vector<int> > grid(N, vector<int>(M, 0));
    vector<vector<bool> > firstBoundary(N, vector<bool>(M, false));
    vector<vector<bool> > secondBoundary(N, vector<bool>(M, false));
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
 
    for (i = 0; i < N; ++i)
    {
        dfs(grid, firstBoundary, i, 0);
        dfs(grid, secondBoundary, i, M - 1);
    }
    for (j = 0; j < M; ++j)
    {
        dfs(grid, firstBoundary, 0, j);
        dfs(grid, secondBoundary, N - 1, j);
    }
 
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            if (firstBoundary[i][j] && secondBoundary[i][j])
                cout << i << " " << j << endl;
    return 0;
}
/**************************************************************
    Problem: 1175
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:58 ms
    Memory:2312 kb
****************************************************************/
```
