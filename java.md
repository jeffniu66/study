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

# 注解

## 定义

![image-20210217182428993](/java_img/image-20210217182428993.png)

## 内置注解

![image-20210217183226503](/java_img/image-20210217183226503.png)

## 元注解

![image-20210217183920366](/java_img/image-20210217183920366.png)



## 自定义注解

![image-20210218073249740](/java_img/image-20210218073249740.png)

![image-20210218074219731](/java_img/image-20210218074219731.png)

# 反射

## 反射概述

![image-20210218074810235](/java_img/image-20210218074810235.png)

![image-20210218075210508](/java_img/image-20210218075210508.png)

## 获得反射对象

![image-20210218165702489](/java_img/image-20210218165702489.png)

![image-20210218165826656](/java_img/image-20210218165826656.png)

![image-20210218165925216](/java_img/image-20210218165925216.png)

![image-20210218171304248](/java_img/image-20210218171304248.png)

```java
package com.lzd.study.reflection;

public class Test2 {

    public static void main(String[] args) throws ClassNotFoundException {
        Class clz = Class.forName("com.lzd.study.reflection.User");
        System.out.println(clz);


        // 一个类在内存中只有一个Class对象
        // 一个类被加载后，类的整个结构都会被封装在Class对象中
        Class c2 = Class.forName("com.lzd.study.reflection.User");
        Class c3 = Class.forName("com.lzd.study.reflection.User");
        Class c4 = Class.forName("com.lzd.study.reflection.User");
        System.out.println(c2.hashCode());
        System.out.println(c3.hashCode());
        System.out.println(c4.hashCode());
    }
}

class User {

    private String name;
    private int id;
    private int age;

    public User() {
    }

    public User(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}
```

## 得到Class类的几种方式

![image-20210218171939688](/java_img/image-20210218171939688.png)

![image-20210218172329107](/java_img/image-20210218172329107.png)

![image-20210218172522250](/java_img/image-20210218172522250.png)

## 所有类型的Class对象

![image-20210218173703859](/java_img/image-20210218173703859.png)

## 类加载内存分析

![image-20210218175009527](/java_img/image-20210218175009527.png)

类的加载

![image-20210218175444319](/java_img/image-20210218175444319.png)

![image-20210218183905301](/java_img/image-20210218183905301.png)

## 分析类初始化

![image-20210219073916542](/java_img/image-20210219073916542.png)

## 类加载器

![image-20210219074823486](/java_img/image-20210219074823486.png)

![image-20210219075055620](/java_img/image-20210219075055620.png)

## 获取类的运行时结构

## 动态创建对象执行方法

## 获取泛型信息

![image-20210221093343663](/java_img/image-20210221093343663.png)

![image-20210221100511963](/java_img/image-20210221100511963.png)

## 获取注解信息

# Java8

## 简介

![image-20210221104802589](/java_img/image-20210221104802589.png)

![image-20210221104925548](/java_img/image-20210221104925548.png)

## Lambda表达式

![image-20210221112449713](/java_img/image-20210221112449713.png)

```java
package com.lzd.study.lambda;

import java.util.Comparator;
import java.util.TreeSet;

public class Test {

    public static void main(String[] args) {

        // 原来的匿名内部类
        Comparator<Integer> com = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1, o2);
            }
        };
        TreeSet<Integer> ts = new TreeSet<>(com);

        // Lambda表达式
        Comparator<Integer> com2 = (x, y) -> Integer.compare(x, y);
        TreeSet<Integer> ts2 = new TreeSet<>(com);
    }
}
```

## Lambda基础语法

```java
// 语法格式一：无参数，无返回值
        int num = 0; // jdk1.7之前，必须加final，1.8以后默认是final

        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello" + num);
            }
        };
        r.run();

        Runnable r1 = () -> System.out.println("Hello");
        r1.run();

        // 语法格式二：有一个参数，并且无返回值
        Consumer<String> con = (x) -> System.out.println(x);
        con.accept("World");

        // 语法格式三：有一个参数，小括号可以不写
        Consumer<String> con1 = x -> System.out.println(x);
        con1.accept("Lambda");

        // 语法格式四：有两个以上参数，有返回值，并且Lambda体中有多条语句
        Comparator<Integer> com = (x, y) -> {
            System.out.println("函数式接口");
            return Integer.compare(x, y);
        };

        // 语法格式五：有两个以上参数，有返回值，如果只有一条语句，大括号都可以省略
        Comparator<Integer> com1 = (x, y) -> Integer.compare(x, y);

        // 语法格式六：Lambda表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出数据类型，即"类型推断"
```

