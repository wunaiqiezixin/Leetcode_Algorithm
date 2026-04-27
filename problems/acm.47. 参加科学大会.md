# 47. 参加科学大会
[题目链接](https://kamacoder.com/problempage.php?pid=1047)

---
## 思路
>dijkstra算法

### ***C++***
```CPP
#include <iostream>
#include <vector>
#include <climits>
using namespace std;
int n, m;
int main()
{
    //优化输入输出
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    //读入地图
    cin >> n >> m;
    vector<vector<int> > grid(n + 1, vector<int>(n + 1, 0));
    while (m --)
    {
        int s, t, val;
        cin >> s >> t >> val;
        grid[s][t] = val;
    }
    vector<bool> visited(n + 1, false);
    vector<int> minDist(n + 1, INT_MAX - 1);//每个节点到源点的最短距离
    int start = 1, end = n;
    minDist[start] = 0;//初始化，源点到源点的距离为0
    for (int i = 1; i <= n; ++i)//需要将n个节点访问并标记以及更新距离
    {
        // 1.找未访问的到源点最近的节点
        int cur = 0;
        int curMin = INT_MAX;
        for (int j = 1; j <= n; ++j)
        {
            if (!visited[j] && curMin > minDist[j])
            {
                cur = j;
                curMin = minDist[j];
            }
        }
        // 2.将该节点标记
        visited[cur] = true;
        // 3.更新未访问节点到源点的最短距离
        for (int j = 1; j <= n; ++j)
        {
            if (!visited[j] && grid[cur][j] && 
                grid[cur][j] + minDist[cur] < minDist[j])
                minDist[j] = grid[cur][j] + minDist[cur];
        }
    }
    if (minDist[end] == INT_MAX - 1)
    {
        cout << -1 << '\n';
        return 0;
    }
    cout << minDist[end] << '\n';
    return 0;
}
```


### 堆优化
>加油，相信自己可以去到自己想去到的地方  
>相信自己可以过上平淡的生活

>2026-4-27

```CPP
#include <iostream>
#include <vector>
#include <list>
#include <climits>
#include <queue>
using namespace std;
class comparison
{
public:
    bool operator() (const pair<int, int>& p1, const pair<int, int>& p2) const
    {
        return p1.second > p2.second;
    }
};
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, m;
    cin >> n >> m;
    struct Edge{int to; int val;Edge(int a, int b): to(a), val(b){}};
    vector<list<Edge> > grid(n + 1);
    vector<bool> visited(n + 1, false);
    while (m --)
    {
        int s, t, val;
        cin >> s >> t >> val;
        grid[s].push_back(Edge{t, val});
    }
    int start = 1, end = n;
    priority_queue<pair<int, int>, vector<pair<int, int> >, comparison> pq;//节点，距离源点的最近距离
    pq.push({1, 0});
    vector<int> minDist(n + 1, INT_MAX);
    minDist[1] = 0;
    while (!pq.empty())
    {
        auto cur = pq.top();pq.pop();
        visited[cur.first] = true;
        for (Edge edge : grid[cur.first])
        {
            if (!visited[edge.to] && edge.val + cur.second < minDist[edge.to])
            {
                minDist[edge.to] = edge.val + cur.second;
                pq.push({edge.to, minDist[edge.to]});
            }
        }
    }
    if (minDist[end] == INT_MAX)
        cout << -1 << '\n';
    else 
        cout << minDist[end] << '\n';
    return 0;
}
```

### 笨拙的走，总有一天能走到的
### 好好的
