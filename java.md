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

## Stream-筛选与切片

![image-20210414235232727](/java_img/image-20210414235232727.png)

```java
package com.lzd.study.stream;

import org.junit.Test;

import java.util.Arrays;
import java.util.Iterator;
import java.util.List;
import java.util.stream.Stream;

public class TestStreamAPI2 {

    List<Employee> list = Arrays.asList(
            new Employee(1, "张三", 18, 999),
            new Employee(2, "李四", 58, 555),
            new Employee(3, "王五", 68, 666),
            new Employee(3, "王五", 68, 666),
            new Employee(3, "王五", 68, 666)
    );

    /**
     * 筛选与切片
     * filter——接收Lambda，从流中排除元素。
     * limit——截断流，使其元素不超过给定数量。
     * skip(n)——跳过元素，返回一个扔掉了前n个元素的流。若流中元素不足n个，则返回一个空流。与limit(n)互补。
     * distinct——筛选，通过流所生成元素的hashCode()和equals()去除重复元素。
     */
    @Test
    public void test4() {
        list.stream().filter((e) -> {
            System.out.println("短路");
            return e.getSalary() > 555;
        }).skip(1).distinct().forEach(System.out::println);
    }

    @Test
    public void test3() {
        list.stream().filter((e) -> {
            System.out.println("短路");
            return e.getSalary() > 555;
        }).limit(2).forEach(System.out::println);
    }

    // 内部迭代：迭代操作由Stream API完成
    @Test
    public void test1() {
        // 中间操作：不会执行任何操作
        Stream<Employee> stream = list.stream().filter((e) -> {
            System.out.println("Stream API 的中间操作"); // 如果没有终止操作 即第28行 这里根本不会打印
            return e.getAge() > 35;
        });
        // 终止操作：一次性执行全部内容，即"惰性求值"
        stream.forEach(System.out::println);
    }

    // 外部迭代：
    @Test
    public void test2() {
        Iterator<Employee> it = list.iterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }
    }
}
```

## Stream-映射

```java
    /**
     * 映射
     * map-接收Lambda，将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新元素。
     * flatMap-接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连成一个流
     */
    @Test
    public void test5() {
        List<String> list2 = Arrays.asList("aaa", "bbb", "ccc", "ddd");
        list2.stream().map((x) -> x.toUpperCase()).forEach(System.out::println);

        System.out.println("----------------------------------");

        list.stream().map(Employee::getName).forEach(System.out::println);

        System.out.println("----------------------------------");

        /*
        Stream<Stream<Character>> stream = list2.stream().map(TestStreamAPI2::filterCharacter); // {{a, a, a}, {b, b, b}}
        stream.forEach((sm) -> {
            sm.forEach(System.out::println);
        });
        */

        System.out.println("----------------------------------");

        Stream<Character> sm = list2.stream().flatMap(TestStreamAPI2::filterCharacter); // {a, a, a, b, b, b}

        sm.forEach(System.out::println);

    }
```

## Stream-排序

```java
		List<Employee> list = Arrays.asList(
            new Employee(1, "张三", 18, 999),
            new Employee(2, "李四", 58, 555),
            new Employee(3, "王五", 68, 666),
            new Employee(3, "王五", 68, 666),
            new Employee(3, "王五", 68, 666)
    );    

		/**
     * 排序
     * sort()——自然排序（Comparable）
     * sorted(Comparator com)——订制排序（Comparator）
     */
    @Test
    public void test6() {
        List<String> list2 = Arrays.asList("ccc", "aaa", "bbb", "ddd");

        list2.stream().sorted().forEach(System.out::println);

        System.out.println("----------------------------------");

        list.stream().sorted((e1, e2) -> {
            if (e1.getAge().equals(e2.getAge())) {
                return e1.getName().compareTo(e2.getName());
            }
            return e2.getAge().compareTo(e1.getAge());
        }).forEach(System.out::println);
    }
```

## Stream-查找与匹配