<font color=gree>Lambda表达式需要“函数式接口”支持，接口中只有一个抽象方法的接口，称为函数式接口。</font>

## Lambda练习

1.调用Collections.sort()方法，通过定制排序比较两个Employee（先按年龄比，年龄相同按姓名比），使用Lambda作为参数传递。

```java
package com.lzd.study.lambda;

import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class Test {

    public static void main(String[] args) {

        List<Employee> emps = Arrays.asList(
                new Employee(101, "张三", 10, 10000.11),
                new Employee(102, "李四", 30, 80000.11),
                new Employee(103, "王五", 20, 30000.11),
                new Employee(104, "赵六", 50, 20000.11)
        );

        Collections.sort(emps, (e1, e2) -> {
            if (e1.getAge() == e2.getAge()) {
                return e1.getName().compareTo(e2.getName());
            }
            return Integer.compare(e1.getAge(), e2.getAge());
        });

        for (Employee emp : emps) {
            System.out.println(emp);
        }
    }
}

class Employee {

    private int id;
    private String name;
    private int age;
    private double salary;

    public Employee() {
    }

    public Employee(int id, String name, int age, double salary) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", salary=" + salary +
                '}';
    }
}
```

2.声明一个函数式接口，操作字符串

```java
package com.lzd.study.lambda;

@FunctionalInterface
public interface MyFun {

    String getValue(String s);
}
```

```java
package com.lzd.study.lambda;

public class Test {

    public static void main(String[] args) {

        String str = strHandler("\t\t\t 大Java威武", s -> s.trim());
        System.out.println(str);

        str = strHandler("abcd", s -> s.toUpperCase());
        System.out.println(str);
    }

    public static String strHandler(String s, MyFun mf) {
        return mf.getValue(s);
    }
}
```

3.①声明一个带两个泛型的函数式接口，泛型类型为<T,R> T为参数，R为返回值

②接口中声明对应抽象方法

③在TestLambda类中声明方法，使用接口作为参数，计算两个long型参数的和

④再计算两个long型参数的乘积

 ```java
package com.lzd.study.lambda;

@FunctionalInterface
public interface MyFun<T, R> {

    R getValue(T t1, T t2);
}
 ```

```java
package com.lzd.study.lambda;

public class Test {

    public static void main(String[] args) {

        op(100L, 200L, (x, y) -> x * y);
    }

    public static void op(Long l1, Long l2, MyFun<Long, Long> mf) {
        System.out.println(mf.getValue(l1, l2));
    }
}
```

## 四大内置核心函数式接口

```java
// 消费型接口
@FunctionalInterface
public interface Consumer<T> {
	void accept(T t);
}

// 供给型接口
@FunctionalInterface
public interface Supplier<T> {
  T get();
}

// 函数型接口
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}

// 断言型接口
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

```java
package com.lzd.study.lambda;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;

public class Test {

    public static void main(String[] args) {

        happy(10000, (m) -> System.out.println("吃饭" + m + "元"));

        List<Integer> numList = getNumList(10, () -> (int) (Math.random() * 100));

        for (Integer integer : numList) {
            System.out.println(integer);
        }

        System.out.println(strHanlder("\t\t\t大java威武", s -> s.trim()));

        List<String> list = Arrays.asList("Hello", "java", "Lambda", "ok");
        List<String> strings = filterStr(list, s -> s.length() > 3);
        for (String s : strings) {
            System.out.println(s);
        }

    }

    public static List<String> filterStr(List<String> list, Predicate<String> pre) {
        List<String> strList = new ArrayList<>();

        for (String s : list) {
            if (pre.test(s)) {
                strList.add(s);
            }
        }

        return strList;
    }

    public static String strHanlder(String s, Function<String, String> f) {
        return f.apply(s);
    }

    public static List<Integer> getNumList(int num, Supplier<Integer> sup) {
        List<Integer> list = new ArrayList<>();

        for (int i = 0; i < num; i++) {
            list.add(sup.get());
        }
        return list;
    }

    public static void happy(double money, Consumer<Double> con) {
        con.accept(money);
    }
}

