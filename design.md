---
typora-root-url: ../study
typora-copy-images-to: ./design_img
---

# 创建型模式

## 简单工厂（静态工厂）

### 传统写法

#### 代码

##### Pizza.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.pizza;

public abstract class Pizza {

    protected String name;

    // 准备原材料，不同的披萨不一样，因此，我们做成抽象方法
    public abstract void prepare();

    public void bake() {
        System.out.println(name + " baking");
    }

    public void cut() {
        System.out.println(name + "cutting");
    }

    public void box() {
        System.out.println(name + " boxing");
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

##### GreekPizza.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.pizza;

public class GreekPizza extends Pizza {

    @Override
    public void prepare() {
        System.out.println("给希腊披萨 准备原材料");
    }
}
```

##### CheesePizza.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.pizza;

public class CheesePizza extends Pizza {

    @Override
    public void prepare() {
        System.out.println("给制作奶酪披萨 准备原材料");
    }
}
```

##### OrderPizza.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.order;

import com.lzd.designpattern.factory.simplefactory.pizzastore.pizza.CheesePizza;
import com.lzd.designpattern.factory.simplefactory.pizzastore.pizza.GreekPizza;
import com.lzd.designpattern.factory.simplefactory.pizzastore.pizza.Pizza;

import java.io.BufferedReader;
import java.io.InputStreamReader;

public class OrderPizza {

    public OrderPizza() {
        Pizza pizza;
        String orderType; // 订购披萨类型
        do {
            orderType = getType();
            if ("greek".equals(orderType)) {
                pizza = new GreekPizza();
                pizza.setName(" 希腊披萨 ");
            } else if ("cheese".equals(orderType)) {
                pizza = new CheesePizza();
                pizza.setName(" 奶酪披萨 ");
            } else {
                break;
            }
            // 输出pizza制作过程
            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();
        } while (true);
    }

    // 获取客户希望订购的披萨种类
    private String getType() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza type: ");
            return strin.readLine();
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }
}
```

##### PizzaStore.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.order;

public class PizzaStore {

    public static void main(String[] args) {
        new OrderPizza();
    }
}
```

#### 存在的问题

如果存在到处创建pizza的代码，即多个OrderPizza，那么每一处都需要修改，违法了OCP原则，考虑把创建pizza对象封装到一个类中。

### 介绍

<font color=gree>1.简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例，通常是一类对象。简单工厂模式是工厂模式家族中最实用且用得最多的模式。</font>

<font color=gree>2.简单工厂模式：定义了一个创建对象的类，由这个类来封装实例化对象的行为（代码）</font>

<font color=gree>3.在软件开发中，当我们会用到大量的创建某种，某类或者谋批对象时，就会使用到工厂模式</font>

### 代码

#### SimpleFactory.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.order;

import com.lzd.designpattern.factory.simplefactory.pizzastore.pizza.CheesePizza;
import com.lzd.designpattern.factory.simplefactory.pizzastore.pizza.GreekPizza;
import com.lzd.designpattern.factory.simplefactory.pizzastore.pizza.Pizza;

public class SimpleFactory {

