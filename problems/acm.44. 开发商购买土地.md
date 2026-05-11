# 44. 开发商购买土地
[题目链接](https://kamacoder.com/problempage.php?pid=1044)

---
## 思路
>二维前缀

### ***C++***
```CPP
#include <iostream>
#include <cstring>
#include <climits>
using namespace std;
int main()
{
    int n, m;
    //二维前缀和数组
    int prefixSum[101][101];
    memset(prefixSum, 0, sizeof(prefixSum));
    cin >> n >> m;
    //统计前缀和
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
        {
            int x;
            cin >> x;
            prefixSum[i][j] = prefixSum[i - 1][j] + prefixSum[i][j - 1] - 
                              prefixSum[i - 1][j - 1] + x;
        }
    int ans = INT_MAX;
    //横向切分
    for (int i = 1; i < n; ++i)
    {
        int upValue = prefixSum[i][m];
        int diff = abs(prefixSum[n][m] - 2 * upValue);
        ans = min(ans, diff);
    }
    //纵向切分
    for (int j = 1; j < m; ++j)
    {
        int downValue = prefixSum[n][j];
        int diff = abs(prefixSum[n][m] - 2 * downValue);
        ans = min(ans, diff);
    }
    cout << ans;
    return 0;
}
```

### **时间复杂度**
**使用二维前缀和预处理矩阵，预处理时间 O (nm)，枚举所有切分方式 O (n+m)，整体时间复杂度 O (nm)，是最优解法。**
