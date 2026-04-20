# 104. 建造最大岛屿
[题目链接](https://kamacoder.com/problempage.php?pid=1176)

---
### ***C++***
```CPP
#include<iostream>
#include<vector>
#include<unordered_map>
#include<unordered_set>
using namespace std;
 
int cnt;
int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};
 
void dfs(vector<vector<int> > & grid, int x, int y, int mark)
{
    if (grid[x][y] != 1)
        return;
    grid[x][y] = mark;
    cnt++;
    for (int i = 0; i < 4; ++i)
    {
        int dx = x + dir[i][0];
        int dy = y + dir[i][1];
        if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
            continue;
        if (grid[dx][dy] == 1)
            dfs(grid, dx, dy, mark);
    }
}
 
int main(void)
{
    int result = 0;
    unordered_map<int, int> gridNum;
    unordered_set<int> st;
     
    int N, M;
    cin >> N >> M;
    int i, j;
    vector<vector<int> > grid(N, vector<int>(M));
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
     
    int mark = 2;
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j] == 1)
            {
                cnt = 0;
                dfs(grid, i, j, mark);
                gridNum[mark] = cnt;
                mark++;
            }
        }
    }
 
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j] == 0)
            {
                cnt = 1;
                st.clear();
                for (int k = 0; k < 4; ++k)
                {
                    int dx = i + dir[k][0];
                    int dy = j + dir[k][1];
                    if (dx < 0 || dy < 0 || dx >= N || dy >= M)
                        continue;
                    if (grid[dx][dy] != 0 && !st.count(grid[dx][dy]))
                    {
                        cnt += gridNum[grid[dx][dy]];
                        st.insert(grid[dx][dy]);
                    }
                }
            }
            result = max(result, cnt);
        }
    }
 
    cout << result << endl;
    return 0;
}
 
 
/**************************************************************
    Problem: 1176
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:50 ms
    Memory:2184 kb
****************************************************************/
```