```java
package com.lzd.study.stream;

import org.junit.Test;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

/**
 * 终止操作
 */
public class TestStreamAPI3 {

    List<Employee> empList = Arrays.asList(
            new Employee(1, "张三", 18, 999, Employee.Status.FREE),
            new Employee(2, "李四", 58, 555, Employee.Status.BUSY),
            new Employee(3, "王五", 68, 666, Employee.Status.VOCATION),
            new Employee(3, "王五", 68, 666, Employee.Status.FREE),
            new Employee(3, "王五", 68, 666, Employee.Status.BUSY)
    );

    /**
     * 查找与匹配
     * allMatch——检查是否匹配所有元素
     * anyMatch——检查是否匹配至少一个元素
     * noneMatch——检查是否没有匹配所有元素
     * findFirst——返回第一个元素
     * findAny——返回当前流中的任意元素
     */
    @Test
    public void test1() {
        boolean b1 = empList.stream().allMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
        System.out.println(b1);

        boolean b2 = empList.stream().anyMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
        System.out.println(b2);

        boolean b3 = empList.stream().noneMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
        System.out.println(b3);

        Optional<Employee> op = empList.stream().sorted((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())).findFirst();
        System.out.println(op.get());

        Optional<Employee> op1 = empList.parallelStream().filter((e) -> e.getStatus().equals(Employee.Status.FREE)).findAny();
        System.out.println(op1.get());
    }

    /**
     * count——返回流中元素的总个数
     * max——返回流中最大值
     * min——返回流中最小值
     */
    @Test
    public void test2() {
        Long count = empList.stream().count();
        System.out.println(count);

        Optional<Employee> op = empList.stream().max((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
        System.out.println(op.get());

        long t1 = System.currentTimeMillis();
        Optional<Double> op1 = empList.stream().map(Employee::getSalary).min(Double::compare);
        System.out.println(op1.get());
        System.out.println(System.currentTimeMillis() - t1);

        long t2 = System.currentTimeMillis();
        Optional<Employee> op2 = empList.stream().min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
        System.out.println(op2.get().getSalary());
        System.out.println(System.currentTimeMillis() - t2);
    }
  
  // 输出
  5
	Employee{id=1, name='张三', age=18, salary=999.0, status=FREE}
	555.0
	3
	555.0
	1
}

```

## Stream-规约与收集

![image-20210417133756847](/java_img/image-20210417133756847.png)

```java
    /**
     * 规约
     * reduce(T identity, BinaryOperator) / reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值
     */
    @Test
    public void test3() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Integer sum = list.stream().reduce(0, (x, y) -> x + y);
        System.out.println(sum);

        System.out.println("--------------------------");

        Optional<Double> op = empList.stream().map(Employee::getSalary).reduce(Double::sum);
        System.out.println(op.get());
    }
```

```java
/**
     * 收集
     * collect——将流转换为其他形式。接收一个Collector接口的实现，用于给Stream中元素做汇总的方法
     */
    @Test
    public void test4() {
        List<String> list = empList.stream().map(Employee::getName).collect(Collectors.toList());
        list.forEach(System.out::println);

        System.out.println("--------------------------");

        HashSet<String> hs = empList.stream().map(Employee::getName).collect(Collectors.toCollection(HashSet::new));
        hs.forEach(System.out::println);

        // 总数
        Long sum = empList.stream().collect(Collectors.counting());
        System.out.println(sum);

        System.out.println("--------------------------");

        // 平均值
        Double avg = empList.stream().collect(Collectors.averagingDouble(Employee::getSalary));
        System.out.println(avg);

        // 总和
        Double sum2 = empList.stream().collect(Collectors.summingDouble(Employee::getSalary));
        System.out.println(sum2);

        // 最大值
        Optional<Employee> max = empList.stream().collect(Collectors.maxBy((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())));
        System.out.println(max.get());

        // 最小值
        Optional<Double> min = empList.stream().map(Employee::getSalary).collect(Collectors.minBy(Double::compareTo));
        System.out.println(min.get());
    }
```

```java
    @Test
    public void test8() {
        DoubleSummaryStatistics dss = empList.stream().collect(Collectors.summarizingDouble(Employee::getSalary));
        System.out.println(dss.getSum());
        System.out.println(dss.getAverage());
        System.out.println(dss.getMax());
    }
// 输出
3552.0
710.4
999.0
```

