---
typora-root-url: ../study
typora-copy-images-to: ./java_img
---

# 泛型

## 定义

<font color="gree">Java 泛型（generics）是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。</font>

如果不指定泛型，编译期不报错，运行时报错

```java
package com.lzd.study.generic;

import java.util.ArrayList;

public class Test {

    public static void main(String[] args) {

        ArrayList list = new ArrayList();
        list.add("hello");
        list.add(100);
        list.add(true);

        for (int i = 0; i < list.size(); i++) {

            Object o = list.get(i);
            String str = (String) o;
            System.out.println(str);
        }
    }
}

hello
Exception in thread "main" java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String
	at com.lzd.study.generic.Test.main(Test.java:17)
```

泛型的好处

1.类型安全

2.消除了强制类型的转换

## 泛型类

```java
package com.lzd.study.generic;

public class Generic<T> {

    // 是由外部类使用的时候来指定的
    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey() {
        return key;
    }

    public void setKey(T key) {
        this.key = key;
    }

    @Override
    public String toString() {
        return "Generic{" +
                "key=" + key +
                '}';
    }
}
```

使用

```java
package com.lzd.study.generic;

public class Test {

    public static void main(String[] args) {

        // 泛型类在创建对象的时候，来指定操作的具体数据类型
        Generic<String> str = new Generic<>("a");
        String key = str.getKey();
        System.out.println(key);

        System.out.println("----------------------");
        Generic<Integer> intGeneric = new Generic<>(100);
        Integer key1 = intGeneric.getKey();
        System.out.println(key1);


        System.out.println("----------------------");
        // 泛型类在创建对象的时候，没有指定类型，将按照Object类型来操作
        Generic generic = new Generic("abc");
        Object key2 = generic.getKey();
        System.out.println(key2);

        System.out.println("----------------------");
        System.out.println(str.getClass());
        System.out.println(intGeneric.getClass());
        System.out.println(str.getClass() == intGeneric.getClass());
        System.out.println(generic.getClass());
    }
}
```

注意事项

<img src="/java_img/image-20210216203311437.png" alt="image-20210216203311437"  />

## 泛型类派生子类



![image-20210216205123696](/java_img/image-20210216205123696.png)

<font color="gree">第一种情况，子类和父类泛型类型一致</font>

```java
package com.lzd.study.generic;

public class Parent<E> {

    private E value;

    public E getValue() {
        return value;
    }

    public void setValue(E value) {
        this.value = value;
    }
}
```

```java
package com.lzd.study.generic;

public class ChildFirst<T> extends Parent<T> {

    @Override
    public T getValue() {
        return super.getValue();
    }
}
```

<font color="gree">第二种情况，如果子类不是泛型类，那么父类要明确数据类型</font>

```java
package com.lzd.study.generic;

public class ChildSecond extends Parent<Integer> {

    @Override
    public Integer getValue() {
        return super.getValue();
    }

    @Override
    public void setValue(Integer value) {
        super.setValue(value);
    }
}
```

```java
package com.lzd.study.generic;

public class Test {

    public static void main(String[] args) {

        ChildFirst<String> childFirst = new ChildFirst<>();
        childFirst.setValue("abc");
        String value = childFirst.getValue();
        System.out.println(value);

        System.out.println("---------------------");
        
        ChildSecond childSecond = new ChildSecond();
        childSecond.setValue(100);
        Integer secondValue = childSecond.getValue();
        System.out.println(secondValue);
    }
}
```

## 泛型接口

![image-20210216212414210](/java_img/image-20210216212414210.png)

<font color=gree>第一种情况</font>

```java
package com.lzd.study.generic;

public interface Generator<T> {

    T getKey();
}
```

```java
package com.lzd.study.generic;

public class Apple implements Generator<String> {

    @Override
    public String getKey() {
        return "hello generic";
    }
}
```

<font color=gree>第二种情况</font>

```java
package com.lzd.study.generic;

/**
 * 泛型接口的实现类，是一个泛型类，那么要保证实现接口的泛型类泛型标识包含泛型接口的泛型标识
 * @param <T>
 * @param <E>
 */
public class Pear<T, E> implements Generator<T> {

    private T key;

    private E value;

    public Pear(T key, E value) {
        this.key = key;
        this.value = value;
    }

    @Override
    public T getKey() {
        return key;
    }

    public E getValue() {
        return value;
    }
}
```

```java
package com.lzd.study.generic;

import javafx.util.Pair;

public class Test {

    public static void main(String[] args) {

        Apple apple = new Apple();
        String key = apple.getKey();
        System.out.println(key);

        System.out.println("-------------------------");
        Pair<String, Integer> pair = new Pair<>("tom", 100);
        System.out.println(pair.getKey());
        System.out.println(pair.getValue());
    }
}
```

## 泛型方法

![image-20210217095611663](/java_img/image-20210217095611663.png)

![image-20210217095808338](/java_img/image-20210217095808338.png)

```java
package com.lzd.study.generic;

import java.util.ArrayList;

public class ProductGetter<T> {

    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }

    public <T> T getProduct(ArrayList<T> list) {
        return null;
    }

    /**
     * 静态的泛型方法，采用多个泛型
     *
     * @param t
     * @param e
     * @param k
     * @param <T>
     * @param <E>
     * @param <K>
     */
    public static <T, E, K> void printType(T t, E e, K k) {
        System.out.println(t + "\t" + t.getClass().getSimpleName());
        System.out.println(e + "\t" + e.getClass().getSimpleName());
        System.out.println(k + "\t" + k.getClass().getSimpleName());
    }
}
```

```java
package com.lzd.study.generic;

public class Test {

    public static void main(String[] args) {

        ProductGetter.printType(100, "java", true);
    }
}
```

泛型方法与可变参数

![image-20210217103108694](/java_img/image-20210217103108694.png)

![image-20210217103212356](/java_img/image-20210217103212356.png)

