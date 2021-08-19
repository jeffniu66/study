---
typora-root-url: ../study
typora-copy-images-to: ./rabbitmq_img
---

# MQ简介

Java里面也提供了Queue，但是是内存级别的，很受限制。分布式中可能有很多服务都需要从队列取数据，所以需要消息中间件。

## 使用场景

### 异步处理

![image-20210808111208710](/rabbitmq_img/image-20210808111208710.png)

### 应用解耦

![image-20210808111747713](/rabbitmq_img/image-20210808111747713.png)

### 流量控制

![image-20210808112107811](/rabbitmq_img/image-20210808112107811.png)

# RabbitMQ简介

## 模式

### 点对点（队列）

1.消息发送者发送消息，消息代理将其放入一个队列中，消息接收者从队列中获取消息内容，消息读取后被移出队列。

2.消息只有唯一的发送者和接收者，但并不是说只有一个接收者。

### 发布订阅（主题）

发送者发送消息到主题，多个接收者（订阅者）监听（订阅）这个主题，那么就会在消息到达时同时收到消息。

## JMS（Java Mesaage Service）

基于JVM消息代理的规范。ActiveMQ、HornetMQ是JMS实现。

## AMQP（Advance Message Queue Protocol）

1.高级消息队列协议，也是消息代理的规范，兼容JMS。

2.RabbitMQ是AMQP的实现。

## ActiveMQ与RabbitMQ对比

![image-20210808115055135](/rabbitmq_img/image-20210808115055135.png)

## Spring的支持

![image-20210808115525219](/rabbitmq_img/image-20210808115525219.png)

# RabbitMQ工作流程

![image-20210809225143209](/rabbitmq_img/image-20210809225143209.png)

# RabbitMQ安装

![image-20210809231220100](/rabbitmq_img/image-20210809231220100.png)

```java
https://rabbitmq.com/networking.html
```

创建并启动

```shell
docker run -d --name rabbitmq -p 5671:5671 \
-p 5672:5672 -p 4369:4369 -p 25672:25672 -p 15671:15671 -p 15672:15672 \
rabbitmq:management
```

设置开机启动

```shell
docker update rabbitmq --restart=always
```

访问地址

```java
192.168.8.136:15672
```

账号密码一致 guest

# Exchange类型

![image-20210810223043117](/rabbitmq_img/image-20210810223043117.png)

![image-20210810224551099](/rabbitmq_img/image-20210810224551099.png)

![image-20210810224406332](/rabbitmq_img/image-20210810224406332.png)

# Direct-Exchange

![image-20210810225747633](/rabbitmq_img/image-20210810225747633.png)

# Fanout-Exchange

无论routing-key用的是什么，发消息后绑定的队列都能收到，广播型的

# Topic-Exchange

通过routing-key来模糊匹配，通常是用于同一类发送消息

# SpringBoot整合RabbitMQ

![image-20210812141313103](/rabbitmq_img/image-20210812141313103.png)

1.引入了amqp，RabbitAutoConfiguration就会自动生效

2.给容器中自动配置了RabbitTemplate，AmqpAdmin，CachingConnectionFactory，RabbitMessagingTemplate，所有的属性都是

@ConfigurationProperties(prefix="spring.rabbitmq")

public class RabbitProperties {}

3.@EnableRabbit就可以使用rabbitmq了

4.如何创建Exchange，Queue，Binding?

使用AmqpAdmin进行创建

5.rabbitmq配置信息

```properties
spring.rabbitmq.host=192.168.8.136
spring.rabbitmq.port=5672
spring.rabbitmq.virtual-host=/
```

# AmqpAdmin使用

