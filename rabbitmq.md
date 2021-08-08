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