    public Pizza createPizza(String orderType) { // 也可以写成静态方法 静态工厂

        Pizza pizza = null;

        System.out.println("使用简单工厂模式");

        if ("greek".equals(orderType)) {
            pizza = new GreekPizza();
            pizza.setName(" 希腊披萨 ");
        } else if ("cheese".equals(orderType)) {
            pizza = new CheesePizza();
            pizza.setName(" 奶酪披萨 ");
        }

        return pizza;
    }
}
```

#### OrderPizza.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.order;

import com.lzd.designpattern.factory.simplefactory.pizzastore.pizza.Pizza;

import java.io.BufferedReader;
import java.io.InputStreamReader;

public class OrderPizza {

//    public OrderPizza() {
//        Pizza pizza;
//        String orderType; // 订购披萨类型
//        do {
//            orderType = getType();
//            if ("greek".equals(orderType)) {
//                pizza = new GreekPizza();
//                pizza.setName(" 希腊披萨 ");
//            } else if ("cheese".equals(orderType)) {
//                pizza = new CheesePizza();
//                pizza.setName(" 奶酪披萨 ");
//            } else {
//                break;
//            }
//            // 输出pizza制作过程
//            pizza.prepare();
//            pizza.bake();
//            pizza.cut();
//            pizza.box();
//        } while (true);
//    }

    // 定义一个简单工厂对象
    SimpleFactory simpleFactory;
    Pizza pizza;

    public OrderPizza(SimpleFactory simpleFactory) {
        setFactory(simpleFactory);
    }

    public void setFactory(SimpleFactory simpleFactory) {

        String orderType; // 用户输入的

        this.simpleFactory = simpleFactory;

        do {
            orderType = getType();
            pizza = this.simpleFactory.createPizza(orderType);

            // 输出pizza
            if (pizza != null) { // 订购成功
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            } else {
                System.out.println(" 订购披萨失败 ");
                break;
            }
        } while (true);
    }


    // 获取客户希望订购的披萨种类
    private String getType() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza type: ");
            return strin.readLine();
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }
}
```

#### PizzaStore.java

```java
package com.lzd.designpattern.factory.simplefactory.pizzastore.order;

public class PizzaStore {

    public static void main(String[] args) {
//        new OrderPizza();
        new OrderPizza(new SimpleFactory());
        System.out.println("退出了程序");
    }
}
```



## 工厂方法模式

### 介绍

定义了一个创建对象的抽象方法，由子类决定要实例化的类。工厂方法模式将<font color=red>对象的实例化推迟到子类</font>

### 代码

#### Pizza.java

```java
package com.lzd.designpattern.factory.factorymethod.pizza;

public abstract class Pizza {

    protected String name;

    // 准备原材料，不同的披萨不一样，因此，我们做成抽象方法
    public abstract void prepare();

    public void bake() {
        System.out.println(name + " baking");
    }

    public void cut() {
        System.out.println(name + "cutting");
    }

    public void box() {
        System.out.println(name + " boxing");
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

#### BJCheesePizza.java

```java
package com.lzd.designpattern.factory.factorymethod.pizza;

public class BJCheesePizza extends Pizza {

    @Override
    public void prepare() {
        setName("北京奶酪pizza");
        System.out.println(" 北京的奶酪pizza 准备原材料");
    }
}

```

#### BJPepperPizza.java

```java
package com.lzd.designpattern.factory.factorymethod.pizza;

public class BJPepperPizza extends Pizza {

    @Override
    public void prepare() {
        setName("北京胡椒pizza");
        System.out.println(" 北京的胡椒pizza 准备原材料");
    }
}

```

#### LDCheesePizza.java

```java
package com.lzd.designpattern.factory.factorymethod.pizza;

public class LDCheesePizza extends Pizza {

    @Override
    public void prepare() {
        setName("伦敦奶酪pizza");
        System.out.println(" 伦敦的奶酪pizza 准备原材料");
    }
}

```

#### LDPepperPizza.java

```java
package com.lzd.designpattern.factory.factorymethod.pizza;

public class LDPepperPizza extends Pizza {

    @Override
    public void prepare() {
        setName("伦敦胡椒pizza");
        System.out.println(" 伦敦的胡椒pizza 准备原材料");
    }
}

```

#### OrderPizza.java

```java
package com.lzd.designpattern.factory.factorymethod.order;

import com.lzd.designpattern.factory.factorymethod.pizza.Pizza;

import java.io.BufferedReader;
import java.io.InputStreamReader;

public abstract class OrderPizza {

    // 定义一个抽象方法，createPizza，让各个工厂子类自己实现
    abstract Pizza createPizza(String orderType);

    public OrderPizza() {
        Pizza pizza;
        String orderType; // 订购披萨类型
        do {
            orderType = getType();
            pizza = createPizza(orderType);
            // 输出pizza制作过程
            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();
        } while (true);
    }

    // 获取客户希望订购的披萨种类
    private String getType() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza type: ");
            return strin.readLine();
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }
}

```

#### BJOrderPizza.java

```java
package com.lzd.designpattern.factory.factorymethod.order;

import com.lzd.designpattern.factory.factorymethod.pizza.BJCheesePizza;
import com.lzd.designpattern.factory.factorymethod.pizza.BJPepperPizza;
import com.lzd.designpattern.factory.factorymethod.pizza.Pizza;

public class BJOrderPizza extends OrderPizza {

    @Override
    Pizza createPizza(String orderType) {
        Pizza pizza = null;
        if ("cheese".equals(orderType)) {
            pizza = new BJCheesePizza();
        } else if ("pepper".equals(orderType)) {
            pizza = new BJPepperPizza();
        }
        return pizza;
    }
}

