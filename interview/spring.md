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