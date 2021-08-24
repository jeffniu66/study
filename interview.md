---
typora-root-url: ../study
typora-copy-images-to: ./interview_img
---

# 1. JAVA

## 1.1 字符串常量Java内部加载

```java
package com.lzd.interview;

public class StringPool58Demo {

    public static void main(String[] args) {

        String s1 = new StringBuilder("58").append("tongcheng").toString();
        System.out.println(s1);
        System.out.println(s1.intern());
        System.out.println(s1 == s1.intern());

        System.out.println();

        String s2 = new StringBuilder("ja").append("va").toString();
        System.out.println(s2);
        System.out.println(s2.intern());
        System.out.println(s2 == s2.intern());
    }
}
/**
输出:
58tongcheng
58tongcheng
true

java
java
false
*/
```

讲解:

<font color="red">1、String::intern()方法是一个本地方法，它的作用是如果字符串常量池中已经包含了一个等于此String对象的字符串，则返回代表池中这个字符串的String对象的引用；否则，会将此String对象包含的字符串添加到常量池中，并且返回此String对象的引用。这段代码在JDK1.6中，会返回两个false，而在JDK1.7中运行，会得到一个true和一个false。</font>

<font color="red">2、产生差异的原因是，在JDK1.6中，intern()方法会把首次遇到的字符串实例复制到永久代的字符串常量池中存储，返回的也是永久代里面这个字符串实例的引用，而由StringBuilder创建的字符串对象实例在Java堆上，所以必然不可能是同一个引用，结果将返回false。</font>

<font color="red">3、在JDK1.7中（以及部分其他虚拟机，例如JRockit）的intern()方法实现就不需要再拷贝字符串的实例到永久代了，既然字符串常量池已经移到Java堆中，那只需要在常量池里记录一下首次出现的实例引用即可，因此intern()返回的引用和由StringBuilder创建的那个字符串实例就是同一个。</font>

<font color="red">4、而对s2比较返回false，这是因为java这个字符串在执行StringBuilder.toString()之前就已经出现过了，字符串常量池中已经有它的引用，不符合intern()要求“首次遇到”的原则，“58tongcheng”这个字符串则是首次出现的，因此结果返回true。</font>

<font color="red">5、参考java.lang.System类中initializeSystemClass()，调用sun.misc.Version.init()初始化一些版本信息。</font>

## 1.2 TwoSum

### 1.2.1 暴力破解法

```java
package com.lzd.interview;

public class TwoSum {

    private static int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return null;
    }

    public static void main(String[] args) {
        int[] arr = twoSum(new int[]{2, 7, 11, 15}, 9);
        System.out.println(arr[0]);
        System.out.println(arr[1]);
    }
}
```

### 1.2.2 HashMap解法

```java
package com.lzd.interview;

import java.util.HashMap;
import java.util.Map;

public class TwoSum {

    private static int[] twoSum(int[] nums, int target) {

        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int num = target - nums[i];
            if (map.containsKey(num)) {
                return new int[]{map.get(num), i};
            }

            map.put(nums[i], i);
        }
        return null;
    }

    public static void main(String[] args) {
        int[] arr = twoSum(new int[]{2, 7, 11, 15}, 22);
        System.out.println(arr[0]);
        System.out.println(arr[1]);
    }
}
```

## 1.3 CAS

AtomicInteger为什么要用CAS而不是用synchronized?

synchronized太重了，杀鸡焉用牛刀

![image-20210824225859988](/interview_img/image-20210824225859988.png)

```java
package com.lzd.interview;

import java.util.concurrent.atomic.AtomicInteger;

public class CasDemo {

    public static void main(String[] args) {

        AtomicInteger atomicInteger = new AtomicInteger(5);

        System.out.println(atomicInteger.compareAndSet(5, 2020) + "\t current data: " + atomicInteger.get());

        System.out.println(atomicInteger.compareAndSet(5, 2021) + "\t current data: " + atomicInteger.get());
    }
}

// 输出
true	 current data: 2020
false	 current data: 2020
```

底层原理是怎样的？

1.自旋锁

2.Unsafe类

![image-20210824232618706](/interview_img/image-20210824232618706.png)

![image-20210824232901840](/interview_img/image-20210824232901840.png)

![image-20210824233219837](/interview_img/image-20210824233219837.png)

![image-20210824233525502](/interview_img/image-20210824233525502.png)

![image-20210824234828561](/interview_img/image-20210824234828561.png)

![image-20210825000027141](/interview_img/image-20210825000027141.png)