```

#### LDOrderPizza.java

```java
package com.lzd.designpattern.factory.factorymethod.order;

import com.lzd.designpattern.factory.factorymethod.pizza.LDCheesePizza;
import com.lzd.designpattern.factory.factorymethod.pizza.LDPepperPizza;
import com.lzd.designpattern.factory.factorymethod.pizza.Pizza;

public class LDOrderPizza extends OrderPizza {

    @Override
    Pizza createPizza(String orderType) {
        Pizza pizza = null;
        if ("cheese".equals(orderType)) {
            pizza = new LDCheesePizza();
        } else if ("pepper".equals(orderType)) {
            pizza = new LDPepperPizza();
        }
        return pizza;
    }
}

```

#### PizzaStore.java

```java
package com.lzd.designpattern.factory.factorymethod.order;

public class PizzaStore {

    public static void main(String[] args) {
//        new BJOrderPizza();
        new LDOrderPizza();
    }
}

```

### 核心

<font color=red>主要是要把创建pizza对象的方法抽象出来，让子类去实现</font>

## 抽象工厂

### 介绍

![image-20210829182133195](/design_img/image-20210829182133195.png)

![image-20210829183529698](/design_img/image-20210829183529698.png)

### 代码

#### AbsFactory.java

```java
package com.lzd.designpattern.factory.absfactory.pizzastore.order;

import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.Pizza;

// 一个抽象工厂模式的抽象层（接口）
public interface AbsFactory {

    Pizza createPizza(String orderType);
}
```

#### BJFactory.java

```java
package com.lzd.designpattern.factory.absfactory.pizzastore.order;

import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.BJCheesePizza;
import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.BJPepperPizza;
import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.Pizza;

public class BJFactory implements AbsFactory {

    @Override
    public Pizza createPizza(String orderType) {
        System.out.println("使用的是抽象工厂模式");
        Pizza pizza = null;
        if ("cheese".equals(orderType)) {
            pizza = new BJCheesePizza();
        } else if ("pepper".equals(orderType)) {
            pizza = new BJPepperPizza();
        }
        return pizza;
    }
}
```

#### LDFactory.java

```java
package com.lzd.designpattern.factory.absfactory.pizzastore.order;

import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.LDCheesePizza;
import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.LDPepperPizza;
import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.Pizza;

public class LDFactory implements AbsFactory {

    @Override
    public Pizza createPizza(String orderType) {
        System.out.println("使用的是抽象工厂模式");
        Pizza pizza = null;
        if ("cheese".equals(orderType)) {
            pizza = new LDCheesePizza();
        } else if ("pepper".equals(orderType)) {
            pizza = new LDPepperPizza();
        }
        return pizza;
    }
}
```

#### OrderPizza.java

```java
package com.lzd.designpattern.factory.absfactory.pizzastore.order;


import com.lzd.designpattern.factory.absfactory.pizzastore.pizza.Pizza;

import java.io.BufferedReader;
import java.io.InputStreamReader;

public class OrderPizza {

    AbsFactory factory;

    // 构造器
    public OrderPizza(AbsFactory factory) {
        setAbsFactory(factory);
    }

    private void setAbsFactory(AbsFactory factory) {
        Pizza pizza;
        String orderType; // 订购披萨类型
        this.factory = factory;
        do {
            orderType = getType();
            pizza = factory.createPizza(orderType);
            if (pizza != null) {
                // 输出pizza制作过程
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            } else {
                System.out.println("订购失败");
                break;
            }
        } while (true);
    }

    // 获取客户希望订购的披萨种类
    private String getType() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza type: ");
            return strin.readLine();
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }
}
```

#### PizzaStore.java

```java
package com.lzd.designpattern.factory.absfactory.pizzastore.order;

public class PizzaStore {

    public static void main(String[] args) {

//        new OrderPizza(new BJFactory());
        new OrderPizza(new LDFactory());
    }
}
```

### JDK源码分析

![image-20210829222904603](/design_img/image-20210829222904603.png)

### 工厂模式小节

![image-20210829223410403](/design_img/image-20210829223410403.png)



# 行为模式

## 命令模式

### 类图代码

![image-20210628220421289](/design_img/image-20210628220421289.png)

#### Command.java

```java
package com.lzd.designmode.command;

public interface Command {

    void execute();

    void undo();
}
```

#### LightOnCommand.java

```java
package com.lzd.designmode.command;

