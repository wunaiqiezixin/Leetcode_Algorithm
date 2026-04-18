# 98. 可达路径
[题目链接](https://kamacoder.com/problempage.php?pid=1170)

---
>图的存储(邻接矩阵、邻接表) + dfs

### 邻接矩阵
### ***C++***
```CPP
#include<iostream>
#include<vector>
using namespace std;
 
vector<vector<int> > result;//存所有路径
vector<int> path;//存放一条路径
 
//深度优先搜索(x->N的所有路径)
void dfs(vector<vector<int> > & graph, int x, int N)
{
    if (x == N)//达到终点
    {
        result.push_back(path);
        return;//返回，寻找其他路径
    }
    //从当前接节点寻找路径
    for (int i = 1; i <= N; ++i)
    {
        if (graph[x][i])//连通
        {
            path.push_back(i);//加入路径
            dfs(graph, i, N);
            path.pop_back();//撤销
        }
    }
}
 
int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> > graph(N + 1, vector<int>(N + 1, 0));
    while (M -- )
    {
        int a, b;
        cin >> a >> b;
        graph[a][b] = 1;
    }
     
    path.push_back(1);//从1出发
    dfs(graph, 1, N);
 
    //输出
    if (result.empty())
    {
        cout << -1 << endl;
        return 0;
    }
    for (int i = 0; i < result.size(); ++i)
    {
        cout << result[i][0];
        for (int j = 1; j < result[i].size(); ++j)
            cout << " " << result[i][j];
        cout << endl;
    }
    return 0;
}
/**************************************************************
    Problem: 1170
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:101 ms
    Memory:2312 kb
****************************************************************/
```
### 邻接表
```CPP
#include<iostream>
#include<vector>
#include<list>
using namespace std;
 
vector<vector<int> > result;
vector<int> path;
 
void dfs(vector<list<int> > & graph, int x, int N);
 
int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<list<int> > graph(N + 1);
    while (M -- )
    {
        int a, b;
        cin >> a >> b;
        graph[a].push_back(b);
    }
 
    path.push_back(1);
    dfs(graph, 1, N);
 
    if (result.empty())
    {
        cout << -1 << endl;
        return 0;
    }
    for (int i = 0; i < result.size(); ++i)
    {
        cout << result[i][0];
        for (int j = 1; j < result[i].size(); ++j)
            cout << " " << result[i][j];
        cout << endl;
    }
    return 0;
}
 
void dfs(vector<list<int> > & graph, int x, int N)
{
    if (x == N)
    {
        result.push_back(path);
        return;
    }
     
    for (auto it = graph[x].begin(); it != graph[x].end(); it++)
    {
        path.push_back(*it);
        dfs(graph, *it, N);
        path.pop_back();
    }
}
/**************************************************************
    Problem: 1170
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:97 ms
    Memory:2332 kb
****************************************************************/
```
