---
layout:       post
title:        "C++ A*B problem（高精度）"
author:       "ZZJ"
header-style: text
catalog:      true
tags: 
    - C++
---

# C++ A*B problem（高精度算法）

在处理非常大的整数时，常规的整型变量无法存储，这时我们可以用字符串表示数字，并通过模拟手工计算的方式实现加法。本文将解析一个实现大数相加的C++程序。

## 算法思路

1. **逆序存储数字**：将输入的两个字符串逆序存入整型数组，方便从低位到高位逐位相加。
2. **逐位相加并处理进位**：每一位相加后，计算当前位的值及进位。
3. **处理最高位进位**：若最高位相加后产生进位，需增加结果位数。
4. **逆序输出结果**：将结果数组逆序输出，得到正确的顺序。

## 代码实现及注释

```cpp
#include <bits/stdc++.h>  // 包含常用头文件，方便竞赛编程使用
using namespace std;

// 将字符串逆序转换为整型数组
void StrToArr(string s, int m[]) {
    // 从字符串末尾开始遍历，逆序存储到数组
    for (int i = 0; i < (int)s.length(); i++) {
        // s.length() - 1 - i 实现逆序访问字符
        m[i] = s[s.length() - 1 - i] - '0'; // 字符转数字
    }
}

int main() {
    string a, b;
    int ans[510], n[510], m[510]; // 结果数组及两个加数数组
    // 初始化数组为0
    memset(ans, 0, sizeof(ans));
    memset(n, 0, sizeof(n));
    memset(m, 0, sizeof(m));
    
    cin >> a >> b; // 输入两个大数
    
    // 将字符串转换为逆序数组
    StrToArr(a, n);
    StrToArr(b, m);
    
    int len = max(a.size(), b.size()); // 计算最大长度
    
    // 逐位相加并处理进位
    for (int i = 0; i < len; i++) {
        ans[i] += n[i] + m[i]; // 当前位相加
        ans[i + 1] += ans[i] / 10; // 处理进位
        ans[i] %= 10; // 取个位数作为当前位结果
    }
    
    // 检查最高位是否有进位
    if (ans[len]) {
        len++;
    }
    
    // 逆序输出结果
    for (int i = len - 1; i >= 0; i--) {
        cout << ans[i];
    }
    
    return 0;
}
