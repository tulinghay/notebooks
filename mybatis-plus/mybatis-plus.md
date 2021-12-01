### 整合mybatis-plus

> 1.导入依赖

```xml
<!--        mybatis-plus-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.2.0</version>
</dependency>
```

> 2.配置数据源

```yml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://59.78.194.45:3306/gulimall_pms
    driver-class-name: com.mysql.jdbc.Driver
```

> 3.在启动类中配置MapperScan，以及在yml配置文件中配置mapp.xml位置

```java
@MapperScan("com.atguigu.gulimall.product.dao")
```

```yml
mybatis-plus:
  mapper-locations: classpath:/mapper/**/*.xml  #如果不配置则默认这路径
```







### 配置详解

```yml
#数据源配置
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://59.78.194.45:3306/gulimall_pms
    driver-class-name: com.mysql.jdbc.Driver

mybatis-plus:
  #配置mapper.xml路径，如果不设置默认是classpath*:/mapper/**/*.xml
  #其中classpath代表的是当前项目下，classpath*还包括依赖项目下
  mapper-locations: classpath:/mapper/**/*.xml
  
  global-config:
    db-config:
      #配置表主键自增，对加了@TableId注解的生效
      id-type: auto
```