public class LightOnCommand implements Command {

    LightReceiver light;

    public LightOnCommand(LightReceiver light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.on();
    }

    @Override
    public void undo() {
        light.off();
    }
}
```

#### LightOffCommand.java

```java
package com.lzd.designmode.command;

public class LightOffCommand implements Command {

    LightReceiver light;

    public LightOffCommand(LightReceiver light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.off();
    }

    @Override
    public void undo() {
        light.on();
    }
}
```

#### LightReceive.java

```java
package com.lzd.designmode.command;

public class LightReceiver {

    public void on() {
        System.out.println("电灯打开了");
    }

    public void off() {
        System.out.println("电灯关闭了");
    }
}
```

#### NoCommand.java

```java
package com.lzd.designmode.command;

/**
 * 没有任何命令，即空执行：用于初始化每个按钮，当调用空命令时，对象什么都不做
 * 其实，这也是一种设计模式，可以省掉对空判断
 */
public class NoCommand implements Command {

    @Override
    public void execute() {

    }

    @Override
    public void undo() {

    }
}
```

#### RemoteController.java

```java
package com.lzd.designmode.command;

public class RemoteController {

    // 开 按钮的命令数组
    Command[] onCommands;
    Command[] offCommands;

    // 执行撤销的命令
    Command undoCommand;

    // 构造器，完成对按钮初始化
    public RemoteController() {
        onCommands = new Command[5];
        offCommands = new Command[5];

        for (int i = 0; i < 5; i++) {
            onCommands[i] = new NoCommand();
            offCommands[i] = new NoCommand();
        }
    }

    // 给我们的按钮设置你需要的命令
    public void setCommand(int no, Command onCommand, Command offCommand) {
        onCommands[no] = onCommand;
        offCommands[no] = offCommand;
    }

    // 按下开按钮
    public void onButtonWasPushed(int no) {
        // 找到你按下的开的按钮，并调用对应方法
        onCommands[no].execute();
        // 记录这次的操作，用于撤销
        undoCommand = onCommands[no];
    }

    // 按下关按钮
    public void offButtonWasPushed(int no) {
        // 找到你按下的关的按钮，并调用对应方法
        offCommands[no].execute();
        // 记录这次的操作，用于撤销
        undoCommand = offCommands[no];
    }

    // 按下撤销按钮
    public void undoButtonWasPushed() {
        undoCommand.undo();
    }
}
```

#### Client.java

```java
package com.lzd.designmode.command;

public class Client {

    public static void main(String[] args) {

        LightReceiver lightReceiver = new LightReceiver();

        LightOnCommand lightOnCommand = new LightOnCommand(lightReceiver);

        LightOffCommand lightOffCommand = new LightOffCommand(lightReceiver);

        RemoteController remoteController = new RemoteController();

        remoteController.setCommand(0, lightOnCommand, lightOffCommand);

        System.out.println("------------按下灯的开按钮---------------");
        remoteController.onButtonWasPushed(0);
        System.out.println("------------按下灯的关按钮---------------");
        remoteController.offButtonWasPushed(0);
        System.out.println("------------按下撤销按钮---------------");
        remoteController.undoButtonWasPushed();
    }
}
```

### 命令模式在Spring框架JdbcTemplate应用的源码分析

StatementCallback接口，类似命令接口（Command）

```java
class QueryStatementCallback implements StatementCallback<T>, SqlProvider
```

匿名内部类，实现了命令接口，同时也充当命令接收者

命令调用者是JdbcTemplate，其中execute(StatementCallback<T> action)方法中，调用action.doInStatement方法，不同的实现StatementCallback接口的对象，对应不同的doInStatement实现逻辑

另外实现StatementCallback命令接口的子类还有

![image-20210629232440295](/design_img/image-20210629232440295.png)

### 命令模式的注意事项和细节

![image-20210629233910530](/design_img/image-20210629233910530.png)

## 中介者模式

![image-20210630220322727](/design_img/image-20210630220322727.png)

#### 类图

![image-20210630221808164](/design_img/image-20210630221808164.png)

![image-20210630222023353](/design_img/image-20210630222023353.png)

#### 代码案例

### ![image-20210630225354371](/design_img/image-20210630225354371.png)

![image-20210630231159325](/design_img/image-20210630231159325.png)

# 结构型模式

## 装饰者模式

![image-20210703101712714](/design_img/image-20210703101712714.png)

![image-20210703103335203](/design_img/image-20210703103335203.png)



### 装饰者模式解决星巴克咖啡订单

![image-20210703172631162](/design_img/image-20210703172631162.png)

### 代码

#### Drink.java

```java
package com.lzd.designpattern.decorator;

