---
layout:       post
title:        "深入理解C++中的二分查找算法"
author:       "ZZJ"
header-style: text
catalog:      true
tags: 
    - C++
    - 二分
    - 笔记
---

## 一、什么是二分查找？

二分查找（Binary Search）是一种在**有序数组**中快速定位目标值的高效搜索算法。它通过每次将搜索范围减半的方式，将时间复杂度降低到O(log n)，相比线性搜索的O(n)具有显著优势。

**核心思想**：  
通过不断缩小搜索区间来逼近目标值，如同"猜数字"游戏中的策略。

## 二、算法原理

### 前提条件
1. 数组必须有序（升序或降序）
2. 适用于可随机访问的数据结构（如数组）

### 执行步骤
1. 确定初始搜索范围[left, right]
2. 计算中间位置mid
3. 比较mid处的值与目标值
4. 根据比较结果调整搜索边界
5. 重复直到找到目标或区间无效

## 三、C++实现

### 1.基本实现（左闭右闭区间）

```cpp
int binary_search(vector<int>& nums, int target) {
	int left = 0;
	int right = nums.size() - 1; // 闭区间

	while (left <= right) { // 注意等号
		int mid = left + (right - left) / 2; // 防止溢出

		if (nums[mid] == target) 
			return mid;
		else if (nums[mid] < target) l
			eft = mid + 1; // 调整左边界
		else 
			right = mid - 1; // 调整右边界
	}

	return -1; // 未找到

	/*
	关键点分析：
	循环条件：left <= right 保证区间有效性
	边界更新：必须排除已检查的mid位置
	防止溢出：使用left + (right - left) / 2 而非 (left + right) / 2
	*/
}
```


### 2.变种实现（左闭右开区间）
```cpp
int binary_search_variant(vector<int>& nums, int target) {
	int left = 0;
	int right = nums.size(); // 开区间

	while (left < right) { // 无等号
		int mid = left + (right - left) / 2;

		if (nums[mid] == target)
			return mid;
		else if (nums[mid] < target)
			left = mid + 1; // 保持左闭
		else
			right = mid; // 保持右开
	}
	return -1;
}
```
## 四、常见变种问题

### 1. 查找第一个>=target的元素
```cpp
int lower_bound(vector<int>& nums, int target) {
	int left = 0, right = nums.size();
	while (left < right) {
		int mid = left + (right - left) / 2;
		if (nums[mid] >= target)
			right = mid;
		else
			left = mid + 1;
	}
	return left;
}
```
### 2. 查找最后一个<=target的元素
```cpp
int upper_bound(vector<int>& nums, int target) {
	int left = 0, right = nums.size();
	while (left < right) {
		int mid = left + (right - left) / 2;
		if (nums[mid] > target)
			right = mid;
		else
			left = mid + 1;
	}
	return left - 1;
}
```
## 五、注意事项

1. 确保数组有序：二分查找的前提条件

2. 边界处理：不同区间写法的循环条件和边界更新方式不同

3. 整数溢出：推荐使用mid = left + (right - left)/2写法

4. 终止条件：避免出现死循环

5. 元素重复：根据需求选择第一个/最后一个匹配项

6. / 2 可写成 >> 1 ,但注意运算优先级

## 六、应用场景
1. 有序数组查询

2. 数学问题求解（平方根、方程解）

3. 答案二分（最大值最小化问题）

4. 旋转数组搜索

5. 范围查询（如IP地理定位）

## 七、复杂度分析
1. 时间复杂度：O(log n)

2. 空间复杂度：O(1)

## 总结
```txt
二分查找看似简单，实则暗藏诸多细节。掌握核心思想，注意边界处理，通过大量练习培养直觉。该算法不仅是基础算法，更是解决复杂问题的有力工具。记住：二分查找的本质是逐步缩小问题的规模，这种分治思想在算法设计中具有重要地位。

"虽然二分查找的基本思想相对简单，但细节可以令人难以捉摸..." ——ZZJ
```