```java
 /**
     * 分组
     */
    @Test
    public void test5() {
        Map<Employee.Status, List<Employee>> map = empList.stream().collect(Collectors.groupingBy(Employee::getStatus));
        System.out.println(map);
    }

// 输出
{VOCATION=[Employee{id=3, name='王五', age=68, salary=666.0, status=VOCATION}], FREE=[Employee{id=1, name='张三', age=18, salary=999.0, status=FREE}, Employee{id=3, name='王五', age=68, salary=666.0, status=FREE}], BUSY=[Employee{id=2, name='李四', age=58, salary=555.0, status=BUSY}, Employee{id=3, name='王五', age=68, salary=666.0, status=BUSY}]}
```

```java
/**
     * 多级分组
     */
    @Test
    public void test6() {
        Map<Employee.Status, Map<String, List<Employee>>> mapMap = empList.stream().collect(Collectors.groupingBy(Employee::getStatus, Collectors.groupingBy((e) -> {
            if (e.getAge() > 35) {
                return "青年";
            }
            if (e.getAge() <= 50) {
                return "中年";
            }
            return "老年";
        })));
        System.out.println(mapMap);
    }

// 输出
{BUSY={青年=[Employee{id=2, name='李四', age=58, salary=555.0, status=BUSY}, Employee{id=3, name='王五', age=68, salary=666.0, status=BUSY}]}, VOCATION={青年=[Employee{id=3, name='王五', age=68, salary=666.0, status=VOCATION}]}, FREE={青年=[Employee{id=3, name='王五', age=68, salary=666.0, status=FREE}], 中年=[Employee{id=1, name='张三', age=18, salary=999.0, status=FREE}]}}
```

```java
/**
     * 分区
     */
    @Test
    public void test7() {
        Map<Boolean, List<Employee>> map = empList.stream().collect(Collectors.partitioningBy((e) -> e.getSalary() > 8000));
        System.out.println(map);
    }

// 输出
{false=[Employee{id=1, name='张三', age=18, salary=999.0, status=FREE}, Employee{id=2, name='李四', age=58, salary=555.0, status=BUSY}, Employee{id=3, name='王五', age=68, salary=666.0, status=VOCATION}, Employee{id=3, name='王五', age=68, salary=666.0, status=FREE}, Employee{id=3, name='王五', age=68, salary=666.0, status=BUSY}], true=[]}
```

```java
    @Test
    public void test9() {
        String s = empList.stream().map(Employee::getName).collect(Collectors.joining(","));
        System.out.println(s);
    }
// 输出
张三,李四,王五,王五,王五
```

## 并行流与串行流

java8之前并行流

```java
package com.lzd.study.stream;

import java.util.concurrent.RecursiveTask;

public class ForkJoinCalculate extends RecursiveTask<Long> {

    private long start;
    private long end;

    private static final long THRESHOLD = 10000;

    public ForkJoinCalculate() {
    }

    public ForkJoinCalculate(long start, long end) {
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {

        long length = end - start;

        if (length < THRESHOLD) {

            long sum = 0;

            for (long i = start; i <= end; i++) {
                sum += i;
            }

            return sum;
        }

        long middle = (start + end) / 2;

        ForkJoinCalculate left = new ForkJoinCalculate(start, middle);
        left.fork();

        ForkJoinCalculate right = new ForkJoinCalculate(middle + 1, end);
        right.fork();

        return left.join() + right.join();
    }
}
```

```java
package com.lzd.study.stream;
import org.junit.Test;

import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;

public class TestForkJoin {

    @Test
    public void test() {
        ForkJoinPool pool = new ForkJoinPool();
        ForkJoinTask<Long> task = new ForkJoinCalculate(0, 100000000);
        Long sum = pool.invoke(task);
        System.out.println(sum);
    }
}
```

java8之后的并行流

```java
		@Test
    public void test1() {
        long sum = LongStream.rangeClosed(0, 100000000).parallel().reduce(0, Long::sum);
        System.out.println(sum);
    }
```

## Optional容器类

<font color=red>尽量避免空指针异常</font>

