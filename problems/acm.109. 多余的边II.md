# 109. 多余的边II
[题目链接](https://kamacoder.com/problempage.php?pid=1182)

---

### ***C++***
```CPP
#include<iostream>
#include<vector>
using namespace std;
 
int n;
vector<int> father(1001, 0);
void init()
{
    for (int i = 0; i <= n; i++)
        father[i] = i;
}
int find(int x)
{
    return father[x] == x ? x : father[x] = find(father[x]);
}
void join(int u, int v)
{
    u = find(u);
    v = find(v);
    if (u == v)
        return;
    father[v] = u;
}
bool same(int u, int v)
{
    u = find(u);
    v = find(v);
    return u == v;
}
 
bool isTreeAfterRemoveEdge(vector<vector<int> >& edges, int x)//删除入度为2的节点的一条边
{
    init();
    for (int i = 0; i < edges.size(); ++i)
    {
        if (i == x) continue;
        else if (same(edges[i][0], edges[i][1])) return false;
        else join(edges[i][0], edges[i][1]);
    }
    return true;
}
 
void getRemoveEdge(vector<vector<int> >& edges)//已经明确有环
{
    init();
    for (int i = 0; i < edges.size(); ++i)
    {
        if (same(edges[i][0], edges[i][1])) 
        {
            cout << edges[i][0] << " " << edges[i][1] << endl;
            return;
        }
        join(edges[i][0], edges[i][1]);
    }
}
 
 
int main(void)
{
    cin >> n;
    vector<vector<int> > edges;
    vector<int> inDegree(n + 1, 0);
    for (int i = 0; i < n; ++i)
    {
        int s, t;
        cin >> s >> t;
        edges.push_back({s, t});
        inDegree[t]++;
    }
 
    vector<int> vec;//记录入度为2的边，注意倒序遍历
    for (int i = n - 1; i >= 0; --i)
    {
        if (inDegree[edges[i][1]] == 2)
            vec.push_back(i);
    }
 
    if (vec.size() > 0)
    {
        if (isTreeAfterRemoveEdge(edges, vec[0]))
            cout << edges[vec[0]][0] << " " << edges[vec[0]][1] << endl;
        else
            cout << edges[vec[1]][0] << " " << edges[vec[1]][1] << endl;
        return 0;
    }
 
    getRemoveEdge(edges);
 
    return 0;
}
/**************************************************************
    Problem: 1182
    User: odCYZ6thcb9Q39G1YK7K3RtZ97KU [wunaiqiezixin]
    Language: C++
    Result: 正确
    Time:41 ms
    Memory:2312 kb
****************************************************************/

```
