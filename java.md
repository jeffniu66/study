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

```java
/**
     * 泛型可变参数定义
     *
     * @param e
     * @param <E>
     */
    public static <E> void print(E... e) {
        for (int i = 0; i < e.length; i++) {
            System.out.println(e[i]);
        }
    }
```

```java
package com.lzd.study.generic;

public class Test {

    public static void main(String[] args) {

        ProductGetter.print(1, 2, 3, 4, 5);
        ProductGetter.print("a", "b", "c", "d", "e");
    }
}
```

![image-20210217104212869](/java_img/image-20210217104212869.png)

## 类型通配符

![image-20210217104452784](/java_img/image-20210217104452784.png)

```java
package com.lzd.study.generic;

public class Box<E> {

    private E first;

    public E getFirst() {
        return first;
    }

    public void setFirst(E first) {
        this.first = first;
    }
}
```

```java
package com.lzd.study.generic;

public class Test {

    public static void main(String[] args) {

        Box<Number> box1 = new Box<>();
        box1.setFirst(100);
        showBox(box1);

        Box<Integer> box2 = new Box<>();
        box2.setFirst(200);
        showBox(box2);
    }


    public static void showBox(Box<?> box) {
        Object first = box.getFirst();
        System.out.println(first);
    }

//    public static void showBox(Box<Integer> box) {
//        Number first = box.getFirst();
//        System.out.println(first);
//    }
}
```

## 类型通配符上限

![image-20210217111350886](/java_img/image-20210217111350886.png)

```java 
package com.lzd.study.generic;

public class Test {

    public static void main(String[] args) {

        Box<Number> box1 = new Box<>();
        box1.setFirst(100);
        showBox(box1);

        Box<Integer> box2 = new Box<>();
        box2.setFirst(200);
        showBox(box2);
    }


    public static void showBox(Box<? extends Number> box) {
        Number first = box.getFirst();
        System.out.println(first);
    }

//    public static void showBox(Box<Integer> box) {
//        Number first = box.getFirst();
//        System.out.println(first);
//    }
}
```

```java
package com.lzd.study.generic;

import java.util.ArrayList;

public class Test {

    public static void main(String[] args) {

        ArrayList<Animal> animals = new ArrayList<>();
        ArrayList<Cat> cats = new ArrayList<>();
        ArrayList<MiniCat> miniCats = new ArrayList<>();

//        showAnimal(animals);
        showAnimal(cats);
        showAnimal(miniCats);
    }

    /**
     * 泛型上限通配符，传递的集合类型，只能是Cat或Cat的子类类型
     *
     * @param list
     */
    public static void showAnimal(ArrayList<? extends Cat> list) {
        // 采用上限是不能添加元素的
//        list.add(new Animal());
//        list.add(new Cat());
//        list.add(new MiniCat());
        for (int i = 0; i < list.size(); i++) {
            Cat cat = list.get(i);
            System.out.println(cat);
        }
    }
}
```

## 类型通配符下限-1

![image-20210217142835891](/java_img/image-20210217142835891.png)

```java
package com.lzd.study.generic;

import java.util.ArrayList;
import java.util.List;

public class Test {

    public static void main(String[] args) {

        ArrayList<Animal> animals = new ArrayList<>();
        ArrayList<Cat> cats = new ArrayList<>();
        ArrayList<MiniCat> miniCats = new ArrayList<>();

        showAnimal(animals);
        showAnimal(cats);
//        showAnimal(miniCats);
    }

    /**
     * 类型通配符下限，要求集合只能是Cat或Cat的父类
     *
     * @param list
     */
    public static void showAnimal(List<? super Cat> list) {
//        list.add(new Animal());
        list.add(new Cat());
        list.add(new MiniCat());
        for (Object o : list) {
            System.out.println(o);
        }
    }
}
```

## 类型通配符下限-2

```java
package com.lzd.study.generic;

import java.util.Comparator;
import java.util.TreeSet;

public class Test {

    public static void main(String[] args) {

        TreeSet<Cat> treeSet = new TreeSet<>(new Comparator1());
        treeSet.add(new Cat("jerry", 20));
        treeSet.add(new Cat("amy", 22));
        treeSet.add(new Cat("frank", 35));
        treeSet.add(new Cat("jim", 15));
        for (Cat cat : treeSet) {
            System.out.println(cat);
        }
    }

}

class Comparator1 implements Comparator<Animal> {

    @Override
    public int compare(Animal o1, Animal o2) {
        return o1.name.compareTo(o2.name);
    }
}

class Comparator2 implements Comparator<Cat> {

    @Override
    public int compare(Cat o1, Cat o2) {
        return o1.age - o2.age;
    }
}

class Comparator3 implements Comparator<MiniCat> {

    @Override
    public int compare(MiniCat o1, MiniCat o2) {
        return o1.level - o2.level;
    }
}
```

## 类型擦除

![image-20210217151430079](/java_img/image-20210217151430079.png)

### 无限制类型擦除

![image-20210217152004393](/java_img/image-20210217152004393.png)

```java
package com.lzd.study.generic;

public class Erasure<T> {

    private T key;

    public T getKey() {
        return key;
    }

    public void setKey(T key) {
        this.key = key;
    }
}
```