```java
package com.lzd.study.stream;

import org.junit.Test;

import java.util.Optional;

public class TestOptional {

    /**
     * Optional容器类的常用方法:
     * Optional.of(T t): 创建一个Optional实例
     */
    @Test
    public void test() {
        Optional<Employee> op = Optional.of(new Employee());

        Employee emp = op.get();
        System.out.println(emp);
    }

    /**
     * Optional.empty(): 创建一个空的Optional实例
     */
    @Test
    public void test1() {
        Optional<Object> op = Optional.empty();
        System.out.println(op.get());
    }

    /**
     * Optional.ofNullable(T t): 若t不为null, 创建Optional实例，否则创建空实例
     */
    @Test
    public void test2() {
        Optional<Employee> op = Optional.ofNullable(null);
        System.out.println(op.get());
    }

    /**
     * isPresent(): 判断是否包含值
     * orElse(T t): 如果调用对象包含值，返回该值，否则返回t
     * orElseGet(Supplier s): 如果调用对象包含值，返回该值，否则返回s获取的值
     */
    @Test
    public void test3() {
        Optional<Object> op = Optional.ofNullable(null);

//        if (op.isPresent()) {
//            System.out.println(op.get());
//        }

//        Employee emp = (Employee) op.orElse(new Employee(7, "田七", 30, 20000, Employee.Status.FREE));
//        System.out.println(emp);

        Employee emp = (Employee) op.orElseGet(() -> new Employee());
        System.out.println(emp);
    }

    /**
     * map(Function f): 如果有值对其处理，并返回处理后的Optional，否则返回Optional.empty()
     * flatMap(Function mapper): 与map类似，要求返回值必须是Optional
     */
    @Test
    public void test4() {
        Optional<Employee> op = Optional.ofNullable(new Employee(7, "田七", 30, 20000, Employee.Status.FREE));

//        Optional<String> s = op.map((e) -> e.getName());
//        System.out.println(s.get());

        Optional<String> s = op.flatMap((e) -> Optional.of(e.getName()));
        System.out.println(s.get());
    }
}
```

## 接口中的默认方法与静态方法

![image-20210418153114458](/java_img/image-20210418153114458.png)

```java
package com.lzd.study.java8;

public interface MyFun {

    default String getName() {
        return "哈哈哈";
    }
}
```

```java
package com.lzd.study.java8;

public class MyClass {

    public String getName() {
        return "嘿嘿嘿";
    }
}
```

```java
package com.lzd.study.java8;

public class SubClass extends MyClass implements MyFun {
}
```

```java
package com.lzd.study.java8;

public class TestInterface {

    public static void main(String[] args) {

        SubClass sc = new SubClass();
        System.out.println(sc.getName());
    }
}

// 输出
嘿嘿嘿
```

```java
package com.lzd.study.java8;

public interface MyFun {

    default String getName() {
        return "哈哈哈";
    }

    public static void show() {
        System.out.println("接口中的静态方法");
    }
}
```

## 传统时间格式化的线程安全问题

```java
package com.lzd.study.java8;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.concurrent.*;

public class TestSimpleDateFormat {

    public static void main(String[] args) throws ExecutionException, InterruptedException {

        SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");

        Callable<Date> task = () -> sdf.parse("20161218");

        ExecutorService pool = Executors.newFixedThreadPool(10);

        List<Future<Date>> results = new ArrayList<>();

        for (int i = 0; i < 10; i++) {
            results.add(pool.submit(task));
        }

        for (Future<Date> future : results) {
            System.out.println(future.get());
        }
    }
}

// 输出
java.util.concurrent.ExecutionException: java.lang.NumberFormatException: For input string: ".2200166E.22001664E4"
	at java.util.concurrent.FutureTask.report(FutureTask.java:122)
	at java.util.concurrent.FutureTask.get(FutureTask.java:192)
	at com.lzd.study.java8.TestSimpleDateFormat.main(TestSimpleDateFormat.java:26)
Caused by: java.lang.NumberFormatException: For input string: ".2200166E.22001664E4"
	at sun.misc.FloatingDecimal.readJavaFormatString(FloatingDecimal.java:2043)
	at sun.misc.FloatingDecimal.parseDouble(FloatingDecimal.java:110)
	at java.lang.Double.parseDouble(Double.java:538)
	at java.text.DigitList.getDouble(DigitList.java:169)
	at java.text.DecimalFormat.parse(DecimalFormat.java:2056)
	at java.text.SimpleDateFormat.subParse(SimpleDateFormat.java:1867)
	at java.text.SimpleDateFormat.parse(SimpleDateFormat.java:1514)
	at java.text.DateFormat.parse(DateFormat.java:364)
	at com.lzd.study.java8.TestSimpleDateFormat.lambda$main$0(TestSimpleDateFormat.java:15)
	at com.lzd.study.java8.TestSimpleDateFormat$$Lambda$1/1066516207.call(Unknown Source)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)
```

