---
layout:       post
title:        "洛谷P1160队列安排题解"
author:       "ZZJ"
header-style: text
catalog:      true
tags: 
    - C++
    - 题解
    - 洛谷
---

#### [点击跳题](https://www.luogu.com.cn/problem/P1160)

### 注释设计原则说明：

```txt
结构分层：用空行分隔代码块，用注释划分「头文件/全局变量/函数/主程序」模块
功能说明：每个代码段上方添加功能描述，如// 插入阶段帮助理解程序流程
难点解释：对特殊语法（如--lst.end()）添加说明，避免理解障碍
技术细节：解释STL容器的选择原因（链表适合频繁插入删除，哈希表实现快速查找）
输入输出：标注cin/cout的操作对象，明确数据流向
代码安全：在删除函数中强调存在性检查的重要性，防止空指针异常
建议小白结合调试工具逐行观察链表和哈希表的变化，能更直观理解数据结构的运作机制。
```

### 示范代码

```cpp
//=====头文件引入=====
#include<iostream>        // 输入输出流库（用于cin/cout）
#include<list>            // 双向链表容器（用于存储数据序列）
#include<unordered_map>   // 哈希表容器（用于快速查找节点位置）
using namespace std;      // 使用标准命名空间（简化代码书写）

//=====全局变量声明=====
int n, m;                 // n-初始元素个数，m-删除操作次数
list<int> lst;            // 双向链表（存储实际数据，支持快速插入/删除）
unordered_map<int, list<int>::iterator> nodes; // 哈希表（记录数值对应的链表节点位置）

//=====函数定义=====
// 插入函数：将x插入到k节点的左侧(p=0)或右侧(p=1)
void ins(int x, int k, int p) {
    auto it = nodes[k];   // 通过哈希表快速找到k对应的链表位置
    if (p) it++;          // 如果p=1（右侧插入），将迭代器后移一位
    nodes[x] = lst.insert(it, x); // 在指定位置插入新节点，并更新哈希表记录[citation:5]
}

// 删除函数：从链表和哈希表中移除指定数值
void del(int k) {
    auto it = nodes.find(k);      // 检查哈希表中是否存在该数值
    if (it != nodes.end()) {      // 如果存在则执行删除
        lst.erase(it->second);    // 从链表中删除节点
        nodes.erase(it);          // 从哈希表删除记录[citation:4]
    }
}

//=====主程序=====
int main() {
    // 初始化：链表必须包含初始元素1
    lst.push_back(1);             // 向链表尾部插入元素1
    nodes[1] = --lst.end();       // 记录1的位置（end()是尾后迭代器需前移一位）[citation:3]

    // 插入阶段：添加n-1个新元素
    cin >> n;
    for (int i = 2; i <= n; i++) {
        int k, p;
        cin >> k >> p;           // 读取基准元素k和插入方向p
        ins(i, k, p);             // 执行插入操作
    }

    // 删除阶段：执行m次删除
    cin >> m;
    while (m--) {
        int k;
        cin >> k;                // 读取要删除的数值
        del(k);                   // 执行删除操作
    }

    // 输出结果：遍历链表打印最终序列
    for (int val : lst) {         // 范围for循环遍历链表
        cout << val << " ";       // 输出数值+空格分隔
    }
    return 0;                     // 程序正常退出
}
```