// 输出
吃饭10000.0元
51
49
92
71
24
32
48
76
46
27
大java威武
Hello
java
Lambda
```

## 方法引用与构造器引用

<font color=gree>***方法引用***</font>：若Lambda体中的内容有方法已经实现了，我们可以使用“方法引用”（可以理解为方法引用是Lambda表达式的另外一种表现形式）。

主要有三种语法格式：

<font color=gree>对象::实例方法名</font>

```java
package com.lzd.study.lambda;

import java.util.function.Consumer;
import java.util.function.Supplier;

public class Test2 {

    public static void main(String[] args) {

        Consumer<String> con = System.out::println;
        con.accept("abc");

        Employee emp = new Employee();
        Supplier<String> sup = emp::getName;
        String s = sup.get();
        System.out.println(s);
    }
}
```

<font color=gree>类::静态方法名</font>

```java
package com.lzd.study.lambda;

import java.util.Comparator;

public class Test2 {

    public static void main(String[] args) {

//        Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
        Comparator<Integer> com = Integer::compare;
    }
}
```

<font color=gree>类::实例方法名</font>

```java
package com.lzd.study.lambda;

import java.util.function.BiPredicate;

public class Test2 {

    public static void main(String[] args) {

//        BiPredicate<String, String> bp = (x, y) -> x.equals(y);
        BiPredicate<String, String> bp = String::equals;
        System.out.println(bp.test("a", "A"));
    }
}
```

<font color=red>注意：1.Lambda体中调用方法的参数列表与返回值类型，要与函数式接口中抽象方法的函数列表和返回值类型返回一致</font>

<font color=red>2.若Lambda参数列表中的第一参数是实例方法的调用者，而第二个参数是实例方法的参数时，可以使用ClassName::method</font>

<font color=gree>***构造器引用***</font>：

格式：ClassName::new

```java
package com.lzd.study.lambda;

import java.util.function.Function;
import java.util.function.Supplier;

public class Test2 {

    public static void main(String[] args) {

//        Supplier<Employee> sup = () -> new Employee();
        Supplier<Employee> sup = Employee::new;

//        Function<Integer, Employee> fun = (x) -> new Employee(x);
        Function<Integer, Employee> fun = Employee::new; // 调用的哪个构造方法取决于方Function方法传入的参数
        System.out.println(fun.apply(100));
    }
}
```

<font color=red>注意：1.需要调用的构造器的参数列表要与函数式接口中抽象方法的参数列表保持一致！</font>

<font color=gree>***数组引用***</font>：

```java
package com.lzd.study.lambda;

import java.util.function.Function;

public class Test2 {

    public static void main(String[] args) {

//        Function<Integer, String[]> fun = (x) -> new String[x];
        Function<Integer, String[]> fun = String[]::new;
        System.out.println(fun.apply(10).length);
    }
}
```

## 创建Stream

<font color=red>集合讲的是数据，流讲的是计算</font>

<font color=red>注意</font>

<font color=gree>1.Stream自己不会存储元素。</font>

<font color=gree>2.Stream不会改变源对象。相反，他们会返回一个持有结果的新Stream。</font>

<font color=gree>3.Stream操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。</font>

```java
package com.lzd.study.stream;

import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

/**
 * 一、Stream 的三个操作步骤
 * <p>
 * 1. 创建 Stream
 * <p>
 * 2. 中间操作
 * <p>
 * 3. 终止操作（终端操作）
 */
public class TestStreamAPI1 {


    // 创建Stream
    @Test
    public void test1() {

        // 1. 可以通过Collection系列集合提供的stream()或parallelStream()
        List<String> list = new ArrayList<>();
        Stream<String> stream = list.stream();

        // 2. 通过Arrays中的静态方法stream()获取数组流
        Integer[] arr = new Integer[10];
        Stream<Integer> stream1 = Arrays.stream(arr);

        // 3. 通过Stream类中的静态方法of()
        Stream<String> stream2 = Stream.of("aa", "bb", "cc");

        // 4. 创建无限流
        // 迭代
        Stream<Integer> stream3 = Stream.iterate(0, (x) -> x + 2);
        stream3.limit(10).forEach(System.out::println);

        // 生成
        Stream.generate(Math::random).limit(5).forEach(System.out::println);
    }
}
```