以前的解决办法

```java
package com.lzd.study.java8;

import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateFormatThreadLocal {

    private static final ThreadLocal<DateFormat> df = new ThreadLocal<DateFormat>() {

        @Override
        protected DateFormat initialValue() {
            return new SimpleDateFormat("yyyyMMdd");
        }
    };

    public static Date convert(String source) throws ParseException {
        return df.get().parse(source);
    }
}
```

```java
package com.lzd.study.java8;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.concurrent.*;

public class TestSimpleDateFormat {

    public static void main(String[] args) throws ExecutionException, InterruptedException {

//        SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");

        Callable<Date> task = () -> DateFormatThreadLocal.convert("20161218");

        ExecutorService pool = Executors.newFixedThreadPool(10);

        List<Future<Date>> results = new ArrayList<>();

        for (int i = 0; i < 10; i++) {
            results.add(pool.submit(task));
        }

        for (Future<Date> future : results) {
            System.out.println(future.get());
        }

        pool.shutdown();
    }
}

// 输出
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
Sun Dec 18 00:00:00 CST 2016
```

java8以后的解决方法

```java
package com.lzd.study.java8;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

public class TestSimpleDateFormat {

    public static void main(String[] args) throws ExecutionException, InterruptedException {

//        SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");

//        Callable<Date> task = () -> DateFormatThreadLocal.convert("20161218");

        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyyMMdd");

        Callable<LocalDate> task = () -> LocalDate.parse("20161218", dtf);

        ExecutorService pool = Executors.newFixedThreadPool(10);

        List<Future<LocalDate>> results = new ArrayList<>();

        for (int i = 0; i < 10; i++) {
            results.add(pool.submit(task));
        }

        for (Future<LocalDate> future : results) {
            System.out.println(future.get());
        }

        pool.shutdown();
    }
}
```

## 新时间与日期API-本地时间与时间戳

![image-20210418164006952](/java_img/image-20210418164006952.png)

```java
/**
     * 1. LocalDate LocalTime LocalDateTime
     */
    @Test
    public void test() {
        LocalDateTime ldt = LocalDateTime.now();
        System.out.println(ldt);

        LocalDateTime ldt1 = LocalDateTime.of(2021, 1, 1, 14, 59, 59);
        System.out.println(ldt1);

        LocalDateTime ldt2 = ldt1.plusYears(2);
        System.out.println(ldt2);

        LocalDateTime ldt3 = ldt1.minusMonths(2);
        System.out.println(ldt3);
    }
```

```java
/**
     * 2. Instant: 时间戳（以Unix元年：1970年1月1日00:00:00到某个时间之间的毫秒值）
     */
    @Test
    public void test1() {
        Instant ins = Instant.now(); // 默认获取UTC时区
        System.out.println(ins);

        OffsetDateTime odt = ins.atOffset(ZoneOffset.ofHours(8));
        System.out.println(odt);

        System.out.println(ins.toEpochMilli());

        Instant ins1 = Instant.ofEpochSecond(60);
        System.out.println(ins1);
    }
```

```java
/**
     * 3. Duration: 计算两个"时间"之间的间隔
     */
    @Test
    public void test2() throws InterruptedException {
        Instant ins1 = Instant.now();

        Thread.sleep(1000);

        Instant ins2 = Instant.now();

        Duration duration = Duration.between(ins1, ins2);
        System.out.println(duration.toMillis());

        System.out.println("---------------------------------");

        LocalTime lt1 = LocalTime.now();

        Thread.sleep(1000);

        LocalTime lt2 = LocalTime.now();
        
        Duration d2 = Duration.between(lt1, lt2);
        System.out.println(d2.toMillis());
    }
```

