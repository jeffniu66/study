---
typora-root-url: ../study
typora-copy-images-to: ./algo_images
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

## 1.4 线性结构与非线性结构

线性结构：数组、栈、队列、链表

非线性结构：二维数组、多维数组、广义表、树结构、图结构

## 1.5 稀疏数组

当一个数组中大部分元素为0，或者为同一个值的数组时，可以使用稀疏数组来保存该数组。

稀疏数组的处理方式是：

1）记录数组<font color=red>一共有几行几列，有多少个不同</font>的值

2）把具有不同值的元素的行列及值记录在一个小规模的数组中，从而<font color=red>缩小程序</font>的规模

![image-20210627112655453](/algo_images/image-20210627112655453.png)

### 1.5.1 二维数组转稀疏数组的思路

1、遍历原始的二维数组，得到有效数据的个数sum

2、根据sum就可以创建稀疏数组sparseArr 它的大小int[sum + 1] [3]

3、将二维数组的有效数据存入到稀疏数组中

### 1.5.2 稀疏数组转原始二维数组的思路

1、先读取稀疏数组的第一行，根据第一行的数据创建原始的二维数组

2、在读取稀疏数组后几行的数据，并赋给原始的二维数组即可

### 1.5.3 稀疏数组的代码实现

```java
package com.lzd.algorithms;

public class SparseArray {

    public static void main(String[] args) {

        // 创建一个原始的二维数组 11 * 11
        // 表示没有棋子，1表示黑子 2表示蓝子
        int chessArr1[][] = new int[11][11];
        chessArr1[1][2] = 1;
        chessArr1[2][3] = 2;
        chessArr1[4][5] = 1;
        // 输出原始的二维数组
        System.out.println("原始的二维数组~~");
        for (int[] row : chessArr1) {
            for (int data : row) {
                System.out.printf("%d\t", data);
            }
            System.out.println();
        }

        // 二维数组转成稀疏数组
        // 1.先遍历二维数组 得到非0数据的个数
        int sum = 0;
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if (chessArr1[i][j] != 0) {
                    sum++;
                }
            }
        }

        // 2.创建对应的稀疏数组
        int sparseArr[][] = new int[sum + 1][3];
        // 3.给稀疏矩阵赋值
        sparseArr[0][0] = 11;
        sparseArr[0][1] = 11;
        sparseArr[0][2] = sum;
        // 4.遍历二维数组，将非0的值存放到sparseArr中
        int count = 0; // 用于记录是第几个非0数据
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if (chessArr1[i][j] != 0) {
                    count++; // 为什么不用i，因为i会遍历每一行，但不一定每一行都有值
                    sparseArr[count][0] = i;
                    sparseArr[count][1] = j;
                    sparseArr[count][2] = chessArr1[i][j];
                }
            }
        }

        // 输出稀疏数组的形式
        System.out.println();
        System.out.println("得到稀疏数组为~~~");
        for (int[] aSparseArr : sparseArr) {
            System.out.printf("%d\t%d\t%d\t\n", aSparseArr[0], aSparseArr[1], aSparseArr[2]);
        }
        System.out.println();

        // 将稀疏数组恢复成原始的二维数组
        int chessArr2[][] = new int[sparseArr[0][0]][sparseArr[0][1]];

        for (int i = 1; i < sparseArr.length; i++) {
            chessArr2[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2];
        }

        System.out.println();
        System.out.println("恢复后的二维数组");

        for (int[] row : chessArr2) {
            for (int data : row) {
                System.out.printf("%d\t", data);
            }
            System.out.println();
        }
    }
}
```

## 1.6 队列

队列是一个有序列表，可以用<font color=red>数组</font>或是<font color=red>链表</font>来实现

### 1.6.1 数组模拟队列代码实现

![image-20210627213753440](/algo_images/image-20210627213753440.png)

```java
package com.lzd.algorithms;

public class ArrayQueue {

    public static void main(String[] args) {
    }
}

// 使用数组模拟队列-编写一个ArrayQueue类
class Queue {
    private int maxSize; // 表示数组的最大容量
    private int front; // 队列头
    private int rear; // 队列尾
    private int[] arr; // 该数据用于存放数据，模拟队列

    // 创建队列的构造器
    public Queue(int arrMaxSize) {
        maxSize = arrMaxSize;
        arr = new int[maxSize];
        front = -1;
        rear = -1;
    }

    // 判断队列是否满
    public boolean isFull() {
        return rear == maxSize - 1;
    }

    // 判断队列是否为空
    public boolean isEmpty() {
        return rear == front;
    }

    // 添加数据到队列
    public void addQueue(int n) {
        if (isFull()) {
            System.out.println("队列满，不能加入数据~");
            return;
        }
        rear++;
        arr[rear] = n;
    }

    public int getQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列空，不能取数据");
        }
        front++;
        return arr[front];
    }

    // 显示队列的所有数据
    public void showQueue() {
        if (isEmpty()) {
            System.out.println("队列为空，没有数据");
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.printf("arr[%d]=[%d]\n", i, arr[i]);
        }
    }

    // 显示队列的头数据，注意不是取出数据
    public int headQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空，没有数据~~");
        }
        return arr[front + 1];
    }
}
```

<font color=red>问题分析并优化</font>

1）目前数组使用一次就不能用，没有达到复用的效果

2）将这个数组使用算法，改进成一个<font color=red>环形的队列</font> 取模：%



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















