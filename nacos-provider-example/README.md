### 1、启动nacos注册中心
nacos可视化页面地址 http://localhost:8848/nacos/index.html
这时服务列表为空，因为我们还没有服务注册。
### 2、启动服务
项目地址 https://github.com/DespairYoke/nacos-examples/tree/master/nacos-provider-example
* maven依赖
```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
            <version>0.2.1.RELEASE</version>
        </dependency>
```
经实验发现该版本不能使用版本控制，可能是还没彻底入住springcloud影响的原因。
* application.properties
```java
server.port=18082
spring.application.name=service-provider
spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848
management.endpoints.web.exposure.include=*
```
`management.endpoints.web.exposure.include=*`是对外暴露该服务信息
### 3、查看效果
* 访问controller接口 http://127.0.0.1:18082/echo/world
返回 hello Nacos Discovery world

* 查看可视化界面![server-provider](https://upload-images.jianshu.io/upload_images/15204062-9fe21595bc4ef79a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到服务注册成功
* 服务详情中可以看到服务随机分配的ip
![服务详情](https://upload-images.jianshu.io/upload_images/15204062-05013283f2f5eb36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
