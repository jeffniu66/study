---
typora-copy-images-to: ./kafka_img
typora-root-url: ./kafka_img
---

# 面试题

<font color=red>1.Kafka中的ISR(InSyncRepli)、OSR(OutSyncRepli)、AR(AllRepli)代表什么？</font>

<font color=gree>ISR : 速率和leader相差低于10秒的follower的集合</font>
<font color=gree>OSR : 速率和leader相差大于10秒的follower</font>
<font color=gree>AR : 所有分区的follower</font>

<font color=red>2.Kafka中的HW、LEO等分别代表什么？</font>

<font color=gree>HW: (High Water)，消费者能见到的最大的offset，ISR队列中最小的LEO</font>

<font color=gree>LEO: 每个分区副本最大的offset</font>

<font color=red>3.Kafka中怎么体现消息顺序性的？</font>

<font color=gree>区内有序，每个分区内，每条消息都有offset，所以只能同一分区内有序，不同的分区无法做到消息顺序性</font>

<font color=red>4.Kafka中的分区器、序列化器、拦截器是否了解？它们之间的处理顺序是什么？</font>

<font color=gree>拦截器>序列化器>分区器</font>

<font color=red>5.Kafka生产者客户端的整体结构是什么样子的？使用了几个线程来处理？分别是什么？</font>

![image-20210411095414410](/image-20210411095414410.png)

