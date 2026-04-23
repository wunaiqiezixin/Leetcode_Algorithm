# 106. 海岸线计算
[题目链接](https://kamacoder.com/problempage.php?pid=1178)

---

```CPP
#include<iostream>
#include<vector>
using namespace std;
 
int dir[4][2] = {1, 0, -1, 0, 0, 1, 0, -1};
 
int main(void)
{
    int result = 0;
    int i, j;
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M));
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
     
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j])
            {
                for (int k = 0; k < 4; ++k)
                {
                    int di = i + dir[k][0];
                    int dj = j + dir[k][1];
                    if (di < 0 || dj < 0 || di >= N || dj >= M || !grid[di][dj])
                        result++;
                }
            }
        }
    }
    cout << result << endl;
    return 0;
}
/**************************************************************
    Problem: 1178
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:56 ms
    Memory:2176 kb
****************************************************************/
```
