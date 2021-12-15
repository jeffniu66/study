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

application.properties

```properties
spring.cloud.sentinel.transport.dashboard=localhost:8333
spring.cloud.sentinel.transport.port=8719
```

## 2.4 在控制台调整所有的流控规则参数【默认所有的流控设置保存在内存中，重启失效】

# 3. 实时监控页面图形化

每一个微服务都导入actuator；并配和management.endpoints.web.exposure.include=*

# 4. 自定义sentinel流控返回数据

```java
package com.lzd.xmall.seckill.config;

import com.alibaba.csp.sentinel.adapter.servlet.callback.UrlBlockHandler;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.alibaba.fastjson.JSON;
import com.lzd.common.exception.BizCodeEnum;
import com.lzd.common.utils.R;
import org.springframework.context.annotation.Configuration;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Configuration
public class SecKillSentinelConfig implements UrlBlockHandler {

    @Override
    public void blocked(HttpServletRequest request, HttpServletResponse response, BlockException ex) throws IOException {
        R r = R.error(BizCodeEnum.SECKILL_EXCEPTION.getCode(), BizCodeEnum.SECKILL_EXCEPTION.getMsg());
        response.setContentType("application/json;charset=utf-8");
        response.getWriter().write(JSON.toJSONString(r));
    }
}
```

