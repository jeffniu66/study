---
typora-root-url: ../study
---

# 1. 算法与数据结构

## 1.1 时间复杂度

<img src="/algo_images/image-20210124154452824.png" alt="image-20210124154452824"  />

## 1.2 二分查找法

最坏时间复杂度O(logn)

最优时间复杂度O(1)

平均时间复杂度O(logn)

空间复杂度O(1)

<font color="red">先决条件：有序数组</font>

<font color="red">注意数据类型是有范围的，采用L + (R - L) / 2更适合</font>

<font color="red">注意start = mid + 1和end = mid - 1，防止死循环</font>

<font color="red">数据库不可过大，因为数组需要全放内存中</font>

```python
def binary_search(arr, key):
    start = 0
    end = len(arr) - 1
    while start <= end:
        mid = (start + end) / 2
        if arr[mid] < key:
            start = mid + 1
        elif arr[mid] > key:
            end = mid - 1
        else:
            return mid
    return -1


print binary_search([1, 2, 3, 4, 5], 4)

# 输出：3
```

## 1.3 数组&链表

时间复杂度

![image-20210124170515129](/algo_images/image-20210124170515129.png)



# 2. 算法题

##  2.1 TwoSum(简单)

```
描述：
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

```python
from typing import List


def twoSum(nums: List[int], target: int) -> List[int]:
    nums_list = enumerate(nums)
    for idx, n in nums_list:
        if (target - n) in nums[idx + 1:]:
            return [idx, nums[idx + 1:].index(target - n) + idx + 1]
    return None


print(twoSum([2, 7, 11, 15], 9))
# 输出：[0, 1]
```





