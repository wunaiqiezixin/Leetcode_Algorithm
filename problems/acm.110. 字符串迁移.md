# 110. 字符串迁移
[题目链接](https://kamacoder.com/problempage.php?pid=1183)

---
### ***C++***
```CPP
#include<iostream>
#include<vector>
#include<string>
#include<unordered_map>
#include<unordered_set>
#include<queue>
using namespace std;
 
int main(void)
{
    /* 读入数据 */
    int n;
    cin >> n;
    string beginStr, endStr;
    cin >> beginStr >> endStr;
    unordered_set<string> strSet;
    unordered_map<string, int> strMap;
    strMap.insert({beginStr, 1});
    strSet.insert(beginStr);
    while (n --)
    {
        string tmp;
        cin >> tmp;
        strSet.insert(tmp);
    }
 
    queue<string> que;
    que.push(beginStr);
    while (!que.empty())
    {
        string word = que.front();que.pop();
        for (int i = 0; i < word.size(); ++i)
        {
            string newWord = word;
            for (int j = 0; j < 26; ++j)
            {
                newWord[i] = 'a' + j;
                if (newWord == endStr)
                {
                    cout << strMap[word] + 1 << endl;
                    return 0;
                }
                else if (strSet.find(newWord) != strSet.end() && 
                strMap.find(newWord) == strMap.end())
                {
                    strMap.insert({newWord, strMap[word] + 1});
                    strSet.insert(newWord);
                    que.push(newWord);
                }
            }
        }
    }
    cout << 0 << endl;
    return 0;
}
/**************************************************************
    Problem: 1183
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:55 ms
    Memory:2188 kb
****************************************************************/
```