public abstract class Drink {

    public String des; // 描述
    private float price = 0.0f;

    public String getDes() {
        return des;
    }

    public void setDes(String des) {
        this.des = des;
    }

    public float getPrice() {
        return price;
    }

    public void setPrice(float price) {
        this.price = price;
    }

    // 计算费用的抽象方法
    // 子类来实现
    public abstract float cost();
}
```

#### Coffee.java

```java
package com.lzd.designpattern.decorator;

public class Coffee extends Drink {

    @Override
    public float cost() {
        return super.getPrice();
    }
}
```

#### Espresso.java

```java
package com.lzd.designpattern.decorator;

public class Espresso extends Coffee {

    public Espresso() {
        setDes(" 意大利咖啡 ");
        setPrice(6.0f);
    }
}
```

#### LongBlack.java

```java
package com.lzd.designpattern.decorator;

public class LongBlack extends Coffee {

    public LongBlack() {
        setDes(" longblack ");
        setPrice(5.0f);
    }
}
```

#### ShortBlack.java

```java
package com.lzd.designpattern.decorator;

public class ShortBlack extends Coffee {

    public ShortBlack() {
        setDes(" shortblack ");
        setPrice(4.0f);
    }
}
```

#### Decorator.java

```java
package com.lzd.designpattern.decorator;

public class Decorator extends Drink {

    private Drink obj;

    public Decorator(Drink obj) { // 组合
        this.obj = obj;
    }

    @Override
    public float cost() {
        return super.getPrice() + obj.cost();
    }

    @Override
    public String getDes() {
        // obj.getDes() 输出被装饰者的信息
        return des + " " + getPrice() + " && " + obj.getDes();
    }
}
```

#### Chocolate.java

```java
package com.lzd.designpattern.decorator;

// 具体的Decorator, 这里就是调味品
public class Chocolate extends Decorator {

    public Chocolate(Drink obj) {
        super(obj);
        setDes(" 巧克力 ");
        setPrice(3.0f);
    }
}
```

#### Milk.java

```java
package com.lzd.designpattern.decorator;

public class Milk extends Decorator {

    public Milk(Drink obj) {
        super(obj);
        setDes(" 牛奶 ");
        setPrice(2.0f);
    }
}
```

#### Soy.java

```java
package com.lzd.designpattern.decorator;

public class Soy extends Decorator {

    public Soy(Drink obj) {
        super(obj);
        setDes(" 豆浆 ");
        setPrice(1.5f);
    }
}
```

#### CoffeeBar.java

```java
package com.lzd.designpattern.decorator;

public class CoffeeBar {

    public static void main(String[] args) {
        // 装饰者模式下订单：2份巧克力 + 一份牛奶的LongBlack

        // 1. 点一份LongBlack
        Drink order = new LongBlack();
        System.out.println("费用1=" + order.cost());
        System.out.println("描述=" + order.getDes());

        // 2. 加入一份牛奶
        order = new Milk(order);
        System.out.println("order 加入一份牛奶 费用=" + order.cost());
        System.out.println("order 加入一份牛奶 描述=" + order.getDes());

        // 3. order 加入一份巧克力
        order = new Chocolate(order);
        System.out.println("order 加入一份巧克力 加入一份牛奶 费用=" + order.cost());
        System.out.println("order 加入一份巧克力 加入一份牛奶 描述=" + order.getDes());
    }
}
```

### IO源码

1. InputStream是抽象类，类似我们前面讲的Drink
2. FileInputStream是InputStream子类，类似我们前面的Decaf，LongBlack
3. FilterInputStream是InputStream子类：类似我们前面的Decorator 修饰者
4. DataInputStream是FilterInputStream子类，具体的修饰者，类似前面的Milk，Soy等
5. FilterInputStream类有protected volatile InputStream in; 即含被装饰者
6. 分析得出在jdk的io体系中，就是使用装饰者模式

```java
package com.lzd.designpattern.decorator;

import java.io.DataInputStream;
import java.io.FileInputStream;

public class DecoratorIo {

