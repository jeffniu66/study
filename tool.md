---
typora-copy-images-to: tool_img
typora-root-url: ./
---

# 1. idea

记录一次eclipse项目，在intelliJ idea里tomcat一直不能正常启动的问题

1. 项目的这2处一定要设置成自己项目的web.xml文件路径和web资源存放的路径

   ![image-20210927194051002](/tool_img/image-20210927194051002.png)

   2.部署项目时要设置一下Application context

   ![image-20210927194212033](/tool_img/image-20210927194212033.png)

   3.项目资源这里也记得设置一下，否则不能找到配置文件，启动不正常

   ![image-20210927194326835](/tool_img/image-20210927194326835.png)

   4.如果不在web.xml里设置类为一个监听器，类不会被加载，static代码块就不会执行

   ![image-20210927194517386](/tool_img/image-20210927194517386.png)


# 2. maven

## 2.1 maven导包不成功

当各种reimport还是失效之后，只能自己把jar包下载下来，然后使用命令安装，一个spring-sessio-data-redis的包折腾了2天，记录一下

```shell
mvn install:install-file -Dfile=/Users/liuzhidong/Documents/spring-session-data-redis-2.3.1.RELEASE.jar -DgroupId=org.springframework.session -DartifactId=spring-session-data-redis -Dversion=2.3.1.RELEASE -Dpackaging=jar
```



