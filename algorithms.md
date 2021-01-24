---
typora-root-url: ../study
---

# 1. 初级

时间复杂度

![image-20210124154452824](/algo_images/image-20210124154452824.png)

## 1.1 TwoSum

```
描述：
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

```java
private static int[] twoSum(int[] nums, int target) {
        int[] array = new int[2];
        for (int i = 0; i < nums.length; i++) {
            if (i == 0) {
                if (nums[0] + nums[1] == target) {
                    array[0] = 0;
                    array[1] = 1;
                }
            } else {
                if (nums[i - 1] + nums[i] == target) {
                    array[0] = i - 1;
                    array[1] = i;
                }
            }
        }
        return array;
}
```





# 2. 中级



# 3. 高级



