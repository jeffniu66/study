---
typora-root-url: ../study
typora-copy-images-to: ./design_img
---

# 命令模式

## 类图

![image-20210628220421289](/design_img/image-20210628220421289.png)

### Command.java

```java
package com.lzd.designmode.command;

public interface Command {

    void execute();

    void undo();
}
```

### LightOnCommand.java

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

### LightOffCommand.java

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

### LightReceive.java

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

### NoCommand.java

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

### RemoteController.java

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

### Client.java

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

## 命令模式在Spring框架JdbcTemplate应用的源码分析

StatementCallback接口，类似命令接口（Command）

```java
class QueryStatementCallback implements StatementCallback<T>, SqlProvider
```

匿名内部类，实现了命令接口，同时也充当命令接收者

命令调用者是JdbcTemplate，其中execute(StatementCallback<T> action)方法中，调用action.doInStatement方法，不同的实现StatementCallback接口的对象，对应不同的doInStatement实现逻辑

另外实现StatementCallback命令接口的子类还有

![image-20210629232440295](/design_img/image-20210629232440295.png)

## 命令模式的注意事项和细节

![image-20210629233910530](/design_img/image-20210629233910530.png)