```java
package com.lzd.xmall.order;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.amqp.core.AmqpAdmin;
import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.DirectExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@Slf4j
@SpringBootTest
class XmallOrderApplicationTests {


    @Autowired
    AmqpAdmin amqpAdmin;

    @Test
    void contextExchange() {
        DirectExchange directExchange = new DirectExchange("hello-java-exchange");
        amqpAdmin.declareExchange(directExchange);
        log.info("Exchange[{}]创建成功", "hello-java-exchange");
    }

    @Test
    void createQueue() {
        Queue queue = new Queue("hello-java-queue");
        amqpAdmin.declareQueue(queue);
        log.info("Queue[{}]创建成功", "hello-java-queue");
    }

    @Test
    void createBinding() {
        Binding binding = new Binding("hello-java-queue", Binding.DestinationType.QUEUE, "hello-java-exchange", "hello.java", null);
        amqpAdmin.declareBinding(binding);
        log.info("Binding[{}]创建成功", "hello-java-binding");
    }

}
```

# RabbitTemplate使用

```java
@Slf4j
@SpringBootTest
class XmallOrderApplicationTests {


    @Autowired
    AmqpAdmin amqpAdmin;
    @Autowired
    RabbitTemplate rabbitTemplate;

    @Test
    void sendMessageTest() {
        String msg = "Hello World";
        rabbitTemplate.convertAndSend("hello-java-exchange", "hello.java", msg);
        log.info("消息发送完成{}", msg);
    }
```

如果是对象，默认使用的是java序列化，可以自定义Jackson2Json序列化

```java
package com.lzd.xmall.order.config;

import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyRabbitConfig {

    @Bean
    public MessageConverter messageConverter() {
        return new Jackson2JsonMessageConverter();
    }
}
```

# RabbitListener&RabbitHandler接收消息

1.监听消息：@RabbitListener，必须放在@Service中

2.@RabbitListener：类+方法（监听哪些队列即可）

RabbitHandler：标在方法上（重载区分不同的消息）

```java
package com.lzd.xmall.order.service.impl;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.lzd.common.utils.PageUtils;
import com.lzd.common.utils.Query;
import com.lzd.xmall.order.dao.OrderItemDao;
import com.lzd.xmall.order.entity.OrderItemEntity;
import com.lzd.xmall.order.service.OrderItemService;
import com.rabbitmq.client.AMQP;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Service;

import java.util.Map;


@Service("orderItemService")
@RabbitListener(queues = {"hello-java-queue"})
public class OrderItemServiceImpl extends ServiceImpl<OrderItemDao, OrderItemEntity> implements OrderItemService {

    @Override
    public PageUtils queryPage(Map<String, Object> params) {
        IPage<OrderItemEntity> page = this.page(
                new Query<OrderItemEntity>().getPage(params),
                new QueryWrapper<>()
        );

        return new PageUtils(page);
    }

    /**
     * queues: 声明需要监听的所有队列
     * <p>
     * class org.springframework.amqp.core.Message
     * <p>
     * 参数可以写一下类型
     * 1. Message message: 原生消息详细信息。头+体
     * 2. T<发送的消息的类型> String s
     * 3. Channel channel：当前传输数据的通道
     * <p>
     * Queue：可以很多人都来监听。只要收到消息，队列删除消息，而且只能有一个人收到此消息
     * 场景：
     * 1）订单服务启动多个；同一个消息，只能有一个客户端收到
     * 2) 只有一个消息完全处理完，方法运行结束，我们就可以接收到下一个消息
     */
    @RabbitHandler
    public void receiveMessage(Message message, String s, AMQP.Channel channel) {
        System.out.println("接收到消息。。。内容：" + message + "===>内容：" + s);
    }

}
```

# 可靠投递-发送端确认

![image-20210815200300593](/rabbitmq_img/image-20210815200300593.png)

![image-20210815204413109](/rabbitmq_img/image-20210815204413109.png)

![image-20210815214727474](/rabbitmq_img/image-20210815214727474.png)

 ```properties
# 开启发送端确认
spring.rabbitmq.publisher-confirms=true
# 开启发送端消息抵达队列的确认
spring.rabbitmq.publisher-returns=true
# 只要抵达队列，以异步方式优先回调我们这个returnconfirm
spring.rabbitmq.template.mandatory=true
 ```

MyRabbitConfig.java