```java
package com.lzd.study.generic;

import java.lang.reflect.Field;

public class Test {

    public static void main(String[] args) {

        Erasure<Integer> erasure = new Erasure<>();
        // 利用反射，获取Erasure类的字节码文件的Class类对象
        Class<? extends Erasure> clz = erasure.getClass();
        Field[] fields = clz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println(field.getName() + ":" + field.getType().getSimpleName());
        }
    }

}

// 输出
key:Object
```

### 有限制类型擦除

![image-20210217153539631](/java_img/image-20210217153539631.png)

```java
package com.lzd.study.generic;

public class Erasure<T extends Number> {

    private T key;

    public T getKey() {
        return key;
    }

    public void setKey(T key) {
        this.key = key;
    }
}
```

```java
package com.lzd.study.generic;

import java.lang.reflect.Field;

public class Test {

    public static void main(String[] args) {

        Erasure<Integer> erasure = new Erasure<>();
        // 利用反射，获取Erasure类的字节码文件的Class类对象
        Class<? extends Erasure> clz = erasure.getClass();
        Field[] fields = clz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println(field.getName() + ":" + field.getType().getSimpleName());
        }
    }

}

// 输出
key:Number
```

### 擦除方法中类型定义的参数

![image-20210217154511367](/java_img/image-20210217154511367.png)

```java
package com.lzd.study.generic;

import java.util.List;

public class Erasure<T extends Number> {

    private T key;

    public T getKey() {
        return key;
    }

    public void setKey(T key) {
        this.key = key;
    }

    public <T extends List> T show(T t) {
        return t;
    }
}
```

```java
package com.lzd.study.generic;

import java.lang.reflect.Method;

public class Test {

    public static void main(String[] args) {

        Erasure<Integer> erasure = new Erasure<>();
        // 利用反射，获取Erasure类的字节码文件的Class类对象
        Class<? extends Erasure> clz = erasure.getClass();
        Method[] methods = clz.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println(method.getName() + ":" + method.getReturnType().getSimpleName());
        }
    }

}

// 输出
getKey:Number
setKey:void
show:List
```

### 桥接方法

![image-20210217155546938](/java_img/image-20210217155546938.png)

```java
package com.lzd.study.generic;

public interface Info<T> {

    T info(T t);
}
```

```java
package com.lzd.study.generic;

public class InfoImpl implements Info<Integer> {

    @Override
    public Integer info(Integer value) {
        return value;
    }
}
```

```java
package com.lzd.study.generic;

import java.lang.reflect.Method;

public class Test {

    public static void main(String[] args) {

        Class<InfoImpl> infoClass = InfoImpl.class;
        Method[] methods = infoClass.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println(method.getName() + ":" + method.getReturnType().getSimpleName());
        }
    }
}

// 输出
info:Integer
info:Object
```

## 泛型与数组

![image-20210217161823082](/java_img/image-20210217161823082.png)

<font color=gree>第一种情况</font>

```java
package com.lzd.study.generic;

import java.util.ArrayList;

public class Test {

    public static void main(String[] args) {

//        ArrayList[] arr = new ArrayList[5];
//        ArrayList<String>[] listArr = arr;
//        ArrayList<String>[] listArr = new ArrayList<>[5]; // 报错
        ArrayList<String>[] listArr = new ArrayList[5];

        ArrayList<String> strList = new ArrayList<>();
        strList.add("abc");
        listArr[0] = strList;

//        arr[0] = intList;
        String s = listArr[0].get(0);
        System.out.println(s);
    }
}
```

<font color=gree>第二种情况</font>

```java
package com.lzd.study.generic;

import java.lang.reflect.Array;

public class Fruit<T> {

    //    private T[] array = new T[3];
    private T[] array;

    public Fruit(Class<T> clz, int len) {
        // 通过Array.newInstance()创建泛型数组
        array = (T[]) Array.newInstance(clz, len);
    }

    /**
     * 填充数组
     *
     * @param index
     * @param t
     */
    public void put(int index, T t) {
        array[index] = t;
    }

    public T get(int index) {
        return array[index];
    }

    public T[] getArray() {
        return array;
    }
}
```

```java
package com.lzd.study.generic;

import java.util.Arrays;

public class Test {

    public static void main(String[] args) {

        Fruit<String> fruit = new Fruit<>(String.class, 3);
        fruit.put(0, "苹果");
        fruit.put(1, "西瓜");
        fruit.put(2, "香蕉");

        System.out.println(Arrays.toString(fruit.getArray()));
    }
}
```

## 泛型和反射

![image-20210217170222871](/java_img/image-20210217170222871.png)

```java
package com.lzd.study.generic;

import java.lang.reflect.Constructor;

public class Test {

    public static void main(String[] args) throws Exception {

//        Class<Person> personClass = Person.class;
//        Constructor<Person> constructor = personClass.getConstructor(personClass);
//        Person person = constructor.newInstance();
        Class personClass = Person.class;
        Constructor constructor = personClass.getConstructor();
        Object o = constructor.newInstance();
    }
}
```

# 反射





