# 94. 城市间货物运输 I
[题目链接](https://kamacoder.com/problempage.php?pid=1152)

---

>单源最短路径(存在权值为负数的情况)   

### ***C++***
```CPP
#include <iostream>
#include <vector>
#include <climits>
using namespace std;
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, m;
    cin >> n >> m;
    vector<vector<int> > edges;
    while (m --)
    {
        int s, t, val;
        cin >> s >> t >> val;
        edges.push_back({s, t, val});
    }
    vector<int> minDist(n + 1, INT_MAX);
    int start = 1, end = n;
    minDist[start] = 0;
    /* 进行n-1次对每一条边的松弛 */
    for (int i = 1; i < n; ++i)
    {
        for (vector<int>& edge : edges)//注意要用引用
        {
            int from = edge[0];
            int to = edge[1];
            int price = edge[2];
            if (minDist[from] != INT_MAX &&
                minDist[from] + price < minDist[to])
                minDist[to] = minDist[from] + price;
        }
    }
    if (minDist[end] == INT_MAX) cout << "unconnected" << '\n';
    else cout << minDist[end] << '\n';
    return 0;
}
```
