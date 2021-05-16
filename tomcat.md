---
typora-root-url: ../study
typora-copy-images-to: ./tomcat_img
---

# Tomcat

<font color=red>Tomcat是一个servlet容器</font>

## Tomcat部署应用的几种方式

1.war包形式

2.jar包形式

3.整个项目文件夹

4.描述符方式

## Tomcat切换IO模型

<font color=red>修改server.xml文件，Connector的protocol属性，指定具体的类路径，xxx.Http11NioProtocol</font>

## 双亲委派机制

<font color=red>可以通过重写loadClass()方法打破双亲委派机制</font>

<font color=red>TCP-操作系统</font>

<font color=red>HTTP-浏览器</font>

## 热部署与热加载

二者监听的东西不一样

热部署只会监听项目目录下的文件与文件夹是否发生变化，默认开启

热加载会监听WEB-INF下的文件，默认不开启，开启热加载，Context节点下配置reloadable=true

# WebSocket

## WebSocket介绍

![image-20210509151846633](/tomcat_img/image-20210509151846633.png)

## Tomcat的Websocket

![image-20210509155644810](/tomcat_img/image-20210509155644810.png)

![image-20210509160121058](/tomcat_img/image-20210509160121058.png)

