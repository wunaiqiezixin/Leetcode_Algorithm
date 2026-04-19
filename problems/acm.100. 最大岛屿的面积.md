# 100. 最大岛屿的面积
[题目链接](https://kamacoder.com/problempage.php?pid=1172)

---
## 思路
遍历矩阵`grid`，当遇到1时(陆地),搜索整个岛屿并标记为已访问   
用一个全局变量`cnt`记录岛屿面积  

### ***C++***
>dfs
```CPP
#include<iostream>
#include<vector>
using namespace std;
 
int dir[4][2] = {0, 1, 1, 0, 0, -1, -1, 0};
int cnt;
void dfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y);
 
int main(void)
{
    int result = 0;
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M, 0));
    vector<vector<bool> > visited(N, vector<bool>(M, false));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
     
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            cnt = 0;
            dfs(grid, visited, i, j);
            if (cnt > result)
                result = cnt;
        }
    }
    cout << result << endl;
    return 0;
}
 
void dfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y)
{
    if (grid[x][y])
    {
        if (!visited[x][y])
        {
            cnt++;
            visited[x][y] = true;
        }
        for (int i = 0; i < 4; ++i)
        {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                continue;
            if (grid[dx][dy] && !visited[dx][dy])
            {
                cnt++;
                visited[dx][dy] = true;
                dfs(grid, visited, dx, dy);
            }
        }
    }
}
/**************************************************************
    Problem: 1172
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:54 ms
    Memory:2176 kb
****************************************************************/
```

>bfs
```CPP
#include<iostream>
#include<queue>
#include<vector>
using namespace std;
 
int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};
int cnt;
void bfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y);
 
int main(void)
{
    int result = 0;
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M, 0));
    vector<vector<bool> > visited(N, vector<bool>(M, false));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
     
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            cnt = 0;
            bfs(grid, visited, i, j);
            if (result < cnt)
                result = cnt;
        }
    }
    cout << result << endl;
    return 0;
}
 
void bfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y)
{
    if (grid[x][y] && !visited[x][y])
    {
        cnt++;
        visited[x][y] = true;
        queue<pair<int, int> > que;
        que.push({x, y});
        while (!que.empty())
        {
            auto cur = que.front();que.pop();
            for (int i = 0; i < 4; ++i)
            {
                int dx = cur.first + dir[i][0];
                int dy = cur.second + dir[i][1];
                if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                    continue;
                if (grid[dx][dy] && !visited[dx][dy])
                {
                    cnt++;
                    visited[dx][dy] = true;
                    que.push({dx, dy});
                }
            }
        }
    }
}
/**************************************************************
    Problem: 1172
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:49 ms
    Memory:2180 kb
****************************************************************/
```
