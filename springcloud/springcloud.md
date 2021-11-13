

### Eureka

api导包：

```xml

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>javax.persistence</groupId>
            <artifactId>javax.persistence-api</artifactId>
            <version>2.2</version>
        </dependency>
    </dependencies>

```

deptcontroller导包

```xml

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>javax.persistence</groupId>
            <artifactId>javax.persistence-api</artifactId>
            <version>2.2</version>
        </dependency>
    </dependencies>

```

consumer使用getForObject导包

```xml

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>javax.persistence</groupId>
            <artifactId>javax.persistence-api</artifactId>
            <version>2.2</version>
        </dependency>
    </dependencies>
```

eureka导包

```xml
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>

    </dependencies>
```

eureka配置文件，其中eureka的启动类需要添加注解@EnableEurekaServer

```yml
server:
  port: 8082
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false #为false则表示自己为注册中心
    service-url: #监控页面
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

CAP原则：一致性，可用性、分区容错性，一般只能满足其中两个角色，eureka满足可用性和分区容错性，zookeeper满足一致性和分区容错性

### eureka集群

集群配置文件

```yml
server:
  port: 8083
eureka:
  instance:
    hostname: eureka8083.com
  client:
    register-with-eureka: false
    fetch-registry: false #为false则表示自己为注册中心
    service-url: #监控页面
      defaultZone: http://eureka8082.com:8082/eureka/,http://eureka8084.com:8084/eureka/
      #一共三个集群82,83,84,三个集群分别配置另外两个的defaultZone
```



### Ribbon 负载均衡

Ribbon可提供负载均衡的效果

常见的负载均衡有Nginx、Lvs

@EnableDiscoveryClient



需要导入包，ribbon

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
            <version>3.0.4</version>

            <exclusions>
                <exclusion>
                    <groupId>javax.servlet</groupId>
                    <artifactId>servlet-api</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>com.google.code.gson</groupId>
                    <artifactId>gson</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-ribbon -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
            <version>2.2.9.RELEASE</version>
        </dependency>

```

消费者配置文件，消费者的启动类需要添加@EnableEurekaClient注解

```yml
server:
  port: 8082
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false  #因为是消费者，所以不向eureka注册自己 
    service-url: #监控页面
      defaultZone: http://eureka8082.com:8082/eureka/,http://eureka8083.com:8083/eureka/,http://eureka8084.com:8084/eureka/
#配置集群
```

ConfigBean中对getRestTemplate()方法配置注解@LoadBalanced实现Ribbon，

这里对消费者使用getForObject的url用http://服务名   替换掉 http://localhost

![image-20211104154812488](asset/image-20211104154812488.png)

![image-20211104154751643](asset/image-20211104154751643.png)

#### 自定义Ribbon负载均衡算法

![image-20211105101128815](asset/image-20211105101128815.png)

算法实现，这里直接使用已经有的RandomRule()算法，可以查看各种策略的代码，自己重新定义

![image-20211105101219533](asset/image-20211105101219533.png)

注意自己定义的类放的包位置不能和启动类同级，如下：

![image-20211105102441940](asset/image-20211105102441940.png)

![image-20211105102826678](asset/image-20211105102826678.png)

### Feign负载均衡——针对接口的负载均衡

导入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-netflix-core</artifactId>
    <version>1.4.7.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

整个访问流程是在api工程中定义Feign的访问服务id，创建对应接口。接着在消费工程中，调用对应接口实现服务id和对应负载的访问。

创建service接口

![image-20211105110537300](asset/image-20211105110537300.png)

创建对应的访问Controller

![image-20211105110039948](asset/image-20211105110039948.png)

Feign默认使用的是Ribbon的轮询负载算法

![image-20211105105700046](asset/image-20211105105700046.png)

添加扫描注解

![image-20211105110659093](asset/image-20211105110659093.png) 

### Hystrix

- 服务降级
- 服务熔断
- 服务限流
- 接近实时的监控
- ……

熔断机制注解：@HystrixCommand

导入包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
    <version>2.2.9.RELEASE</version>
</dependency>
```

配置执行失败后的方法

![image-20211105122624084](asset/image-20211105122624084.png)

开启熔断的支持

![image-20211105122745627](asset/image-20211105122745627.png)

### 服务降级

服务熔断：服务端   某个服务超时或异常，引起熔断

服务降级：客户端   从整体网站请求负载考虑，当某个服务熔断或者关闭之后，服务将不再调用，整体的服务水平下降，但是可用 

创建类

![image-20211106090239036](asset/image-20211106090239036.png)

配置类

![image-20211106090320769](asset/image-20211106090320769.png)

yml配置文件配置

![image-20211106090447390](asset/image-20211106090447390.png)

#### Hystrix dashboard

导入依赖

![image-20211106104308661](asset/image-20211106104308661.png)

端口配置

![image-20211106104320636](asset/image-20211106104320636.png)

开启注解

![image-20211106104337109](asset/image-20211106104337109.png)

服务添加依赖

![image-20211106104405859](asset/image-20211106104405859.png)

对服务添加

![image-20211106104433154](asset/image-20211106104433154.png)



### Zuul路由网关

导入依赖

![image-20211106111704667](asset/image-20211106111704667.png)

配置路由

![image-20211106111606982](asset/image-20211106111606982.png)

启动类添加注解

![image-20211106111733793](asset/image-20211106111733793.png)



## spring cloud config







## 总结

![image-20211106212549565](asset/image-20211106212549565.png)

