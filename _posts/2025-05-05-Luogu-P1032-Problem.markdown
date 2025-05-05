---
layout:     post
title:      "洛谷P1032字串变换题解"
date:       2025-05-05
author:     "ZZJ"
header-style: text
catalog: true
tags:
    - C++
    - 洛谷
    - 题解
---

#### [点击跳题](https://www.luogu.com.cn/problem/P1032)

### 思路：
```txt
采用广度优先搜索（BFS）逐层探索所有可能的字符串变换路径。从初始字符串出发，每一步尝试所有替换规则的所有可能位置，生成新字符串并记录步数。用哈希集合去重避免重复处理相同状态，首次遇到目标字符串时的步数即为最小变换次数。若超过10步或队列耗尽仍未找到解，则判定无解。
```
### 题解代码

```cpp
#include <bits/stdc++.h>
using namespace std;

// 结构体：存储当前字符串和变换次数
struct Node {
    string s;    // 当前字符串状态
    int step;    // 已使用的变换次数
};
unordered_set<string> vis; // 记录已访问的字符串，防止重复计算
string a, b, st[7], ed[7]; // st存规则左边，ed存规则右边，最多6条规则
int cnt; // 实际规则数量

int main() {
    cin >> a >> b; // 输入初始字符串a和目标字符串b
    // 读取所有替换规则，例如将st[0]替换为ed[0]
    while (cin >> st[cnt] >> ed[cnt]) cnt++;

    queue<Node> q;  // BFS队列，先进先出保证最少步数
    q.push({a, 0}); // 初始状态：字符串a，0次变换
    vis.insert(a);  // 标记初始字符串已访问

    while (!q.empty()) {
        Node cur = q.front(); // 取出队列最前面的元素
        q.pop();

        // 找到目标直接输出答案
        if (cur.s == b) {
            cout << cur.step;
            return 0;
        }
        // 超过10步不再处理（题目限制）
        if (cur.step >= 10) continue;

        // 遍历当前字符串的每个字符位置
        for (int i = 0; i < cur.s.size(); i++) {
            // 尝试应用每条规则
            for (int j = 0; j < cnt; j++) {
                // 检查当前位置是否能匹配规则左侧
                if (cur.s.substr(i, st[j].size()) == st[j]) {
                    // 生成新字符串：替换规则左侧为右侧
                    string news = cur.s.substr(0, i)       // 前半部分
                                + ed[j]                    // 替换内容
                                + cur.s.substr(i + st[j].size()); // 后半部分
                    // 如果新字符串未被访问过
                    if (!vis.count(news)) {
                        vis.insert(news); // 标记为已访问
                        q.push({news, cur.step + 1}); // 入队，步数+1
                    }
                }
            }
        }
    }
    // 无法在10步内变换到目标字符串
    cout << "NO ANSWER!";
    return 0;
}
```