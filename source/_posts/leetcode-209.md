---
title: 力扣闯关记录-209
date: 2025-11-11
updated: 2025-11-11
---


## 题目

- 题目链接: [LeetCode 209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)
- 难度: 中等
- 标签: 数组, 二分查找, 前缀和, 滑动窗口

## 题目描述（摘录/自述）

给定一个含有 `n` 个正整数的数组和一个正整数 `target`。

找出该数组中满足其总和大于等于 `target` 的长度最小的子数组 `[numsl, numsl+1, ..., numsr-1, numsr]`，并返回其长度。如果不存在符合条件的子数组，返回 `0`。

**示例 1：**
```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**示例 2：**
```
输入：target = 4, nums = [1,4,4]
输出：1
```

**示例 3：**
```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```



## 思路

这道题目要求找到长度最小的连续子数组，使其和大于等于目标值。由于数组中的元素都是正整数，可以使用**滑动窗口**来解决这个问题。

### 滑动窗口思路
1. **窗口定义**：使用左右指针 `left` 和 `right` 定义一个滑动窗口
2. **窗口扩展**：右指针向右移动，将新元素加入窗口和
3. **窗口收缩**：当窗口和大于等于目标值时，记录当前窗口长度，然后左指针向右移动缩小窗口
4. **优化处理**：在开始时先检查总和是否小于目标值，可以直接返回0

### 为什么能用滑动窗口？
- 数组中所有元素都是**正整数**，所以窗口扩大时和增加，窗口缩小时和减少
- 如果当前窗口的和 >= target，那么任何包含这个窗口的更大窗口都不可能是最优解
- 这种单调性使得我们可以用线性时间解决

### 算法步骤
1. 预处理：计算数组总和，提前判断不存在的情况
2. 初始化左右指针和窗口和
3. 遍历数组，扩展右边界
4. 当满足条件时，收缩左边界，更新最小长度



## 代码（Python）

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        n = len(nums)
        left = 0
        total = 0
        min_len = n + 1

        total_sum = sum(nums)
        if total_sum < target:
            return 0
        elif total_sum == target:
            return n

        for right, num in enumerate(nums):
            total += num

            while total >= target:
                min_len = min(min_len, right - left + 1)
                if min_len == 1:
                    return 1 
                total -= nums[left]
                left += 1

        return 0 if min_len == n + 1 else min_len

```

## 复杂度分析

- **时间复杂度**: O(n)
  - 虽然有嵌套的while循环，但每个元素最多被访问两次（一次被右指针加入，一次被左指针移除）
  - 总体来说是线性时间复杂度

- **空间复杂度**: O(1)
  - 只使用了常数个额外变量（left, right, total, min_len）
  - 没有使用额外的数据结构 



## 常见坑点

1. **边界情况处理**
   - 数组总和小于target，应返回0
   - 数组总和等于target，应返回数组长度
   - 目标target为1且数组中有1，应立即返回1

2. **初始化问题**
   - min_len应初始化为一个不可能的大数（n+1）
   - 不能初始化为0，会导致无法正确更新结果

3. **循环条件**
   - while循环条件是 `total >= target` 而不是 `total > target`
   - 因为题目要求是"大于等于"

4. **左指针移动**
   - 每次成功找到符合条件的子数组后，都要移动左指针
   - 不能忘记在while循环中递增left

5. **返回值判断**
   - 需要检查是否找到符合条件的子数组（min_len是否被更新）
   - 如果min_len仍为初始值，说明没找到，返回0


## 小结

本题是一道典型的滑动窗口应用题。关键在于理解**为什么能用滑动窗口**：

1. **正数性质**：数组元素都是正数，保证了窗口和的单调性
2. **最优子结构**：当窗口满足条件时，缩小窗口可能找到更优解
3. **线性时间**：每个元素被访问常数次，实现O(n)时间复杂度

滑动窗口模板：
```
left = 0, total = 0, result = INF
for right in range(n):
    total += nums[right]
    while condition_met:
        result = min(result, right - left + 1)
        total -= nums[left]
        left += 1
```

这类题目还有很多变种，比如：
- 找长度最大的子数组（条件相反）
- 元素可以为负数时需要其他方法
- 多条件约束的子数组问题

掌握滑动窗口思想对于解决数组区间类问题非常有帮助！


