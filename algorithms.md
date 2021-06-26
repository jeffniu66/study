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

##  2.1 数组

###  2.1.1 TwoSum（第1题-简单）

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

### 2.1.2 最大子序和（第53题-简单）

```java
描述：
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

#### 暴力法

```java
package com.lzd.algorithms;

public class MaxSubArray {

    public static void main(String[] args) {

        int[] nums = new int[]{-2, 1, -3, 4, -1, 2, 1, -5, 4};

        System.out.println(maxSubArray(nums));
    }

    private static int maxSubArray(int[] nums) {

        int max = nums[0];

        for (int i = 0; i < nums.length; i++) {

            int sum = 0;

            for (int j = i; j < nums.length; j++) {

                sum += nums[j];

                max = Math.max(max, sum);
            }

        }

        return max;
    }
}
```

#### 分治法

```java

```

#### 动态规划

```java

```

### 2.1.3 买卖股票的最佳时机 （第121题-简单）

```java
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。
  
示例 1：
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
  
示例 2：
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

<font color=red>动态规划解法</font>

```java
package com.lzd.algorithms;

public class Stock {

    public static void main(String[] args) {

        int[] prices = new int[]{7, 1, 5, 3, 6, 4};
        System.out.println(maxProfit(prices));
    }

    private static int maxProfit(int[] prices) {
        int max = 0;
        int minPrice = prices[0];

        for (int i = 1; i < prices.length; i++) {
            minPrice = Math.min(prices[i], minPrice);
            max = Math.max(max, prices[i] - minPrice);
        }

        return max;
    }
}
```

### 2.1.4 多数元素（第169题-简单）

```java
给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例 1：
输入：[3,2,3]
输出：3
示例 2：

输入：[2,2,1,1,1,2,2]
输出：2
```

```java
package com.lzd.algorithms;

import java.util.Arrays;

public class Algo169 {

    public static void main(String[] args) {
        int[] nums = new int[]{2, 2, 1, 1, 1, 2, 2};
        System.out.println(majorityElement(nums));
    }

    private static int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length - 1];
    }
}
```

## 2.2 链表

### 2.2.1 合并两个有序链表（第21题-简单）

```java
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
  
示例 1：
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
  
示例 2：
输入：l1 = [], l2 = []
输出：[]
  
示例 3：
输入：l1 = [], l2 = [0]
输出：[0]
```















