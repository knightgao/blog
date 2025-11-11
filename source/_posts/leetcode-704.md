---
title: 力扣闯关记录-704
date: 2025-11-06
updated: 2025-11-06
---


## 题目

- 题目链接: [LeetCode 704. 二分查找](https://leetcode.cn/problems/binary-search/)
- 难度: 简单
- 标签: 数组, 二分查找

## 题目描述（摘录/自述）

在升序数组 `nums` 中，查找目标值 `target` 的索引。如果目标值不存在，返回 `-1`。

## 思路

- **单调性**: 升序数组具备单调性，适合使用二分查找以在对数时间内定位。
- **区间定义**: 使用闭区间 `[left, right]`，循环条件为 `left <= right`。
- **收缩规则**:
  - 若 `nums[mid] < target` 则目标在右侧，`left = mid + 1`
  - 若 `nums[mid] > target` 则目标在左侧，`right = mid - 1`
  - 否则返回 `mid`
- **溢出规避**: 计算中点使用 `left + (right - left) // 2`

## 代码（Python）

```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                return mid
        return -1
```

### 代码变体：左闭右开区间 `[left, right)`

- 区间定义为半开区间：`left` 可取，`right` 不可取。
- 循环条件变为 `left < right`；当收缩右边界时，赋值为 `right = mid`（而非 `mid - 1`）。

```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)
        while left < right:
            mid = left + (right - left) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                return mid
        return -1
```

## 复杂度分析

- **时间复杂度**: `O(log n)`
- **空间复杂度**: `O(1)`

## 边界与测试用例

```python
sol = Solution()

# 基本用例
assert sol.search([1, 2, 3, 4, 5], 3) == 2

# 目标不存在
assert sol.search([1, 2, 4, 5], 3) == -1

# 单元素数组
assert sol.search([1], 1) == 0
assert sol.search([1], 2) == -1

# 重复元素（若出现，返回任意一个索引均可）
assert sol.search([1, 2, 2, 2, 3], 2) in {1, 2, 3}

# 空数组
assert sol.search([], 1) == -1
```

## 常见坑点

- 区间选择不一致导致死循环（如用 `[left, right)` 却写成 `left <= right`）。
- `mid` 计算写成 `mid = mid = ...` 或者使用 `(left + right) // 2` 在极端语言/范围下可能溢出（Python 无此忧虑，但推荐规范写法）。
- 更新 `left/right` 时忘记 `± 1`，导致无限循环。

## 变体与扩展

- 查找左/右边界（lower_bound / upper_bound）。
- 在旋转有序数组中查找目标。
- 在答案空间上二分（如最小可行值、最大最小值问题）。

## 小结

二分查找的关键在于：定义清晰的搜索区间、正确的循环不变式与收缩规则，并用用例覆盖边界场景。
