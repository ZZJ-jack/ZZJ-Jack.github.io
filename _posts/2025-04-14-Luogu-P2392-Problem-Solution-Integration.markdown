---
layout:     post
title:      "洛谷P2392 题解整合"
date:       2025-04-14
author:     "ZZJ"
header-style: text
catalog: true
tags:
    - C++
    - 洛谷
---

## 题目

[点击跳题](https://www.luogu.com.cn/problem/P2392)

------------------------

## 题解

### 方法一：递归回溯法
```cpp
/**
 * 核心思想：将每个科目的题目分配到左/右脑，求最小化最大耗时
 * 时间复杂度：O(2^n) 每个科目独立处理
 */
#include<bits/stdc++.h>
using namespace std;

// 全局状态记录
int Left, Right;    // 左右脑当前累计耗时
int minn;           // 当前科目最小化最大耗时
int ans;            // 四科目总耗时
int s[5];           // s[1]-s[4]存储四科题目数
int a[21][5];       // a[题号][科目] 存储具体耗时

void search(int x, int y) { // x:当前题号，y:科目编号
    if(x > s[y]) {          // 终止条件：处理完当前科目所有题目
        minn = min(minn, max(Left, Right)); // 更新最小值
        return;
    }
    // 尝试分配给左脑
    Left += a[x][y];  
    search(x+1, y);         // 递归处理下一题
    Left -= a[x][y];        // 回溯
    
    // 尝试分配给右脑
    Right += a[x][y];  
    search(x+1, y);         // 递归处理下一题
    Right -= a[x][y];       // 回溯
}

int main() {
    for(int i=1; i<=4; i++) cin>>s[i]; // 读取各科题目数
    
    for(int i=1; i<=4; i++) {  // 分别处理四科目
        Left = Right = 0;      // 重置状态
        minn = INT_MAX;       // 初始化极大值
        for(int j=1; j<=s[i]; j++) 
            cin>>a[j][i];     // 读取当前科目各题耗时
            
        search(1, i);         // 开始递归搜索
        ans += minn;          // 累加各科最优解
    }
    cout << ans;
    return 0;
}
```

----------------------------

### 方法二：动态规划法（01背包变种）

```cpp
/**
 * 核心思想：将单科题目转化为背包问题，求sum/2容量的最大装载值
 * 时间复杂度：O(n*sum) 每科独立处理
 */
#include<bits/stdc++.h>
using namespace std;

int a[5];           // 各科题目数
int t;              // 总耗时
int homework[21];   // 当前科目各题耗时
int dp[2501];       // dp数组，容量上限2500（20题*125分钟）

int main() {
    for(int i=1; i<=4; i++) cin>>a[i];
    
    for(int i=1; i<=4; i++) {
        int sum = 0;
        for(int j=1; j<=a[i]; j++) {
            cin >> homework[j];
            sum += homework[j];  // 计算单科总耗时
        }
        
        // 01背包核心：逆向遍历防止重复计算
        for(int j=1; j<=a[i]; j++) 
            for(int k=sum/2; k>=homework[j]; k--) 
                dp[k] = max(dp[k], dp[k-homework[j]] + homework[j]);
        
        t += sum - dp[sum/2];  // 较大半部耗时 = 总耗时 - 较小半部最优解
        memset(dp, 0, sizeof(dp)); // 重置背包处理下一科目
    }
    cout << t;
    return 0;
}
```

---------------------

## 方法对比
| 特征 | 递归回溯法  |  动态规划法  |
| ---- | ---------  |  --------   |
| 时间复杂度 | O(2ⁿ) 每科目 | O(n*sum) 每科目 |
| 空间复杂度 | O(n) 递归栈 | O(sum) 数组空间 |
| 适用数据规模 | n ≤ 20 (单科题目数) | sum ≤ 2500 (单科总耗时) |
| 核心思想 | 暴力枚举所有分组组合 | 转化为背包问题求最优解 |
| 代码复杂度 | 需要回溯逻辑 | 需要背包问题理解 |
| 实际运行效率 | 20题需处理约百万次操作 | 2500容量仅需数万次操作 |
| 优势场景 | 小规模数据直观实现 | 大规模数据高效处理 |
