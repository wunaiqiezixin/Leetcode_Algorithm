# 101. 孤岛的总面积
[题目链接](https://kamacoder.com/problempage.php?pid=1173)

---
## 思路
对矩阵`grid`边缘的陆地(1)进行搜索，将与其连着的整座岛屿置为0    

### ***C++***
>dfs
```CPP
#include<iostream>
#include<vector>
using namespace std;
 
int dir[4][2] = {0, 1, 0, -1, 1, 0, -1,0};
 
void dfs(vector<vector<int> > & grid,int x, int y)
{
        if (grid[x][y])
        grid[x][y] = 0;
        for (int i = 0; i < 4; ++i)
        {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                continue;
            if (grid[dx][dy])
            {
                grid[dx][dy] = 0;
                dfs(grid, dx, dy);
            }
        }
     
}
 
int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
    for (i = 0; i < N; ++i)
    {
        if (grid[i][0])
            dfs(grid, i, 0);
        if (grid[i][M - 1])
            dfs(grid, i, M - 1);
    }
    for (j = 0; j < M; ++j)
    {
        if (grid[0][j])
            dfs(grid, 0, j);
        if (grid[N - 1][j])
            dfs(grid, N - 1, j);
    }
    int cnt = 0;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            if (grid[i][j])
                cnt++;
    cout << cnt << endl;
    return 0;
}
/**************************************************************
    Problem: 1173
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:47 ms
    Memory:2176 kb
****************************************************************/
```

>bfs

```CPP
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
 
int dir[4][2] = {0, 1, 0, -1, 1, 0, -1,0};
 
 
void bfs(vector<vector<int> > & grid, int x, int y)
{
    if (grid[x][y])
    {
        grid[x][y] = 0;
        queue<int> que;
        que.push(x);que.push(y);
        while (!que.empty())
        {
            int xx = que.front();que.pop();
            int yy = que.front();que.pop();
            for (int i = 0; i < 4; ++i)
            {
                int dx = xx + dir[i][0];
                int dy = yy + dir[i][1];
                if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                    continue;
                if (grid[dx][dy])
                {
                    grid[dx][dy] = 0;
                    que.push(dx);que.push(dy);
                }
            }
        }
    }
}
 
int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
    for (i = 0; i < N; ++i)
    {
        if (grid[i][0])
            bfs(grid, i, 0);
        if (grid[i][M - 1])
            bfs(grid, i, M - 1);
    }
    for (j = 0; j < M; ++j)
    {
        if (grid[0][j])
            bfs(grid, 0, j);
        if (grid[N - 1][j])
            bfs(grid, N - 1, j);
    }
    int cnt = 0;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            if (grid[i][j])
                cnt++;
    cout << cnt << endl;
    return 0;
}
/**************************************************************
    Problem: 1173
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:45 ms
    Memory:2180 kb
****************************************************************/
```
