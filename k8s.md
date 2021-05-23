---
typora-root-url: ../study
typora-copy-images-to: ./k8s_img
---

# k8s

## 简介

kubernetes简称k8s。是用于自动部署，扩展和管理容器化应用程序的开源系统。
中文官网：https://kubernetes.io/Zh/
中文社区：https://www.kubernetes.org.cn/
官方文档：https://kubernetes.io/zh/docs/home/
社区文档：https://docs.kubernetes.org.cn/

调度

![image-20210225225228410](/k8s_img/image-20210225225228410.png)

![image-20210225225436863](/k8s_img/image-20210225225436863.png)

## 架构

### 整体主从方式

![image-20210225230027512](/k8s_img/image-20210225230027512.png)

![image-20210225230106399](/k8s_img/image-20210225230106399.png)

### Master节点架构

![image-20210227120236064](/k8s_img/image-20210227120236064.png)

![image-20210227120741264](/k8s_img/image-20210227120741264.png)

![image-20210227121018488](/k8s_img/image-20210227121018488.png)

### Node节点架构

![image-20210227121128167](/k8s_img/image-20210227121128167.png)

![image-20210227121453012](/k8s_img/image-20210227121453012.png)

## 概念

![image-20210227122750760](/k8s_img/image-20210227122750760.png)

![image-20210227123957573](/k8s_img/image-20210227123957573.png)

![image-20210227124137347](/k8s_img/image-20210227124137347.png)

![image-20210227124503453](/k8s_img/image-20210227124503453.png)

![image-20210227135425038](/k8s_img/image-20210227135425038.png)

![image-20210227140251359](/k8s_img/image-20210227140251359.png)

![image-20210227140321802](/k8s_img/image-20210227140321802.png)

![image-20210227140522918](/k8s_img/image-20210227140522918.png)

![image-20210227140600007](/k8s_img/image-20210227140600007.png)

## 快速体验

### 1. 安装minikube

https://github.com/kubernetes/minikube/releases
下载minikuber-windows-amd64.exe 改名为minikube.exe
打开virtualBox，打开cmd
运行
minikube start --vm-driver=virtualbox --registry-mirror=https://registry.docker-cn.com
等待20分钟即可。