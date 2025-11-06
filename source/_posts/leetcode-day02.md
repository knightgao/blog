---
title: 力扣闯关记录-day02
date: 2025-11-06
updated: 2025-11-06
---


## 题目

- 题目链接: [LeetCode 27. 移除元素](https://leetcode.cn/problems/remove-element/)
- 难度: 简单
- 标签: 数组, 双指针

## 题目描述（摘录/自述）

给定数组 `nums` 和值 `val`，就地移除所有数值等于 `val` 的元素，并返回移除后数组的新长度 `k`（前 `k` 个元素为移除后的结果，元素顺序可以改变或保持，题目不强制要求）。

## 思路

- **双指针（快慢指针）**: 使用 `fast` 遍历数组，`slow` 指向下一个应写入的位置。
- **覆盖保留**: 当 `nums[fast] != val` 时，将其写到 `nums[slow]`，然后 `slow += 1`；无论是否相等，`fast` 每次都前进。
- **返回值**: 遍历结束后，`slow` 即为新数组长度。

## 代码（Python）

```python
from typing import List


class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow, fast = 0, 0
        while fast < len(nums):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```

## 复杂度分析

- **时间复杂度**: `O(n)`
- **空间复杂度**: `O(1)`

## 边界与测试用例

```python
sol = Solution()

# 基本用例
nums = [3, 2, 2, 3]
k = sol.removeElement(nums, 3)
assert k == 2 and sorted(nums[:k]) == [2, 2]

# 不含目标值
nums = [0, 1, 2]
k = sol.removeElement(nums, 3)
assert k == 3 and nums[:k] == [0, 1, 2]

# 全部为目标值
nums = [2, 2, 2]
k = sol.removeElement(nums, 2)
assert k == 0

# 目标在中间/两端
nums = [4, 1, 4, 2, 3]
k = sol.removeElement(nums, 4)
assert k == 3 and sorted(nums[:k]) == [1, 2, 3]

# 空数组
nums = []
k = sol.removeElement(nums, 1)
assert k == 0
```

## 常见坑点

- 只需要返回长度 `k`，不需要截断数组或返回新数组。
- 覆盖赋值时务必先判断再写入，避免把目标值写回前缀。
- 若不要求保持相对顺序，可用“尾部交换”法减少写入次数。

## 变体与扩展

- 变体：尾部交换法。当 `nums[fast] == val` 时，用 `nums[fast] = nums[right-1]` 并 `right -= 1`，不移动 `fast`；当不等时 `fast += 1`。适合不需要保持顺序的场景。

## 小结

本题核心是双指针就地覆盖，关注返回长度 `k` 与写入边界即可。更多细节见题目页：[LeetCode 27. 移除元素](https://leetcode.cn/problems/remove-element/)。
