---
typora-copy-images-to: ./sentinel_img
typora-root-url: ../study
---

# 1. Hystrix与Sentinel

![image-20210919150305499](/sentinel_img/image-20210919150305499.png)

# 2. SpringBoot整合Sentinel

## 2.1 引入jar包

```xml
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```



## 2.2 下载Sentinel的控制台

使用命令，默认端口8080

```shell
java -jar sentinel-dashboard-1.6.3.jar
```

指定具体端口

```shell
java -jar sentinel-dashboard-1.6.3.jar --server.port=8333
```

## 2.3 配置sentinel控制台地址信息

