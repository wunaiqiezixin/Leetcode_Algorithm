# 53. 寻宝
[题目链接](https://kamacoder.com/problempage.php?pid=1053)

---
## 思路
- 最小生成树
- Prim算法

### ***C++***
```CPP
#include <iostream>
#include <vector>
#include <climits>
using namespace std;
#define M 10001
int main()
{
    //输入输出优化
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int v, e;
    cin >> v >> e;
    /* 权值初始化为最大10001 */
    vector<vector<int> > graph(v + 1, vector<int>(v + 1, M));
    for (int i = 0; i < e; ++i)
    {
        int s, t, val;
        cin >> s >> t >> val;
        graph[s][t] = graph[t][s] = val;/* 无向图 */
    }
    /* 每个节点距离最小生成树的最小距离 */
    vector<int> minDist(v + 1, M);
    /* 节点是否在最小生成树里面 */
    vector<bool> isInTree(v + 1, false);
    /* v个节点，只需找出v - 1条边，循环v - 1次即可 */
    for (int i = 1; i < v; ++i)
    {
        /* prim三部曲 */
        // 1.选出一个距离生成树最近的节点
        int cur;//距离生成树最近的节点的下标
        int curMin = INT_MAX;//最小距离
        for (int j = 1; j <= v; ++j)
        {
            /*
            * 选取条件：
            * (1)节点不在生成树中
            * (2)节点距离生成树最近
            * */
            if (!isInTree[j] && minDist[j] < curMin)
            {
                cur = j;
                curMin = minDist[j];
            }
        }
        // 2.这个距离最小生成树最近的节点加入最小生成树
        isInTree[cur] = true;

        // 3.更新非生成树节点到生成树的最近距离
        for (int i = 1; i <= v; ++i)
        {
            //只需考虑与新加入生成树节点相连的节点
            /*
            * 更新条件：
            * (1)节点不在生成树里面
            * (2)节点到"新加入生成树节点的距离"小于"原先到生成树的距离"
            *
            * */
            if (!isInTree[i] && minDist[i] > graph[cur][i])
                minDist[i] = graph[cur][i];
        }
    }
    //统计minDist中的和
    int result = 0;
    for (int i = 2; i <= v; ++i)
        result += minDist[i];
    //输出
    cout << result << "\n";
    return 0;
}   
```