```java
package com.lzd.xmall.order.config;

import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.annotation.PostConstruct;

@Configuration
public class MyRabbitConfig {

    @Autowired
    RabbitTemplate rabbitTemplate;

    @Bean
    public MessageConverter messageConverter() {
        return new Jackson2JsonMessageConverter();
    }

    /**
     * 定制RabbitTemplate
     * 1.服务收到消息就回调
     *      1.spring.rabbitmq.publisher-confirms=true
     *      2.设置确认回调ConfirmCallback
     * 2.消息正确抵达队列进行回调
     *      spring.rabbitmq.publisher-returns=true
     *      spring.rabbitmq.template.mandatory=true
     */
    @PostConstruct
    public void initRabbitTemplate() {
        // 设置确认回调
        rabbitTemplate.setConfirmCallback(new RabbitTemplate.ConfirmCallback() {
            /**
             * @param correlationData 当前消息的唯一关联数据（这个是消息的唯一id）
             * @param ack 消息是否成功收到
             * @param cause 失败的原因
             */
            @Override
            public void confirm(CorrelationData correlationData, boolean ack, String cause) {
                System.out.println("confirm...correlationData[" + correlationData + "]===>ack[" + ack + "]===>cause" + cause + "]");
            }
        });

        // 设置消息抵达队列的确认回调，
        rabbitTemplate.setReturnCallback(new RabbitTemplate.ReturnCallback() {
            /**
             * 只要消息没有投递给指定的队列，就触发这个失败回调
             * @param message 投递失败的消息详细信息
             * @param replyCode 回复的状态码
             * @param replyText 回复的文本内容
             * @param exchange 当时这个消息发送给哪个交换机
             * @param routingKey 当时这个消息用哪个路由键
             */
            @Override
            public void returnedMessage(Message message, int replyCode, String replyText, String exchange, String routingKey) {
                System.out.println("message: " + message + " replyCode: " + replyCode + " replyText: " + replyText + " exchange: " + exchange + " routingKey: " + routingKey);
            }
        });
    }
}
```

# 可靠投递-消费端确认

![image-20210819224738246](/rabbitmq_img/image-20210819224738246.png)

application.properties

```properties
spring.rabbitmq.host=192.168.8.136
spring.rabbitmq.port=5672
spring.rabbitmq.virtual-host=/

# 开启发送端确认
spring.rabbitmq.publisher-confirms=true
# 开启发送端消息抵达队列的确认
spring.rabbitmq.publisher-returns=true
# 只要抵达队列，以异步方式优先回调我们这个returnconfirm
spring.rabbitmq.template.mandatory=true

# 手动ack消息
spring.rabbitmq.listener.simple.acknowledge-mode=manual
```

MyRabbitConfig.java

