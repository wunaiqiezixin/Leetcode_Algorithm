# 96. 城市间货物运输 III
[题目链接](https://kamacoder.com/problempage.php?pid=1154)

---
>单源有限最短路


>最朴素代码

```CPP
#include <iostream>
#include <vector>
#include <climits>
using namespace std;
int main()
{
    //输入输出优化
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    //读入数据
    int n, m;
    cin >> n >> m;
    vector<vector<int> > edges;//记录所有的边
    while (m --)
    {
        int s, t, val;
        cin >> s >> t >> val;
        edges.push_back({s, t, val});
    }
    int src, dst, k;
    cin >> src >> dst >> k;
    //源点到节点的最近距离
    vector<int> minDist(n + 1, INT_MAX);
    minDist[src] = 0;
    vector<int> copy_minDist(n + 1);//数组的拷贝
    //进行k+1次松弛(经过k+1条边)
    for (int i = 1; i <= k + 1; ++i)
    {
        copy_minDist = minDist;//拷贝上一次的minDist
        for (vector<int>& vec : edges)
        {
            int from = vec[0];
            int to = vec[1];
            int value = vec[2];
            if (copy_minDist[from] != INT_MAX && 
                minDist[to] > copy_minDist[from] + value)
                minDist[to] = copy_minDist[from] + value;
        }
    }
    //输出 
    if (minDist[dst] == INT_MAX)
        cout << "unreachable" << '\n';
    else 
        cout << minDist[dst] << '\n';
    return 0;
}
```