    public static void main(String[] args) throws Exception {

        DataInputStream dis = new DataInputStream(new FileInputStream("d:\\abc.txt"));
        System.out.println(dis.read());
        dis.close();
    }
}
```

## 组合模式

### 代码

#### OrganizationComponent

```java
package com.lzd.designpattern.composite;

public abstract class OrganizationComponent {

    private String name; // 名字
    private String des; // 说明

    public OrganizationComponent(String name, String des) {
        super();
        this.name = name;
        this.des = des;
    }

    protected void add(OrganizationComponent organizationComponent) {
        // 默认实现
        throw new UnsupportedOperationException();
    }

    protected void remove(OrganizationComponent organizationComponent) {
        // 默认实现
        throw new UnsupportedOperationException();
    }

    // 方法print，做成抽象的，子类都需要实现
    protected abstract void print();

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDes() {
        return des;
    }

    public void setDes(String des) {
        this.des = des;
    }
}
```

#### University

```java
package com.lzd.designpattern.composite;

import java.util.ArrayList;
import java.util.List;

// University就是Composite，可以管理College
public class University extends OrganizationComponent {

    List<OrganizationComponent> organizationComponents = new ArrayList<>();

    public University(String name, String des) {
        super(name, des);
    }

    @Override
    protected void add(OrganizationComponent organizationComponent) {
        organizationComponents.add(organizationComponent);
    }

    @Override
    protected void remove(OrganizationComponent organizationComponent) {
        organizationComponents.remove(organizationComponent);
    }

    @Override
    public String getName() {
        return super.getName();
    }

    @Override
    public String getDes() {
        return super.getDes();
    }

    /**
     * 输出University包含的学院
     */
    @Override
    protected void print() {
        System.out.println("----------------" + getName() + "--------------");
        for (OrganizationComponent organizationComponent : organizationComponents) {
            organizationComponent.print();
        }
    }

}
```

#### Department

```java
package com.lzd.designpattern.composite;

public class Department extends OrganizationComponent {

    public Department(String name, String des) {
        super(name, des);
    }

    @Override
    public String getName() {
        return super.getName();
    }

    @Override
    public String getDes() {
        return super.getDes();
    }

    @Override
    protected void print() {
        System.out.println(getName());
    }
}
```

#### College

```java
package com.lzd.designpattern.composite;

import java.util.ArrayList;
import java.util.List;

public class College extends OrganizationComponent {

    // 存放的Department
    List<OrganizationComponent> organizationComponents = new ArrayList<>();

    public College(String name, String des) {
        super(name, des);
    }

    @Override
    protected void add(OrganizationComponent organizationComponent) {
        organizationComponents.add(organizationComponent);
    }

    @Override
    protected void remove(OrganizationComponent organizationComponent) {
        organizationComponents.remove(organizationComponent);
    }

    @Override
    public String getName() {
        return super.getName();
    }

    @Override
    public String getDes() {
        return super.getDes();
    }

    /**
     * 输出University包含的学院
     */
    @Override
    protected void print() {
        System.out.println("----------------" + getName() + "--------------");
        for (OrganizationComponent organizationComponent : organizationComponents) {
            organizationComponent.print();
        }
    }

}
```

#### Client

```java
package com.lzd.designpattern.composite;

public class Client {

    public static void main(String[] args) {

        // 从大到小创建对象 学校
        OrganizationComponent university = new University("清华大学", "中国顶级大学");

        // 创建学院
        OrganizationComponent computerCollege = new College("计算机学院", "计算机学院");
        OrganizationComponent infoEngineerCollege = new College("信息工程学院", "信息工程学院");

        // 创建各个学院下面的系（专业）
        computerCollege.add(new Department("软件工程", "软件工程不错"));
        computerCollege.add(new Department("网络工程", "网络工程不错"));
        computerCollege.add(new Department("计算机科学与技术", "计算机科学与技术是一个老牌专业"));

        infoEngineerCollege.add(new Department("通信工程", "通信工程不好学"));
        infoEngineerCollege.add(new Department("信息工程", "信息工程好学"));

        university.add(computerCollege);
        university.add(infoEngineerCollege);

        university.print();
    }
}
```

### JDK源码使用了组合模式

<font color=red>AbstractMap和Map相当于Component，HashMap相当于Composite，Node是静态内部类，相当于叶子节点Leaf</font>

### 组合模式的注意事项和细节

![image-20210724145908475](/design_img/image-20210724145908475.png)