```java
package com.lzd.xmall.order.config;

import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.annotation.PostConstruct;

@Configuration
public class MyRabbitConfig {

    @Autowired
    RabbitTemplate rabbitTemplate;

    @Bean
    public MessageConverter messageConverter() {
        return new Jackson2JsonMessageConverter();
    }

    /**
     * 定制RabbitTemplate
     * 1.服务收到消息就回调
     *      1.spring.rabbitmq.publisher-confirms=true
     *      2.设置确认回调ConfirmCallback
     * 2.消息正确抵达队列进行回调
     *      1.spring.rabbitmq.publisher-returns=true
     *        spring.rabbitmq.template.mandatory=true
     *      2.设置确认回调ReturnCallback
     * 3.消费端确认（保证每个消息被正确消费，此时才可以broker删除这个消息）
     *      spring.rabbitmq.listener.simple.acknowledge-mode=manual
     *      1.默认是自动确认的，只要消息接收到，客户端会自动确认，服务端就会移除这个消息
     *      问题：
     *          我们收到很多消息，自动回复给服务器ack，只有一个消息处理成功，宕机了。发生消息丢失
     *          解决：消费者手动确认模式。
     *               只要我们没有明确告诉MQ，货物被签收。没有ack，消息就一直是unacked状态。即使Consumer宕机，消息也不会丢失，会重新变为Ready状态，下一次有新的
     *               Consumer连接进来就发给他
     *      2.如何签收
     *          channel.basicAck(deliveryTag, false); 签收；业务成功完成就应该签收
     *          channel.basicNack(deliveryTag, false, true); 拒签；业务失败，拒签
     */
    @PostConstruct
    public void initRabbitTemplate() {
        // 设置确认回调
        rabbitTemplate.setConfirmCallback(new RabbitTemplate.ConfirmCallback() {
            /**
             * @param correlationData 当前消息的唯一关联数据（这个是消息的唯一id）
             * @param ack 消息是否成功收到
             * @param cause 失败的原因
             */
            @Override
            public void confirm(CorrelationData correlationData, boolean ack, String cause) {
                System.out.println("confirm...correlationData[" + correlationData + "]===>ack[" + ack + "]===>cause" + cause + "]");
            }
        });

        // 设置消息抵达队列的确认回调，
        rabbitTemplate.setReturnCallback(new RabbitTemplate.ReturnCallback() {
            /**
             * 只要消息没有投递给指定的队列，就触发这个失败回调
             * @param message 投递失败的消息详细信息
             * @param replyCode 回复的状态码
             * @param replyText 回复的文本内容
             * @param exchange 当时这个消息发送给哪个交换机
             * @param routingKey 当时这个消息用哪个路由键
             */
            @Override
            public void returnedMessage(Message message, int replyCode, String replyText, String exchange, String routingKey) {
                System.out.println("message: " + message + " replyCode: " + replyCode + " replyText: " + replyText + " exchange: " + exchange + " routingKey: " + routingKey);
            }
        });
    }
}
```

OrderItemServiceImpl.java

```java
package com.lzd.xmall.order.service.impl;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.lzd.common.utils.PageUtils;
import com.lzd.common.utils.Query;
import com.lzd.xmall.order.dao.OrderItemDao;
import com.lzd.xmall.order.entity.OrderItemEntity;
import com.lzd.xmall.order.service.OrderItemService;
import com.rabbitmq.client.Channel;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.Map;


@Service("orderItemService")
@RabbitListener(queues = {"hello-java-queue"})
public class OrderItemServiceImpl extends ServiceImpl<OrderItemDao, OrderItemEntity> implements OrderItemService {

    @Override
    public PageUtils queryPage(Map<String, Object> params) {
        IPage<OrderItemEntity> page = this.page(
                new Query<OrderItemEntity>().getPage(params),
                new QueryWrapper<>()
        );

        return new PageUtils(page);
    }

    /**
     * queues: 声明需要监听的所有队列
     * <p>
     * class org.springframework.amqp.core.Message
     * <p>
     * 参数可以写一下类型
     * 1. Message message: 原生消息详细信息。头+体
     * 2. T<发送的消息的类型> String s
     * 3. Channel channel：当前传输数据的通道
     * <p>
     * Queue：可以很多人都来监听。只要收到消息，队列删除消息，而且只能有一个人收到此消息
     * 场景：
     * 1）订单服务启动多个；同一个消息，只能有一个客户端收到
     * 2) 只有一个消息完全处理完，方法运行结束，我们就可以接收到下一个消息
     */
    @RabbitHandler
    public void receiveMessage(Message message, String s, Channel channel) {
        System.out.println("接收到消息。。。内容：" + message + "===>内容：" + s);
        // channel内按顺序自增的
        long deliveryTag = message.getMessageProperties().getDeliveryTag();
        System.out.println("deliveryTag==>" + deliveryTag);

        // 签收货物，非批量模式
        try {
            if (deliveryTag % 2 == 0) {
                channel.basicAck(deliveryTag, false);
                System.out.println("签收了货物: " + deliveryTag);
            } else {
                // 退货 requeue=false 丢弃 requeue=true 发回服务器，服务器重新入队
                channel.basicNack(deliveryTag, false, true);
                System.out.println("没有签收了货物: " + deliveryTag);
            }
        } catch (IOException e) {
            // 网络中断
            e.printStackTrace();
        }
    }

}
```







