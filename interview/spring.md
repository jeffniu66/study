# 1、谈谈Spring IOC的理解，原理和实现

## 总：

控制反转：原来的对象是由使用者来进行控制，有了spring之后，可以把整个对象交给spring来帮我们进行管理

DI：依赖注入，把对应属性的值注入到具体的对象中，@Autowired，populateBean完成属性的注入

容器：存储对象，使用map结构来存储，在spring中一般存在三级缓存，singletonObjects存放完整的bean对象，整个bean的生命周期，从创建到使用到销毁的过程全部都是由容器来管理（bean的生命周期）

## 分：

1、一般聊ioc容器的时候要涉及到容器的创建过程（beanFactory，DefaultListableBeanFactory），向bean工厂中设置一些参数（BeanPostProcessor，Aware接口的子类）等等属性

2、加载解析bean对象，准备要创建的bean对象的定义对象beanDefinition（xml，注解的解析过程）

3、beanFactoryPostProcessor的处理，此处是扩展点，PlaceHolderConfigurSupport，ConfigurationClassPostProcessor

4、BeanPostProcessor的注册功能，方便后续对bean对象完成具体的扩展功能

5、通过反射的方式将BeanDefinition对象实例化具体的bean对象

6、bean对象的初始化过程（填充属性，调用aware子类的方法，调用BeanPostProcessor前置处理方法，调用init-method方法，调用BeanPostProcessor的后置处理方法）

7、生成完整的bean对象，通过getBean方法可以直接获取

8、销毁过程

面试官，这是我对象ioc整体的理解，包含了一些具体的过程，您看下有什么问题可以指点一下

具体的细节可能记不太清，但是spring中bean都是通过反射的方式生成的，同时其中包含了很多的扩展点，比如最常用的对BeanFactory的扩展，对bean的扩展（对占位符的处理）。

# 2、谈一下spring IOC的底层实现

底层实现：工作原理、过程、数据结构、流程、设计模式、设计思想

你对他的理解和你了解的实现过程

反射、工厂、设计模式、关键的几个方法

createBeanFactory、getBean、doGetBean、createBean、doCreateBean、createBeanInstance（getDeclaredConstructor、newinstance）、populateBean、initializingBean

1、先通过createBeanFactory创建出一个Bean工厂（DefaultListableBeanFactory）

2、开始循环创建对象，因为容器中的bean默认都是单例的，所以优先通过getBean、doGetBean从容器中查找，找不到的话

3、通过createBean、doCreateBean方法，以反射的方式创建对象，一般情况下使用的是无参的构造方法（getDeclaredConstructor、newinstance）

4、进行对象属性的填充populateBean

5、进行其他的初始化操作（initializingBean）

# 3、描述一下bean的生命周期

1、实例化bean：反射的方式生成对象

2、填充bean的属性：populateBean()，循环依赖的问题（三级缓存）

3、调用aware接口相关的方法：invokeAwareMethod(完成BeanName，BeanFactory，BeanClassLoader对象的属性设置)

4、调用BeanPostProcessor中的前置处理方法：使用比较多的有（ApplicationContextPostProcessor，设置ApplicationContext，Environment，ResourceLoad，EmbedValueResolver等对象）

5、调用initmethod方法：invokeInitmethod()，判断是否实现了initializingBean接口，如果有，调用afterPropertiesSet方法，没有就不调用

6、调用BeanPostProcessor的后置处理方法，spring的aop就是在此处实现的，AbstractAutoProxyCreator

​		注册Destructor相关的回调接口：钩子函数

7、获取到完整的对象，可以通过getBean的方式来进行对象的获取

8、销毁流程，一：判断是否实现了DisposebleBean接口；二：调用destoryMethod方法

# 4、Spring是如何解决循环依赖的问题

三级缓存，提前暴露对象，aop

总：什么是循环依赖问题，A依赖B，B依赖A

分：先说明bean的创建过程：实例化，初始化（填充属性）

​	1、先创建A对象，实例化A对象，次数A对象中的b属性为空，填充属性b

​	2、从容器中查找Bean对象，如果找到了，直接赋值，不存在循环依赖问题（不通），找不到直接创建B对象

​	3、实例化B对象，此时B对象中的a属性为空，填充属性a

​	4、从容器中查找A对象，找不到，直接创建

形成闭环的原因

此时，如果仔细琢磨的话，会发现A对象是存在的，只不过此时的A对象不是一个完整的状态，只完成了实例化但是未完成初始化，

如果在程序调用过程中，拥有了某个对象引用，能否在后期给他完成赋值的操作，可以优先把非完整状态的对象优先赋值，等待后续操作来完成赋值，相当于提前暴露了某个不完整对象的引用，所以解决问题的核心在于实例化和初始化分开操作，这也是解决循环依赖问题的关键

当所有的对象都完成实例化和初始化操作之后，还要把完整的对象放到容器中，此时在容器中存在对象的几个状态，完成实例化=但未完成初始化，完整状态，因为都在容器中，所以要使用不同的map结构来进行存储，此时就有了一级缓存和二级缓存，如果一级缓存中有了，那么二级缓存中就不会存在同名的对象，因为他们查找的顺序是1，2，3这样的方式来查找的。一级缓存中放的是完整对象，二级缓存中放的是非完整对象

为什么需要三级缓存？三级缓存的value类型是ObjectFactory，是一个函数式接口，存在的意义是保证在整个容器的运行过程中同名的bean对象只能有一个。

如果一个对象需要被代理，或者说需要生成代理对象，那么要不要优先生成一个普通对象？要

普通对象和代理对象是不能同时出现在容器中的，因为当一个对象需要被代理的时候，就要使用代理对象覆盖掉之前的普通对象，在实际的调用过程中，是没有办法确定什么时候对象被使用，所以就要求当某个对象被调用的时候，优先判断对象是否需要被代理，类似一种回调机制的实现，因此传入lambda表达式的时候，可以通过lambda表达式来执行对象的覆盖过程，getEarlyBeanReference()

因此，所有的bean对象在创建的时候都要优先放到三级缓存中，在后续的使用过程中，如果需要使用被代理则返回代理对象，如果不需要，则直接返回普通对象





