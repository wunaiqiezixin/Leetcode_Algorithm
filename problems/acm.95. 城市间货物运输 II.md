# 95. 城市间货物运输 II
[题目链接](https://kamacoder.com/problempage.php?pid=1153)

---
>进行第n次松弛，如果minDist数组会改变，说明存在负权回路

### ***C++***
```CPP
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

int main()
{
    //优化输入输出
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    //读入地图
    int n, m;
    cin >> n >> m;
    vector<vector<int> > edges;//所有的边
    while (m --)
    {
        int s, t, val;
        cin >> s >> t >> val;
        edges.push_back({s,t,val});
    }
    //记录每个节点到源点的最短距离
    vector<int> minDist(n + 1, INT_MAX);
    int start = 1, end = n;
    minDist[start] = 0;//初始化
    //对所有的边进行n-1次松弛
    for (int i = 1; i < n; ++i)
    {
        for (vector<int>& vec : edges)
        {
            int from = vec[0];
            int to = vec[1];
            int value = vec[2];
            if (minDist[from] != INT_MAX && 
                minDist[to] > minDist[from] + value)
                minDist[to] = minDist[from] + value;
        }
    }
    //再次松弛，判断是否存在负权回路
    for (vector<int>& vec : edges)
    {
        int from = vec[0];
        int to = vec[1];
        int value = vec[2];
        if (minDist[from] != INT_MAX && 
            minDist[to] > minDist[from] + value)
        {
            cout << "circle" << '\n';
            return 0;
        }
    }
    if (minDist[end] == INT_MAX)
        cout << "unconnected" << '\n';
    else
        cout << minDist[end] << '\n';
    return 0;
}
```