```java
/**
     * 4. Period: 计算两个"日期"之间的间隔
     */
    @Test
    public void test3() {
        LocalDate ld1 = LocalDate.of(2016, 12, 18);
        LocalDate ld2 = LocalDate.now();

        Period period = Period.between(ld1, ld2);
        System.out.println(period);

        System.out.println(period.getYears());
        System.out.println(period.getMonths());
        System.out.println(period.getDays());
    }
```

## 新时间与日期API-时间校正器

![image-20210418174810324](/java_img/image-20210418174810324.png)

```java
/**
     * TemporalAdjust: 时间校正器
     */
    @Test
    public void test4() {
        LocalDateTime ldt = LocalDateTime.now();
        System.out.println(ldt);

        LocalDateTime ldt2 = ldt.withDayOfMonth(10);
        System.out.println(ldt2);

        LocalDateTime ldt3 = ldt.with(TemporalAdjusters.next(DayOfWeek.FRIDAY));
        System.out.println(ldt3);

        // 自定义：下一个工作日
        LocalDateTime ldt5 = ldt.with((l) -> {
            LocalDateTime ldt4 = (LocalDateTime) l;

            DayOfWeek dayOfWeek = ldt4.getDayOfWeek();

            if (dayOfWeek.equals(DayOfWeek.FRIDAY)) {
                return ldt4.plusDays(3);
            }
            return ldt4.plusDays(1);
        });
        System.out.println(ldt5);
    }
```

## 新时间与日期API-时间格式化与时区的处理

```java
/**
     * DateTimeFormatter: 格式化时间/日期
     */
    @Test
    public void test5() {
        DateTimeFormatter dtf = DateTimeFormatter.ISO_DATE;
        LocalDateTime ldt = LocalDateTime.now();

        String strDate = ldt.format(dtf);
        System.out.println(strDate);

        System.out.println("------------------------");

        DateTimeFormatter dtf2 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String strDate2 = dtf2.format(ldt);
        System.out.println(strDate2);

        LocalDateTime newDate = ldt.parse(strDate2, dtf2);
        System.out.println(newDate);
    }
```

![image-20210418190812146](/java_img/image-20210418190812146.png)

```java
/**
     * ZonedDate、ZonedTime、ZonedDateTime
     */
    @Test
    public void test6() {
        Set<String> set = ZoneId.getAvailableZoneIds();
        System.out.println(set);
    }
```

```java
		@Test
    public void test7() {
        LocalDateTime ldt = LocalDateTime.now(ZoneId.of("Asia/Singapore"));
        System.out.println(ldt);

        LocalDateTime ldt2 = LocalDateTime.now(ZoneId.of("Asia/Singapore"));
        ZonedDateTime zdt = ldt2.atZone(ZoneId.of("Asia/Singapore"));
        System.out.println(zdt);
    }
```

## 重复注解与类型注解

![image-20210418192130084](/java_img/image-20210418192130084.png)

```java
package com.lzd.study.java8;

import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.*;

@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {

    String value() default "jeffrey";
}
```

```java
package com.lzd.study.java8;

import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.*;

@Repeatable(MyAnnotations.class)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, TYPE_PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {

    String value() default "jeffrey";
}
```

```java
package com.lzd.study.java8;

import org.junit.Test;

import java.lang.reflect.Method;

/**
 * 重复注解与类型注解
 */
public class TestAnnotation {

    @MyAnnotation("Hello")
    @MyAnnotation("World")
    public void show(@MyAnnotation("abc") String str) {

    }

    @Test
    public void test() throws NoSuchMethodException {

        Class<TestAnnotation> clazz = TestAnnotation.class;
        Method m = clazz.getMethod("show");

        MyAnnotation[] mas = m.getAnnotationsByType(MyAnnotation.class);

        for (MyAnnotation myAnnotation : mas) {
            System.out.println(myAnnotation.value());
        }

    }
}
```











