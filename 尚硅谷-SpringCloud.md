---
title: 尚硅谷-SpringCloud
date: 2023-04-19 09:57:43
tags: SpringCloud
---





# 1 基础入门-创建项目

## 1.1 简介

教程地址

SpringCloud 是微服务一站式服务解决方案，微服务全家桶。它是微服务开发的主流[技术栈](https://so.csdn.net/so/search?q=技术栈&spm=1001.2101.3001.7020)。它采用了英国地铁名称作为版本号，而非数字版本号。

SpringCloud 和 springCloud Alibaba 目前是最主流的微服务框架组合。

![1597213385700](尚硅谷-SpringCloud/cd4957eee96b1eb47276c47f88111fab.png)

什么是[微服务](https://so.csdn.net/so/search?q=微服务&spm=1001.2101.3001.7020)架构：

> In short, the **microservice architectural style** is an approach to developing a single application as a **suite of small services**, each **running in its own process** and communicating with **lightweight** mechanisms, often an **HTTP** resource API. These services are built around **business capabilities** and **independently deployable** by fully **automated deployment** machinery. There is a bare minimum of centralized management of these services, which may be **written in different programming languages** and use different data storage technologies.——[James Lewis and Martin Fowler (2014)](https://martinfowler.com/articles/microservices.html)

- 微服务是一种架构风格
- 一个应用拆分为一组小型服务
- 每个服务运行在自己的进程内，也就是可独立部署和升级
- 服务之间使用轻量级HTTP交互
- 服务围绕业务功能拆分
- 可以由全自动部署机制独立部署
- 去中心化，服务自治。服务可以使用不同的语言、不同的存储技术



## 1.2 springcloud与springboot版本的对应关系

版本选择：

> 选用 springboot 和 springCloud 版本有约束，不按照它的约束会有冲突。

[官方对应关系](http://start.spring.io/actuator/info)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70.png)

## 1.3 关于Cloud各种组件的停更/升级/替换

由于升级和停更引发的技术变更：（圈起来的是阳哥推荐的技术）

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1681906480964-3.png)

## 1.4 微服务架构编码构建

### 1.4.1 创建父工程

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.atguigu.springcloud</groupId>
    <artifactId>cloud2020</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>Maven</name>

    <modules>
        <module>cloud-provider-payment8001</module>
        <module>cloud-consumer-order80</module>
        <module>cloud-consumerzk-order80</module>
        <module>cloud-api-commons</module>
        <module>cloud-eureka-server7001</module>
        <module>cloud-eureka-server7002</module>
        <module>cloud-provider-payment8002</module>
        <module>cloud-provider-payment8004</module>
        <module>cloud-consumerzk-order80</module>
        <module>cloud-providerconsul-payment8006</module>
        <module>cloud-consumerconsul-order80</module>
        <module>cloud-consumer-feign-order80</module>
        <module>cloud-provider-hystrix-payment8001</module>
        <module>cloud-consumer-feign-hystrix-order80</module>
        <module>cloud-consumer-hystrix-dashboard9001</module>
        <module>cloud-gateway-gateway9527</module>
        <module>cloud-config-center-3344</module>
        <module>cloud-config-client-3355</module>
        <module>cloud-config-client-3366</module>
        <module>cloud-stream-rabbitmq-provider8801</module>
        <module>cloud-stream-rabbitmq-consumer8802</module>
        <module>cloud-stream-rabbitmq-consumer8804</module>
        <module>cloud-stream-rabbitmq-consumer8803</module>
        <module>cloudalibaba-provider-payment9001</module>
        <module>cloudalibaba-provider-payment9002</module>
        <module>cloudalibaba-consumer-nacos-order83</module>
        <module>cloudalibaba-config-nacos-client3377</module>
        <module>cloudalibaba-sentinel-service8401</module>
        <module>cloudalibaba-provider-payment9003</module>
        <module>cloudalibaba-provider-payment9004</module>
        <module>cloudalibaba-consumer-nacos-order84</module>
        <module>seata-order-service2001</module>
        <module>seata-order-service2002</module>
        <module>seata-account-service2003</module>
    </modules>

    <!-- 统一管理jar包版本 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
        <mysql.version>5.1.47</mysql.version>
        <druid.version>1.1.16</druid.version>
        <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
    </properties>

    <!-- 1、只是声明依赖，并不实际引入，子项目按需声明使用的依赖 -->
    <!-- 2、子项目可以继承父项目的 version 和 scope -->
    <!-- 3、子项目若指定了 version 和 scope，以子项目为准 -->
    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.version}</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
                <optional>true</optional>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

maven中dependencyManagement标签：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70.png)
子项目中，如果不指定，默认和父项目dependencyManagement标签中的版本一致，并且父项目dependencyManagement标签只是规定了版本号，具体引入依赖还是子项目引入。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040530864-1.png)
maven中跳过单元测试
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040530865-2.png)
父工程创建完成后执行`mvn:install`将父工程发布到仓库方便子工程继承。

### 1.4.2 Rest微服务工程构建

最开始的订单模块

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040560070-9.png)

#### 1.4.2.1 微服务提供者支付module模块cloud-provider-payment8001

pom

```xml
    <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud2020</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8001</artifactId>

<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.20</version>
            <!--子工程写了版本号，就使用子工程的版本号，如果没写版本,找父工程中规定的版本号-->
        </dependency>
        <!--mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

yml

```yaml
#微服务建议一定要写服务端口号和微服务名称
server:
  #端口号
  port: 8001

spring:
  application:
    #微服务名称
    name: cloud-payment-service
  #数据库配置
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    #mysql5.x的没有cj
    driver-class-name: com.mysql.cj.jdbc.Driver
    #记得先创建数据库
    url: jdbc:mysql://localhost:3306/db2020?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456

#mybatis配置
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.angenin.springcloud.entities  #所有Entity别名类所在包

```

业务类

1. 建表SQL

   ```sql
   CREATE TABLE `payment`(
   	`id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'ID',
   	`serial` VARCHAR(200) DEFAULT '',
   	PRIMARY KEY(`id`)
   )ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
   
   INSERT INTO payment(`serial`)VALUES("张三");
   1234567
   ```

2. entities
   在springcloud包下新建实体类entities.Payment

   ```java
   //这三个注解是lombok的，除了导入依赖，idea还需要安装插件（具体操作问度娘）
   @Data   //set/get方法
   @AllArgsConstructor //有参构造器
   @NoArgsConstructor  //无参构造器
   public class Payment implements Serializable {
       private long id;
       private String serial;
   }
   12345678
   ```

   在entities包下新建CommonResult（json封装体，传给前端的）

   ```java
   //返回给前端的通用json数据串
   @Data   //set/get方法
   @AllArgsConstructor //有参构造器
   @NoArgsConstructor  //无参构造器
   public class CommonResult<T> {
       private Integer code;
       private String message;
       private T data; //泛型，对应类型的json数据
   
       //自定义两个参数的构造方法
       public CommonResult(Integer code, String message){
           this(code, message, null);
       }
   }
   ```

3. dao
   在springcloud包下新建Dao.PaymentDao接口

   ```java
   @Mapper
   public interface PaymentDao {
   
       //增
       int create(Payment payment);
   
       //改     加上@Param注解，mapper中就可以采用#{}的方式把@Param注解括号内的参数进行引用
       Payment getPaymentById(@Param("id") Long id);
       
       //这里用增和改进行演示，有兴趣的可以自己加其他的方法
   }
   ```

4. mapper
   在resources目录下新建mapper目录，然后新建PaymentMapper.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.angenin.springcloud.Dao.PaymentDao">
   
       <resultMap id="BaseResultMap" type="com.angenin.springcloud.entities.Payment">
           <id column="id" property="id" jdbcType="BIGINT"/>
           <id column="serial" property="serial" jdbcType="VARCHAR"/>
       </resultMap>
       
   
       <!--  增  -->
       <!--  Payment标红了不用管，因为我们已经在yml文件中指定了Payment的位置了  -->
       <insert id="create" parameterType="Payment" useGeneratedKeys="true" keyProperty="id">
           insert into payment(serial)values(#{serial});
       </insert>
   
       <!--  改  -->
       <!--返回用resultMap，防止命名不规范-->
       <select id="getPaymentById" parameterType="Long" resultMap="BaseResultMap">
           select * from payment where id=#{id};
       </select>
   </mapper>
   
   ```

5. service
   在springcloud包下新建service.PaymentService接口

   ```java
   public interface PaymentService {
   
       int create(Payment payment);
   
       Payment getPaymentById(@Param("id") Long id);
   
   }
   ```

   在service包下新建impl.PaymentServiceIpml实现类

   ```java
   @Service
   public class PaymentServiceIpml implements PaymentService {
   
       @Resource   //@Autowired也可以
       private PaymentDao paymentDao;
   
       public int create(Payment payment){
           return paymentDao.create(payment);
       }
   
       public Payment getPaymentById(Long id){
           return paymentDao.getPaymentById(id);
       }
   
   }
   ```

6. controller
   在springcloud包下新建controller.PaymentController

   ```java
   @RestController
   @Slf4j  //日志
   public class PaymentController {
   
       @Resource
       private PaymentService paymentService;
   
       //前后端分离，所以不能直接返回对象，数据要先经过CommonResult封装再返回
       @PostMapping("/payment/create")
       public CommonResult create(@RequestBody Payment payment){
           int result = paymentService.create(payment);
           log.info("******插入的数据为：" + payment);
           log.info("******插入结果：" + result);
   
           if(result > 0){
               //插入成功
               return new CommonResult(200, "插入数据库成功", result);
           }else{
               return new CommonResult(444, "插入数据库失败");
           }
       }
   
   
       @GetMapping("/payment/get/{id}")
       public CommonResult getPaymentById(@PathVariable("id") Long id){
           Payment payment = paymentService.getPaymentById(id);
           log.info("******查询结果：" + payment);
   
           if(payment != null){
               //查询成功
               return new CommonResult(200, "查询成功", payment);
           }else{
               return new CommonResult(444, "没有对应记录，查询ID：" + id);
           }
       }
   
   }
   ```

启动项目
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040794540-12.png)
浏览器输入`http://localhost:8001/payment/get/1`，查询成功。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040794541-13.png)
因为浏览器一般不支持直接发送post请求，所以，需要使用工具进行测试。（我这里用的是Postman）
重新测试查询，没问题。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040794541-14.png)
输入`http://localhost:8001/payment/create`发送post请求，往数据库中插入一条数据，需要把数据写到body中。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040794541-15.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682040794541-16.png)

#### 1.4.2.2 微服务消费者订单module模块cloud-consumer-order80

pom (同上面一样继承父工程)

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>

```

yml

```yaml
#访问一个网站时，默认是80端口，给用户80端口，用户就可以不用加端口直接访问页面
server:
  port: 80
```

业务

1. 复制cloud-provider-payment8001项目里的entities（里面2个实体类）到本项目（cloud-consumer-order80）的springcloud包下。（在后面的工程重构，阳哥会把实体类抽取出来）

2. 在springcloud包下新建config.ApplicationContextConfig

   ```java
   @Configuration
   public class ApplicationContextConfig {
   
       //往容器中添加一个RestTemplate
       //RestTemplate提供了多种便捷访问远程http访问的方法
       @Bean
       public RestTemplate restTemplate(){
           return new RestTemplate();
       }
   
   }
   ```

3. 在springcloud包下新建controller.OrderController

   ```java
   @RestController
   @Slf4j
   public class OrderController {
   
       public static final String PAYMENT_URL = "http://localhost:8001";
   
       @Resource
       private RestTemplate restTemplate;
   
       //因为浏览器只支持get请求，为了方便这里就用get
       @GetMapping("/consumer/payment/create")
       public CommonResult<Payment> create(Payment payment){
           log.info("********插入的数据：" + payment);
           //postForObject分别有三个参数：请求地址，请求参数，返回的对象类型
           return restTemplate.postForObject(PAYMENT_URL + "/payment/create", payment, CommonResult.class);
       }
   
       @GetMapping("/consumer/payment/get/{id}")
       public CommonResult<Payment> getPayment(@PathVariable("id") Long id){
           log.info("********查询的id：" + id);
           //getForObject两个参数：请求地址，返回的对象类型
           return restTemplate.getForObject(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
       }
   }
   123456789101112131415161718192021222324
   ```



启动两个项目进行测试，两个都启动后，右下角会弹出个services提示，点击show。
然后会把运行的项目合并在一起显示：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041031226-27.png)
在浏览器中输入`http://localhost/consumer/payment/get/1`成功查询到数据。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041031228-28.png)
在浏览器中输入`http://localhost/consumer/payment/create?serial=王五`插入一条数据。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041031228-29.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041031228-30.png)

#### 1.4.2.3 工程重构

观察问题：系统中有重复部分，重构。

1. 新建新工程：cloud-api-commons

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041110053-39.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041110053-40.png)

2. 改pom

添加依赖：

```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <!--   一个Java工具包     -->
       <dependency>
           <groupId>cn.hutool</groupId>
           <artifactId>hutool-all</artifactId>
           <version>5.1.0</version>
       </dependency>
   </dependencies>
```

3. 拷贝entities到本项目中（路径需要一样，先在本项目中建包，然后在拷贝）

4. 对本项目进行打包

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041110053-41.png)

5. 删除另外两个项目的entities包。

6. 然后给两个项目引入打包后项目的坐标。

```xml
       <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
       <dependency>
           <groupId>com.angenin.springcloud</groupId>
           <artifactId>cloud-api-commons</artifactId>
           <version>${project.version}</version>
       </dependency>
```

1. 重新测试
   `http://localhost/consumer/payment/get/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041110053-42.png)
   `http://localhost/consumer/payment/create?serial=赵六`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041110053-43.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041110054-44.png)

# 2 初级部分，注册中心【Eureka、Zookeeper和Consul】

## 2.1 Eureka服务注册与发现

### 2.1.1 Eureka基础知识

#### 服务治理

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-57.png)

#### 服务注册

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-58.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-59.png)

#### Eureka两组件

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-60.png)

### 2.1.2 单机Eureka构建步骤

#### 1 IDEA生成eurekaServer端服务注册中心

1. 建module

cloud-eureka-server7001

2. 改pom

server端依赖对比：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-61.png)
在pom中添加

```xml
  <dependencies>
      <!-- eureka-server -->
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
      </dependency>
      <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
      <dependency>
          <groupId>com.angenin.springcloud</groupId>
          <artifactId>cloud-api-commons</artifactId>
          <version>${project.version}</version>
      </dependency>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
      <!--监控-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-actuator</artifactId>
      </dependency>
      <!-- 一般通用配置 -->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-devtools</artifactId>
          <scope>runtime</scope>
          <optional>true</optional>
      </dependency>
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <optional>true</optional>
      </dependency>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
          <scope>test</scope>
      </dependency>
  </dependencies>
```

3. 写yml

在resources目录下新建application.yml文件

```yml
server:
  port: 7001

eureka:
  instance:
    hostname: localhost  #eureka服务端的实例名称
  client:
    #false表示不向注册中心注册自己（想注册也可以，不过没必要）
    register-with-eureka: false
    #false表示自己端就是注册中心，职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与eurekaServer交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

4. 主启动

在java包下新建com.angenin.springcloud.EurekaMain7001

```java
@EnableEurekaServer //表示此项目是eureka的服务注册中心
@SpringBootApplication
public class EurekaMain7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7001.class, args);
    }
}
```

5. 测试

启动项目，在浏览器输入`http://localhost:7001/`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-62.png)

#### 2 将EurekaClient端8001注册进EurekaServer成为服务提供者provider

client端依赖对比
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-63.png)

1. 引入依赖

   ```xml
    <!-- eureka-client -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
   12345
   ```

2. 在yml文件中添加

   ```yml
   eureka:
     client:
       #true表示向注册中心注册自己，默认为true
       register-with-eureka: true
       #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
       fetch-registry: true
       service-url:
         defaultZone: http://localhost:7001/eureka
   ```

3. 在主配置类上加上`@EnableEurekaClient`注解，表示这个项目是eureka的客户端。

4. 启动项目，然后刷新页面，成功注册进注册中心。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617533-64.png)
   在yml文件中application.name就是注册进注册中心时的应用名。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-65.png)

#### 3 将EurekaClient端80注册进EurekaServer成为服务消费者consumer

1. 引入依赖

   ```xml
    <!-- eureka-client -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
   ```

2. 在yml文件中添加

   ```yml
   spring:
     application:
       name: cloud-order-service
   
   eureka:
     client:
       register-with-eureka: true
       fetch-registry: true
       service-url:
         defaultZone: http://localhost:7001/eureka
   12345678910
   ```

3. 在主配置类上加上`@EnableEurekaClient`注解。

4. 启动项目，刷新页面
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-66.png)

### 2.1.3 集群Eureka构建步骤

#### 1 原理说明

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-67.png)
搭建Eureka注册中心集群，实现负载均衡+故障容错。

Eureka集群：**相互注册，相互守望**。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-68.png)

#### 2 构建eurekaServer集群环境

1. 参照cloud-eureka-server7001新建cloud-eureka-server7002和cloud-eureka-server7003。

2. Windows系统的兄弟就跟着老师修改hosts文件，因为我的是mac系统的，接下来写的步骤是mac的。

   打开终端，输入

   ```
   sudo vim /etc/hosts
   ```

   来修改hosts文件。（权限不够需要加上sudo并输入密码）

   在最后一行加入：

   ```shell
   127.0.0.1       eureka7001.com
   127.0.0.1       eureka7002.com
   127.0.0.1		eureka7003.com
   ```

   然后

   ```
   :wq!
   ```

   保存退出。

3. 修改7001的yml文件

   ```yml
   eureka:
     instance:
       hostname: eureka7001.com  #eureka服务端的实例名称
     client:
       register-with-eureka: false
       fetch-registry: false
       service-url:
   #      单机
   #      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
         #集群版  相互注册，相互守望
         defaultZone: http://eureka7002.com:7002/eureka/, http://eureka7003.com:7003/eureka/ 
         
   #    defaultZone是固定写法，如果想自定义，需要按以下写法才行：
   #    region: eureka-server
   #    availability-zones:
   #      eureka-server: server1,server2
   #    service-url:
   #      server1: http://eureka7002.com:7002/eureka/
   #      server2: http://eureka7003.com:7003/eureka/
   ```

4. 修改7002的yml文件

   ```yml
   eureka:
     instance:
       hostname: eureka7002.com  #eureka服务端的实例名称
     client:
       register-with-eureka: false
       fetch-registry: false
       service-url:
         #集群版  相互注册，相互守望
         defaultZone: http://eureka7001.com:7001/eureka/, http://eureka7003.com:7003/eureka/ #相互注册，相互守望
   ```

5. 修改7003的yml文件

   ```yml
   eureka:
     instance:
       hostname: eureka7003.com  #eureka服务端的实例名称
     client:
       register-with-eureka: false
       fetch-registry: false
       service-url:
         #集群版  相互注册，相互守望
         defaultZone: http://eureka7001.com:7001/eureka/, http://eureka7002.com:7002/eureka/ #相互注册，相互守望
   ```

6. 然后启动这三个项目
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-69.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-70.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-71.png)

#### 3 将支付服务8001和订单服务80微服务发布到集群配置中

把两个项目的yml文件中的defaultZone改为：

```yml
      #集群版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka,http://eureka7003.com:7003/eureka
```

启动5个项目进行测试：（先启动集群，再启动8001，最后启动80）
集群后台截图：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-72.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-73.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-74.png)

#### 4 构建支付服务提供者集群环境

1. 按照8001新建8002（只多建了一个提供者，建多了怕电脑受不了）。（除了要yml文件中需要改端口号和主配置类，其他直接复制8001的，yml文件中的应用名不需要改，因为是集群，所以应用名需要一致）

2. 分别在所有的提供者的PaymentController中加入：（这个@Value是spring的注解）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-75.png)

3. 修改消费者的OrderController，把写死的url改为服务名称：

   ```java
   public static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";
   ```

4. 然后在消费者的ApplicationContextConfig里的restTemplate方法上加上`@LoadBalanced`，开启负载均衡功能。

5. 启动eurekaServer集群，启动提供者集群，启动消费者。
   *如果启动提供者后出现，这个错误：`Public Key Retrieval is not allowed`*
   *请在yml文件中的datasource.datasource.url后加上`&allowPublicKeyRetrieval=true`即可解决。*
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-76.png)
   另外两个eurekaserver也一样，就不截图。
   在浏览器中输入`http://localhost/consumer/payment/get/1`，多次刷新可以看到，提供服务的应用在不同的切换，实现负载均衡的效果。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-77.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-78.png)

### 2.1.4 actuator微服务信息完善

修改三个微服务的yml文件：

```yml
#######8001#######
# client:	
#	 ...	instance要和client对齐
  instance:
    instance-id: payment8001
    prefer-ip-address: true   #访问路径可以显示ip地址

#######8002#######
  instance:
    instance-id: payment8002   #修改显示的主机名
    prefer-ip-address: true   #访问路径可以显示ip地址
    
#######80#######
  instance:
    instance-id: consumer80   #修改显示的主机名
    prefer-ip-address: true   #访问路径可以显示ip地址
12345678910111213141516
```

修改前：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-79.png)
修改后：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617534-80.png)

### 2.1.5 服务发现Discovery

对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息。

#### 修改提供者集群的controller

1. 在主配置类上加上`@EnableDiscoveryClient`注解，启用发现客户端。
2. 在两个提供者的PaymentController中加入：

```java
    @Resource
    private DiscoveryClient discoveryClient;	//springframework的DiscoveryClient（不要导错包了）
    

    @GetMapping("/payment/discovery")
    public Object discovery(){
        //获取服务列表的信息
        List<String> services = discoveryClient.getServices();
        for (String element : services) {
            log.info("*******element：" + element);
        }

        //获取CLOUD-PAYMENT-SERVICE服务的所有具体实例
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance instance : instances) {
            //getServiceId服务器id getHost主机名称 getPort端口号  getUri地址
            log.info(instance.getServiceId() + "\t" + instance.getHost() + "\t" + instance.getPort() + "\t" + instance.getUri());
        }

        return this.discoveryClient;
    }

}
```

#### 测试

对8001进行测试
，在浏览器输入：`http://localhost:8001/payment/discovery`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-81.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-82.png)

### 2.1.6 Eureka自我保护

#### 概述

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-83.png)

##### 导致原因

![在这里插入图片描述](尚硅谷-SpringCloud/20200605213632574.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-84.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-85.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-86.png)
*只有在一定时间内丢失大量服务的心跳才开启自我保护模式。*

#### 禁止自我保护

先把cloud-eureka-server7001和cloud-provider-payment8001都切回单机版测试禁止自我保护。

cloud-eureka-server7001的yml文件：

```yml
#  client
#  	...		server与client对齐
  server:
    #关闭自我保护，默认为true
    enable-self-preservation: false
    #心跳的间隔时间，单位毫秒
    eviction-interval-timer-in-ms: 2000
```

cloud-provider-payment8001的yml文件：

```yml
    #Eureka客户端向服务端发送心跳的时间间隔，单位秒（默认30秒）
    lease-renewal-interval-in-seconds: 1
    #Eureka服务端在收到最后一次心跳后等待的时间上限，单位秒（默认90秒），超时剔除服务
    lease-expiration-duration-in-seconds: 2
```

启动注册中心和提供者：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-87.png)
然后关闭提供者（模拟网络延时），提供者直接被剔除。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682041617535-88.png)

## 2.2 Zookeeper服务注册与发现

### 2.2.1 注册中心Zookeeper

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042464303-155.png)

关闭linux的防火墙：

```shell
systemctl stop firewalld
systemctl status firewalld
```

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042464303-156.png)
使用docker启动Zookeeper：（docker的操作可以看这篇：[Docker基础入门学习笔记](https://blog.csdn.net/qq_36903261/article/details/105870268)）

```shell
#拉取Zookeeper镜像
docker pull zookeeper

#启动Zookeeper
docker run --name zk01 -p 2181:2181 --restart always -d zookeeper
```

#### 服务节点是临时节点还是持久节点？

zookeeper也是有心跳机制，在一定时间能如果一直没心跳返回，Zookeeper就会把服务节点剔除掉。所以在Zookeeper上的服务节点是临时节点。

### 2.2.2 服务提供者

为了区分，中间空个8003。

1. 新建工程cloud-provider-payment8004

2. pom

   ```xml
   <!--因为接下来不会用到数据库，所以不导入数据库相关的依赖（防止没配置而报错）-->
   
   	<!--替换掉eureka依赖，其他直接复制8001-->
           <!--SpringBoot整合Zookeeper客户端-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
   <!--        <exclusions>-->
               <!--先排除自带的zookeeper3.5.3-->
   <!--            <exclusion>-->
   <!--                <groupId>org.apache.zookeeper</groupId>-->
   <!--                <artifactId>zookeeper</artifactId>-->
   <!--            </exclusion>-->
   <!--        </exclusions>-->
           </dependency>
   
           <!--添加zookeeper3.4.9版本（引入对应版本的依赖）-->
   <!--        <dependency>-->
   <!--            <groupId>org.apache.zookeeper</groupId>-->
   <!--            <artifactId>zookeeper</artifactId>-->
   <!--            <version>3.4.9</version>-->
   <!--        </dependency>-->
   ```

3. yml

   ```yml
   #端口号
   server:
     port: 8004
   
   
   spring:
     application:
       #服务别名——注册到zookeeper注册中心的名称
       name: cloud-provider-payment
     cloud:
       zookeeper:
         connect-string: 10.211.55.17:2181 #linux的ip加暴露的端口号
   ```

4. 主启动类

   ```java
   @EnableDiscoveryClient	//该注解用于向使用consul或者Zookeeper作为注册中心时注册服务
   @SpringBootApplication
   public class PaymentMain8004 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentMain8004.class, args);
       }
   }
   ```

5. controller
   新建controller.PaymentController

   ```java
   @Slf4j
   @RestController
   public class PaymentController {
   
       @Value("${server.port}")        //获取端口号
       private String serverPort;
   
       @RequestMapping("/payment/zk")
       public String paymentzk(){
           return "springcloud with zookeeper：" + serverPort + "\t" + UUID.randomUUID().toString();
       }
   
   }
   12345678910111213
   ```

6. Zookeeper容器
   默认已经帮我们启动了服务端和客户端，如果想进去容器的话，使用下面的命令。

   ```shell
   #查看正在运行的容器（查看Zookeeper容器的id）
   docker ps
   
   #进入zookeeper容器
   docker exec -it 容器ID /bin/bash
   
   #退出容器（或者按快捷键ctrl+P+Q退出）
   exit
   #启动容器
   docker start 容器ID
   #关闭容器
   docker stop 容器ID
   123456789101112
   ```

7. 启动8004项目，浏览器输入`http://localhost:8004/payment/zk`
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200606152453907.png)

8. 进入Zookeeper容器（成功注册进注册中心）
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200606152531888.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042464304-157.png)
   https://tool.lu/json/
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042464304-158.png)

### 2.2.3 服务消费者

1. 新建消费者模块cloud-consumerzk-order80。

2. pom和yml直接复制8004。（yml中端口号改为80，应用名改为cloud-consumer-order，其他都相同）

3. 主启动类。（与8004相同）

4. 在springcloud包下新建config.ApplicationContextConfig

   ```java
   @Configuration
   public class ApplicationContextConfig {
   
       @Bean
       @LoadBalanced   //负载均衡
       public RestTemplate restTemplate(){
           return new RestTemplate();
       }
   
   }
   ```

5. 新建controller.OrderZKController

   ```java
   @RestController
   @Slf4j
   public class OrderZKController {
   
       public static final String INVOKE_URL = "http://cloud-provider-payment";
   
       @Resource
       private RestTemplate restTemplate;
   
       @RequestMapping("/consumer/payment/zk")
       public String paymentInfo(){
           String result = restTemplate.getForObject(INVOKE_URL + "/payment/zk", String.class);
           return result;
       }
   
   }
   ```

6. 启动项目
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200606160012462.png)
   `http://localhost/consumer/payment/zk`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042464304-159.png)

## 2.3 Consul服务注册与发现

Consul官网：https://www.consul.io/
Consul中文文档：https://www.springcloud.cc/spring-cloud-consul.html

### 2.3.1 简介

Consul是一种服务网格解决方案，提供具有服务发现，配置和分段功能的全功能控制平面。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建完整的服务网格。Consul需要一个数据平面，并支持代理和本机集成模型。Consul附带了一个简单的内置代理，因此一切都可以直接使用，还支持Envoy等第三方代理集成。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705408-176.png)

### 2.3.2 主要特点

- 服务发现：Consul的客户端可以注册服务，例如 api或mysql，其他客户端可以使用Consul来发现给定服务的提供者。使用DNS或HTTP，应用程序可以轻松找到它们依赖的服务。
- 健康检测：领事客户端可以提供任意数量的运行状况检查，这些检查可以与给定服务（“ Web服务器是否返回200 OK”）或本地节点（“内存利用率低于90％”）相关。操作员可以使用此信息来监视群集的运行状况，服务发现组件可以使用此信息将流量从不正常的主机发送出去。
- KV存储：应用程序可以将Consul的分层键/值存储用于多种目的，包括动态配置，功能标记，协调，领导者选举等。简单的HTTP API使其易于使用。
- 安全的服务通信：领事可以为服务生成并分发TLS证书，以建立相互TLS连接。 意图 可用于定义允许哪些服务进行通信。可以使用可以实时更改的意图轻松管理服务分段，而不必使用复杂的网络拓扑和静态防火墙规则。
- 多数据中心：Consul开箱即用地支持多个数据中心。这意味着Consul的用户不必担心会构建其他抽象层以扩展到多个区域。

Consul旨在对DevOps社区和应用程序开发人员友好，使其非常适合现代，灵活的基础架构。

### 2.3.3 在docker上安装启动consul

```shell
#拉取consul镜像
docker pull consul

#启动consul
docker run -d  -p 8500:8500/tcp --name myConsul  consul agent -server -ui -bootstrap-expect=1 -client=0.0.0.0
12345
```

然后在浏览器输入`http://http://10.211.55.17/:8500`（linux的IP地址加上冒号8500）
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-177.png)

### 2.3.4 服务提供者

1. 新建服务提供者cloud-provider-consul-payment8006。
2. pom复制8004。（用下面的依赖替换Zookeeper的依赖）

```xml
 <!--SpringCloud consul-server-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-consul-discovery</artifactId>
  </dependency>
12345
```

1. yml

```yml
server:
  port: 8006


spring:
  application:
    name: consul-provider-payment
  cloud:
    consul:
      host: 10.211.55.17  #用linux的ip地址（consul在本机就填localhost）
      port: 8500
      discovery:
        service-name: ${spring.application.name}
12345678910111213
```

1. 主启动类（与8004相同）

2. controller

   ```java
   @RestController
   @Slf4j
   public class PaymentController {
   
       @Value("${server.port}")        //获取端口号
       private String serverPort;
   
       @RequestMapping("/payment/consul")
       public String paymentConsul(){
           return "springcloud with zookeeper：" + serverPort + "\t" + UUID.randomUUID().toString();
       }
   
   }
   12345678910111213
   ```

3. 启动项目
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-178.png)
   `http://localhost:8006/payment/consul`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-179.png)

### 2.3.5 服务消费者

1. 新建模块cloud-consumer-consul-order80

2. pom（与8006相同）

3. yml（端口号为80，应用名为consul-consumer-order，其他和8006相同）

4. 主启动类（与8006相同）

5. config（和zk的消费者相同）

6. controller.OrderConsulController

   ```java
   @RestController
   @Slf4j
   public class OrderConsulController {
   
       public static final String INVOKE_URL = "http://consul-provider-payment";
   
       @Resource
       private RestTemplate restTemplate;
   
       @RequestMapping("/consumer/payment/consul")
       public String paymentInfo(){
           String result = restTemplate.getForObject(INVOKE_URL + "/payment/consul", String.class);
           return result;
       }
   
   }
   12345678910111213141516
   ```

7. 启动项目
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-180.png)
   `http://localhost/consumer/payment/consul`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-181.png)

## 2.4 三者的异同点

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-182.png)

```
CAP：（只能二选一）
A：可用性
C：一致性
P：分区容错性（微服务架构必须保证有P）
```

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-183.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-184.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682042705409-185.png)

​      

# 3 Ribbon负载均衡服务调用

恢复[eureka](https://so.csdn.net/so/search?q=eureka&spm=1001.2101.3001.7020)集群环境，以便接下来的练习。

## 3.1 简介

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017110-206.png)

### 负载均衡

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017110-207.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017110-208.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017110-209.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017110-210.png)

## 3.2 Ribbon（负载均衡+RestTemplate调用）

Ribbon是客户端（消费者）负载均衡的工具。
![在这里插入图片描述](尚硅谷-SpringCloud/20200606201514137.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017110-211.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-212.png)

### 3.2.1 pom

```xml
  <!--Ribbon的依赖-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
 </dependency>
12345
```

新版的eureka依赖以及集成了Ribbon依赖，所以可以不引用。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-213.png)

### 3.2.2 RestTemplate

@LoadBalanced注解给RestTemplate开启负载均衡的能力。

官方文档：https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-214.png)

#### 1 getForObject/getForEntity方法

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-215.png)
getForObject已经用过了，所以只测试getForEntity方法。

测试getForEntity方法

在消费者cloud-consumer-order80的OrderController方法中添加：

```java
    @GetMapping("/consumer/payment/getEntity/{id}")
    public CommonResult<Payment> getPayment2(@PathVariable("id") Long id){
        log.info("********查询的id：" + id);
        //getForObject两个参数：请求地址，返回的对象类型
//        return restTemplate.getForObject(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
        ResponseEntity<CommonResult> entity = restTemplate.getForEntity(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);

        //getStatusCode获取状态码，is2xxSuccessful如果是状态码是2xx
        if(entity.getStatusCode().is2xxSuccessful()){
            //返回body
            return entity.getBody();
        }else{
            return new CommonResult<>(444, "操作失败");
        }
    }
123456789101112131415
```

然后启动eureka集群，提供者集群，消费者。（记得把eureka7001和提供者8001的yml文件改为集群环境（这个就这东西搞了我半小时，我就说怎么负载均衡不了了，只使用了8002））
在浏览器输入`http://localhost/consumer/payment/getEntity/1`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-216.png)

#### 2 postForObject/postForEntity方法

```java
    @GetMapping("/consumer/payment/createEntity")
    public CommonResult<Payment> create2(Payment payment){
        log.info("********插入的数据：" + payment);
        //postForObject分别有三个参数：请求地址，请求参数，返回的对象类型
//        return restTemplate.postForObject(PAYMENT_URL + "/payment/create", payment, CommonResult.class);
        ResponseEntity<CommonResult> entity = restTemplate.postForEntity(PAYMENT_URL + "/payment/create", payment, CommonResult.class);
        if(entity.getStatusCode().is2xxSuccessful()){
            return entity.getBody();
        }else{
            return new CommonResult<>(444, "操作失败");
        }
    }
123456789101112
```

输入`http://localhost//consumer/payment/createEntity?serial=田七`，插入一条数据。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-217.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-218.png)

## 3.3 Ribbon核心组件IRUle

IRUle接口的实现类：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-219.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-220.png)
*默认为RoundRobinRule轮询。*

### 3.3.1 替换

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-221.png)
**Ribbon的自定义配置类不可以放在@ComponentScan所扫描的当前包下以及子包下**，否则这个自定义配置类就会被所有的Ribbon客户端共享，达不到为指定的Ribbon定制配置，**而@SpringBootApplication注解里就有@ComponentScan注解**，所以不可以放在主启动类所在的包下。（因为Ribbon是客户端（消费者）这边的，所以Ribbon的自定义配置类是在客户端（消费者）添加，不需要在提供者或注册中心添加）

1. 所以Ribbon的自定义配置类不能放在springcloud包下，要在angenin包下再新建一个myrule包。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-222.png)

2. 在此包下新建MySelfRule自定义配置类

   ```java
   @Configuration
   public class MySelfRule {
   
       @Bean
       public IRule myRule(){
           return new RandomRule();    //负载均衡机制改为随机
       }
   
   }
   123456789
   ```

3. 在主启动类上添加`@RibbonClient(name = "CLOUD-PAYMENT-SERVICE", configuration = MySelfRule.class)`
   name为指定的服务名（服务名必须与注册中心显示的服务名大小写一致）
   configuration为指定服务使用自定义配置（自定义负载均衡机制）

4. 启动eurekaserver集群，提供者集群，消费者。

5. 浏览器输入`http://localhost/consumer/payment/get/1`，多次刷新实现负载均衡为随机。

## 3.4 Ribbon负载均衡——轮询算法

### 3.4.1 RoundRobinRule原理

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-223.png)
取余

### 3.4.2 RoundRobinRule源码

RoundRobinRule的核心为choose方法：

```java
public class RoundRobinRule extends AbstractLoadBalancerRule {
	//AtomicInteger原子整形类
    private AtomicInteger nextServerCyclicCounter;
	...
    public RoundRobinRule() {
    	//此时nextServerCyclicCounter是一个原子整形类，并且value为0
        nextServerCyclicCounter = new AtomicInteger(0);
    }
	...
	//ILoadBalancer选择的负载均衡机制，这里lb为轮询
    public Server choose(ILoadBalancer lb, Object key) {
    	//如果传入的lb没有负载均衡，为空
        if (lb == null) {
            log.warn("no load balancer");
            return null;
        }

        Server server = null;
        int count = 0;
        //还没选到执行的server，并且选择的次数没超过10次，进行选择server
        while (server == null && count++ < 10) {
        	//lb.getReachableServers获取所有状态是up的服务实例
            List<Server> reachableServers = lb.getReachableServers();
            //lb.getAllServers获取所有服务实例
            List<Server> allServers = lb.getAllServers();
            //状态为up的服务实例的数量
            int upCount = reachableServers.size();
            //所有服务实例的数量
            int serverCount = allServers.size();
			
			//如果up的服务实例数量为0或者服务实例为0，打印日志log.warn并返回server=null
            if ((upCount == 0) || (serverCount == 0)) {
                log.warn("No up servers available from load balancer: " + lb);
                return null;
            }
			
			//获取到接下来server的下标
            int nextServerIndex = incrementAndGetModulo(serverCount);
            //获取下一个server
            server = allServers.get(nextServerIndex);

			//如果
            if (server == null) {
                //线程让步，线程会让出CPU执行权，让自己或者其它的线程运行。（让步后，CPU的执行权也有可能又是当前线程）
                Thread.yield();
                //进入下次循环
                continue;
            }
			
			//获取的server还活着并且还能工作，则返回该server
            if (server.isAlive() && (server.isReadyToServe())) {
                return (server);
            }

            //否则server改为空
            server = null;
        }

		//选择次数超过10次，打印日志log.warn并返回server=null
        if (count >= 10) {
            log.warn("No available alive servers after 10 tries from load balancer: "
                    + lb);
        }
        return server;
    }


    private int incrementAndGetModulo(int modulo) {
    	//CAS加自旋锁
    	//CAS（Conmpare And Swap）：是用于实现多线程同步的原子指令。CAS机制当中使用了3个基本操作数：内存地址V，旧的预期值A，要修改的新值B。更新一个变量的时候，只有当变量的预期值A和内存地址V当中的实际值相同时，才会将内存地址V对应的值修改为B。
    	//自旋锁：是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。 
        for (;;) {
        	//获取value，即0
            int current = nextServerCyclicCounter.get();
            //取余，为1
            int next = (current + 1) % modulo;
            //进行CAS判断，如果此时在value的内存地址中，如果value和current相同，则为true，返回next的值，否则就一直循环，直到结果为true
            if (nextServerCyclicCounter.compareAndSet(current, next))
                return next;
        }
    }
    ...
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283
```

AtomicInteger的compareAndSet方法：

```java
public class AtomicInteger extends Number implements java.io.Serializable {
	...
    public final boolean compareAndSet(int expect, int update) {
        return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }
    ...
}
1234567
```

### 3.4.3 手写一个轮询自定义配置类

##### 8001和8002微服务改造

在8001和8002的PaymentController中加上这个方法，用于测试我们的自定义轮询：

```java
    @GetMapping("/payment/lb")
    public String getPaymentLB(){
        return serverPort;
    }
```

##### 80订单微服务改造

1. 去掉ApplicationContextConfig里restTemplate方法上的@LoadBalanced注解。

2. 在springcloud包下新建lb.ILoadBalancer接口（自定义负载均衡机制（面向接口））

   ```java
   public interface ILoadBalancer {
   
       //传入具体实例的集合，返回选中的实例
       ServiceInstance instances(List<ServiceInstance> serviceInstance);
   
   }
   ```
   
3. 在lb包下新建自定义ILoadBalancer接口的实现类

   ```java
   @Component  //加入容器
   public class MyLB implements ILoadBalancer {
   
       //新建一个原子整形类
       private AtomicInteger atomicInteger = new AtomicInteger(0);
   
       //
       public final int getAndIncrement(){
           int current;
           int next;
           do{
               current = this.atomicInteger.get();
               //如果current是最大值，重新计算，否则加1（防止越界）
               next = current >= Integer.MAX_VALUE ? 0 : current + 1;
   
           //进行CAS判断，如果不为true，进行自旋
           }while (!this.atomicInteger.compareAndSet(current, next));
           System.out.println("****第几次访问，次数next：" + next);
   
           return next;
       }
   
       @Override
       public ServiceInstance instances(List<ServiceInstance> serviceInstance) {
           //非空判断
           if(serviceInstance.size() <= 0){
               return null;
           }
           //进行取余
           int index = getAndIncrement() % serviceInstance.size();
           //返回选中的服务实例
           return serviceInstance.get(index);
       }
   }
   ```
   
4. 在OrderController添加：

   ```java
       @Resource
       private ILoadBalancer iLoadBalancer;
       @Resource
       private DiscoveryClient discoveryClient;
   	
       @GetMapping("/consumer/payment/lb")
       public String getPaymentLB(){
           //获取CLOUD-PAYMENT-SERVICE服务的所有具体实例
           List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
           if(instances == null || instances.size() <= 0){
               return null;
           }
   
           ServiceInstance serviceInstance = iLoadBalancer.instances(instances);
           URI uri = serviceInstance.getUri();
           System.out.println(uri);
   
           return restTemplate.getForObject(uri + "/payment/lb", String.class);
       }
   ```
   
5. 启动server集群，提供者集群，80消费者，然后在浏览器中输入`http://localhost/consumer/payment/lb`，多次刷新，实现自定义轮询。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-224.png)

# 4 OpenFeign服务接口调用

## 4.1 简介

官网文档：https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/#spring-cloud-openfeign
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-225.png)
[Feign](https://so.csdn.net/so/search?q=Feign&spm=1001.2101.3001.7020)是一个声明式的web服务客户端，让编写web服务客户端变得非常容易，只需创建一个接口并在接口上添加注解即可。

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-226.png)
Feign与OpenFeign的区别
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-227.png)

## 4.2 OpenFeign的使用（也是在消费者端）

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017111-228.png)

1. 新建模块cloud-consumer-feign-order80

2. pom

   ```xml
   <dependencies>
       <!-- openfeign -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-openfeign</artifactId>
       </dependency>
       <!-- eureka-client -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
       <dependency>
           <groupId>com.angenin.springcloud</groupId>
           <artifactId>cloud-api-commons</artifactId>
           <version>${project.version}</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```
   
3. yml

   ```yml
   server:
     port: 80
   
   
   eureka:
     client:
       register-with-eureka: false
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka,http://eureka7003.com:7003/eureka
   123456789
   ```

4. 主启动类

   ```java
   @EnableFeignClients //激活feign
   @SpringBootApplication
   public class OrderFeignMain80 {
   
       public static void main(String[] args) {
           SpringApplication.run(OrderFeignMain80.class, args);
       }
   
   }
   ```
   
5. 在springcloud包下新建service.PaymentFeignService接口
   *（业务逻辑接口+@FeignClient配置调用provider服务。）*

   新建PaymentFeignService接口并新增注解@FeignClient

   ```java
   //Feign封装了Ribbon和RestTemplate，实现负载均衡和发送请求
   @Component
   @FeignClient(value = "CLOUD-PAYMENT-SERVICE")    //作为feign的接口，找CLOUD-PAYMENT-SERVICE服务
   public interface PaymentFeignService {
   
       //直接复制8001的方法
       @GetMapping("/payment/get/{id}")
       public CommonResult getPaymentById(@PathVariable("id") Long id);
   
   }
   ```
   
6. 在springcloud包下新建controller.OrderFeignController

   ```java
   @Slf4j
   @RestController
   public class OrderFeignController {
   
       @Resource
       private PaymentFeignService paymentFeignService;
   
       @GetMapping("/consumer/payment/get/{id}")
       public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id){
           return paymentFeignService.getPaymentById(id);
       }
   
   }
   ```
   
7. 按照顺序启动项目（server集群，提供者集群，消费者），然后在浏览器输入`http://localhost/consumer/payment/get/1`，成功查询到数据，并且有负载均衡（轮询）。 ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-229.png)

总结：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-230.png)

## 4.3 OpenFeign超时控制

提供者在处理服务时用了3秒，提供者认为花3秒是正常，而消费者只愿意等1秒，1秒后，提供者会没返回数据，消费者就会造成超时调用报错。
所以需要双方约定好时间，不使用默认的。

### 4.3.1 模拟超时出错的情况

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-231.png)

1. 在8001的PaymentController里添加：（模拟服务处理时间长）

   ```java
       @GetMapping("/payment/feign/timeout")
       public String paymentFeignTimeout(){
           try {
               TimeUnit.SECONDS.sleep(3);
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
           return serverPort;
       }
   ```
   
2. 在80的PaymentFeignService中添加：

   ```java
   @GetMapping("/payment/feign/timeout")
   public String paymentFeignTimeout();
   ```
   
3. 然后在80的OrderFeignController中添加：

   ```java
       @GetMapping("/consumer/payment/feign/timeout")
       public String paymentFeignTimeout(){
           //openFeign-ribbon，客户端一般默认等待1秒
           return paymentFeignService.paymentFeignTimeout();
       }
   ```
   
4. 启动server集群，提供者8001，消费者80
   `http://localhost/consumer/payment/feign/timeout`
   三秒后报错。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-232.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-233.png)

1. 在80的yml中添加：

   ```yml
   #没提示不管它，可以设置
   ribbon:
     #指的是建立连接后从服务器读取到可用资源所用的时间
     ReadTimeout: 5000
     #指的是建立连接使用的时间，适用于网络状况正常的情况下，两端连接所用的时间
     ConnectTimeout: 5000
   ```
   
2. 重新访问`http://localhost/consumer/payment/feign/timeout`，3秒后显示。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-234.png)

## 4.4 OpenFeign日志打印功能

![在这里插入图片描述](尚硅谷-SpringCloud/20200608102405693.png)

### 4.4.1 日志级别

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-235.png)

### 4.4.2 步骤

1. 配置日志bean
   在80的springcloud包下新建config.FeignConfig

```java
import feign.Logger;	//不要导错包

@Configuration
public class FeignConfig {

    @Bean
    Logger.Level feignLoggerLevel(){
        //打印最详细的日志
        return Logger.Level.FULL;
    }

}
123456789101112
```

1. 在80的yml文件中添加：

   ```yml
   #开启日志的feign客户端
   logging:
     level:
       #feign日志以什么级别监控哪个接口
       com.angenin.springcloud.service.PaymentFeignService: debug	#写你们自己的包名
   ```
   
2. 启动项目，`http://localhost/consumer/payment/feign/timeout`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682064017112-236.png)

# 5 Hystrix断路器

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467355-303.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467355-304.png)

## 5.1 简介

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467355-305.png)
功能：**服务降级**，**服务熔断**，**接近实时的监控**，限流，隔离等。

## 5.2 Hystrix重要概念

### 5.2.1 服务降级（fallback）

提供者和消费者都可以进行服务降级。（一般都是放在客户端（消费者））
![在这里插入图片描述](尚硅谷-SpringCloud/20200608113847222.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467355-306.png)

### 5.2.2  服务熔断（break）

![在这里插入图片描述](尚硅谷-SpringCloud/20200608113944442.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200608114110865.png)

### 5.2.3  服务限流（flowlimit）

![在这里插入图片描述](尚硅谷-SpringCloud/20200608114039678.png)

## 5.3 Hystrix案例

### 5.3.1 构建

把7001改为单机版，方便后面进行案例测试。

1. 新建项目cloud-provider-hystrix-payment8001

2. pom

   ```xml
   <dependencies>
       <!-- hystrix-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
       </dependency>
       <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
       <dependency>
           <groupId>com.angenin.springcloud</groupId>
           <artifactId>cloud-api-commons</artifactId>
           <version>${project.version}</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <!--监控-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--eureka client-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <!--   一个Java工具包     -->
       <dependency>
           <groupId>cn.hutool</groupId>
           <artifactId>hutool-all</artifactId>
           <version>5.1.0</version>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```
   
3. yml

   ```yml
   server:
     port: 8001
   
   
   spring:
     application:
       name: cloud-provider-hystrix-payment
   	
   
   eureka:
     client:
       register-with-eureka: true
       fetch-registry: true
       service-url:
         #单机版
         defaultZone: http://localhost:7001/eureka
         #集群版
   #      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka,http://eureka7003.com:7003/eureka
   ```
   
4. 主启动类

   ```java
   @EnableEurekaClient
   @SpringBootApplication
   public class PaymentHystrixMain8001 {
   
       public static void main(String[] args) {
           SpringApplication.run(PaymentHystrixMain8001.class, args);
       }
   
   }
   ```
   
5. service

   ```java
   @Service
   public class PaymentService {
   
       //正常访问方法
       public String paymentInfo_OK(Integer id){
           return "线程池：" + Thread.currentThread().getName() + "\tpaymentInfo_OK，id：" + id;
       }
   
   
       //超时访问方法
       public String paymentInfo_TimeOut(Integer id){
           int timeNumber = 3;
           try {
               TimeUnit.SECONDS.sleep(timeNumber);
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
           return "线程池：" + Thread.currentThread().getName() +
                   "\tpaymentInfo_TimeOut，id：" + id + "，耗时：" + timeNumber + "秒";
       }
   
   }
   ```
   
6. controller

   ```java
   @Slf4j
   @RestController
   public class PaymentController {
   
       @Resource
       PaymentService paymentService;
   
       @Value("${server.port}")    //spring的@Value注解
       private String ServerPort;
   
       @GetMapping("/payment/hystrix/ok/{id}")
       public String paymentInfo_OK(@PathVariable("id") Integer id){
           String result = paymentService.paymentInfo_OK(id);
           log.info("******result：" + result);
           return result;
       }
   
       @GetMapping("/payment/hystrix/timeout/{id}")
       public String paymentInfo_TimeOut(Integer id){
           String result = paymentService.paymentInfo_OK(id);
           log.info("******result：" + result);
           return result;
       }
   
   }
   ```
   
7. 启动7001和8001
   `http://localhost:8001/payment/hystrix/ok/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608144948608.png)
   `http://localhost:8001/payment/hystrix/timeout/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608145241573.png)

![在这里插入图片描述](尚硅谷-SpringCloud/20200608144837222.png)

### 5.3.2 高并发测试

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467356-307.png)

#### 安装JMeter

JMeter下载地址：http://jmeter.apache.org/download_jmeter.cgi

下载tgz和zip都可以：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467356-308.png)
进入解压后的目录的bin目录，找到jmeter.properties文件，修改语言zh_CN。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467356-309.png)
从终端进入bin目录，输入`./jmeter`运行jmeter，修改成中文版。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467356-310.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467356-311.png)

#### 进行高并发测试

测试`http://localhost:8001/payment/hystrix/timeout/1`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467356-312.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467356-313.png)
然后去访问`http://localhost:8001/payment/hystrix/ok/1`，访问速度变慢了。
![在这里插入图片描述](尚硅谷-SpringCloud/20200608152534193.png)

#### 新建80

1. 新建cloud-consumer-feign-hystrix-order80

2. pom

   ```xml
   <dependencies>
       <!-- openfeign -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-openfeign</artifactId>
       </dependency>
       <!--   hystrix     -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
       </dependency>
       <!--eureka client-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
       <dependency>
           <groupId>com.angenin.springcloud</groupId>
           <artifactId>cloud-api-commons</artifactId>
           <version>${project.version}</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
   ```

3. yml

   ```yml
   server:
     port: 80
   
   
   eureka:
     client:
       register-with-eureka: false
       service-url:
         defaultZone: http://localhost:7001/eureka
   
   #需要加上，否则会报错
   ribbon:
     ReadTimeout: 4000
     ConnectTimeout: 4000
   ```
   
4. 主启动类

   ```java
   @EnableEurekaClient
   @EnableFeignClients
   @SpringBootApplication
   public class OrderHystrixMain80 {
   
       public static void main(String[] args) {
           SpringApplication.run(OrderHystrixMain80.class, args);
       }
   
   }
   ```

5. service

   ```java
   @Component
   @FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT")
   public interface PaymentHystrixService {
   
       @GetMapping("/payment/hystrix/ok/{id}")
       public String paymentInfo_OK(@PathVariable("id") Integer id);
       
       @GetMapping("/payment/hystrix/timeout/{id}")
       public String paymentInfo_TimeOut(@PathVariable("id") Integer id);
   }
   ```

6. controller

   ```java
   @Slf4j
   @RestController
   public class OrderHystrixController {
   
       @Resource
       private PaymentHystrixService paymentHystrixService;
   
   
       @GetMapping("/consumer/payment/hystrix/ok/{id}")
       public String paymentInfo_OK(@PathVariable("id") Integer id){
           String result = paymentHystrixService.paymentInfo_OK(id);
           return result;
       }
   
       @GetMapping("/consumer/payment/hystrix/timeout/{id}")
       public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
           String result = paymentHystrixService.paymentInfo_TimeOut(id);
           return result;
       }
   
   }
   ```

7. 启动80，进行测试
   `http://localhost/consumer/payment/hystrix/ok/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608155353468.png)

   `http://localhost/consumer/payment/hystrix/timeout/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-314.png)

8. 启动jmeter，然后再进行测试
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-315.png)

#### 故障现象、导致原因以及解决

现象：
![在这里插入图片描述](尚硅谷-SpringCloud/20200608160322883.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200608160503320.png)
解决：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-316.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-317.png)

## 5.4 服务降级

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-318.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-319.png)

### 5.4.1 提供者

1. 修改8001中PaymentService的paymentInfo_TimeOut方法，并添加paymentInfo_TimeOutHandler方法：

   ```java
       @HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler", commandProperties = {
               //设置自身超时调用时间的峰值为3秒，峰值内可以正常运行，超过了需要有兜底的方法处理，服务降级fallback
               @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "3000")
       })
       public String paymentInfo_TimeOut(Integer id){
           int timeNumber = 5;
           //int i = 1 / 0;
           try {
               TimeUnit.SECONDS.sleep(timeNumber);
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
           return "线程池：" + Thread.currentThread().getName() +
                   "\tpaymentInfo_TimeOut，id：" + id + "，耗时：" + timeNumber + "秒";
       }
       public String paymentInfo_TimeOutHandler(Integer id){
           return "8001提供者，线程池：" + Thread.currentThread().getName() + 
                   "\tpaymentInfo_TimeOutHandler系统繁忙，请稍后再试，id：" + id;
       }
   ```
   
2. 然后在8001的主启动类上添加`@EnableCircuitBreaker`注解，启用断路器。

3. 启动7001和8001，测试8001的fallback，`http://localhost:8001/payment/hystrix/timeout/1`成功进入fallback方法。（并且fallback方法是用Hystrix的线程池）
   ![在这里插入图片描述](尚硅谷-SpringCloud/2020060816415940.png)

4. 去掉sleep，改为 1 / 0，测试方法运行异常，`http://localhost:8001/payment/hystrix/timeout/1`，也可以进入fallback方法。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608164714747.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-320.png)

### 5.4.2 消费者

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-321.png)

1. 在80的yml中添加：

   ```yml
   feign:
     hystrix:
       enabled: true
   ```
   
2. 在主启动类添加`@EnableHystrix`注解。

3. 修改OrderHystrixController的paymentInfo_TimeOut方法，并添加paymentTimeOutFallbackMethod方法：

   ```java
       @HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod", commandProperties = {
               @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "1500")
       })
       @GetMapping("/consumer/payment/hystrix/timeout/{id}")
       public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
           String result = paymentHystrixService.paymentInfo_TimeOut(id);
           return result;
       }
       public String paymentTimeOutFallbackMethod(@PathVariable("id") Integer id){
           return "消费者80，支付系统繁忙";
       }
   ```
   
4. 启动7001，8001，80，`http://localhost/consumer/payment/hystrix/timeout/1`（如果是提供者那边出问题，并且消费者设置了fallback，会优先进入消费者的fallback）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-322.png)

5. 在中添加`int i = 1 / 0;`，运行异常也会进入80的fallback方法。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608173757412.png)

### 5.4.3 目前的问题和解决办法

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-323.png)

#### 代码膨胀的解决办法

解决办法：设置全局fallback方法。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-324.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-325.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-326.png)

1. 在80的OrderHystrixController中添加全局fallback方法：

```java
    //全局fallback方法，不能有传参
    public String payment_Global_FallbackMethod(){
        return "Global异常处理信息，请稍后再试！";
    }
```

1. 并在OrderHystrixController类上加上`@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")`，设置全局fallback方法。

2. 把paymentInfo_TimeOut方法的@HystrixCommand

   ```java
   //    @HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod", commandProperties = {
   //            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "1500")
   //    })
       @HystrixCommand
   ```
   
3. 进行测试，`http://localhost/consumer/payment/hystrix/timeout/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608185611971.png)

#### 混乱的解决办法

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467357-327.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-328.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-329.png)

#### 模拟宕机场景

1. 在80的service包下新建PaymentFallbackService类，实现PaymentHystrixService接口

   ```java
   //统一为接口里面的方法进行异常处理
   @Component
   public class PaymentFallbackService implements PaymentHystrixService {
       @Override
       public String paymentInfo_OK(Integer id) {
           return "----PaymentFallbackService\t fallback-paymentInfo_OK----";
       }
   
       @Override
       public String paymentInfo_TimeOut(Integer id) {
           return "----PaymentFallbackService\t fallback-paymentInfo_TimeOut----";
       }
   }
   12345678910111213
   ```

2. 要在yml中加上：（我们在之前就加上了）

   ```yml
   feign:
     hystrix:
       enabled: true
   123
   ```

3. 然后给PaymentHystrixService接口的@FeignClient注解加上`fallback = PaymentFallbackService.class`属性，用于出错进行fallback处理。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-330.png)

4. 启动7001，8001，80，然后先访问``，成功访问
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-331.png)

5. 然后关掉8001，模拟提供者宕机，刷新页面
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608202822142.png)

![在这里插入图片描述](尚硅谷-SpringCloud/20200608202919567.png)

## 5.5 服务熔断

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-332.png)

### 实操

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-333.png)

1. 在8001的PaymentService中添加

   ```java
   	@HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
           @HystrixProperty(name = "circuitBreaker.enabled", value = "true"),                      //开启断路器
           @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold", value = "10"),         //请求总数阈值（默认20）
           @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds", value = "10000"),   //休眠时间窗口期（休眠多久进入半开模式（单位毫秒，默认5秒））
           @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage", value = "60"),       //请求次数的错误率达到多少跳闸（百分率%，默认50%）
   })
       public String paymentCircuitBreaker(@PathVariable("id") Integer id) {
           if(id < 0){
               throw  new RuntimeException("****id 不能为负数");
           }
           String serialNumber = IdUtil.simpleUUID();
   
           return  Thread.currentThread().getName() + "\t" + "调用成功，流水号：" + serialNumber;
       }
       public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id){
           return "id 不能为负数,请稍后再试， id: " + id;
       }
   ```
   
2. 在8001的PaymentController中添加

   ```java
   @GetMapping("/payment/circuit/{id}")
   public String paymentCircuitBreaker(@PathVariable("id") Integer id){
       String result = paymentService.paymentCircuitBreaker(id);
       log.info("******result：" + result);
       return result;
   }
   123456
   ```

3. 启动7001和8001
   `http://localhost:8001/payment/circuit/-1`（输入超过6次进入熔断）
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608223649310.png)
   熔断10秒内就算是正确的请求也返回错误信息。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608224017409.png)
   10秒后进入半开模式，对请求进行处理，此时如果是正确的请求，那么就关闭熔断，否则再次进入熔断，10秒后再次开启半开模式，对请求进行处理，直到半开模式处理到正确请求。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200608224005451.png)

### 总结

https://martinfowler.com/bliki/CircuitBreaker.html
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-334.png)

我的总结：如果请求次数的错误率超过指定值，开启熔断，经过一段时间后，变为半开模式，然后放进一个请求进行处理，如果请求处理成功，关闭熔断；如果还是报错，继续进入熔断，再经过一段时间后，变为半开模式，再进行对下一个请求进行处理，一直在熔断，半开模式来回切换，直到请求成功，关闭熔断。

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-335.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-336.png)
官网步骤：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-337.png)

断路器在什么情况下开始起作用：

https://github.com/Netflix/Hystrix/wiki/How-it-Works
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-338.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-339.png)
断路器开启或关闭的条件：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-340.png)
断路器打开之后：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-341.png)

## 5.6 服务限流

会在后面高级篇alibaba的Sentinel讲解。



## 5.7 Hystrix工作流程

https://github.com/Netflix/Hystrix/wiki/How-it-Works
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467358-342.png)
官方图例：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-343.png)

## 5.8 服务监控HystrixDashboard

### 5.8.1 简介

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-344.png)

### 5.8.2 仪表盘9001

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-345.png)

1. 新建模块cloud-consumer-hystrix-dashboard9001

2. pom

   ```xml
   <dependencies>
       <!--   hystrix仪表盘图形化     -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
       </dependency>
       <dependency>
           <groupId>com.angenin.springcloud</groupId>
           <artifactId>cloud-api-commons</artifactId>
           <version>${project.version}</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   12345678910111213141516171819202122232425262728293031323334353637
   ```

3. yml

   ```yml
   server:
     port: 9001
   12
   ```

4. 主启动类

   ```java
   @EnableHystrixDashboard //启用Hystrix仪表板
   @SpringBootApplication
   public class HystrixDashboard9001 {
   
       public static void main(String[] args) {
           SpringApplication.run(HystrixDashboard9001.class, args);
       }
   
   }
   123456789
   ```

5. 启动9001，在浏览器中输入`http://localhost:9001/hystrix`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-346.png)

### 5.8.3 断路器演示（服务监控hystrixDashboard）

注意：所有微服务提供者都需要在pom中引入监控依赖。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
1234
```

修改8001
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-347.png)
在8001的主启动类中添加：

```java
   /**
    * 此配置是为了服务监控而配置，与服务容错本身无关，springcloud升级后的坑
    * ServletRegistrationBean因为SpringBoot的默认路径不是 “/hystrix.stream"
    * 只要在自己的项目里配置上下的servlet就可以了
    */
   @Bean
   public ServletRegistrationBean getServlet() {
       HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet() ;
       ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
       registrationBean.setLoadOnStartup(1);
       registrationBean.addUrlMappings("/hystrix.stream");
       registrationBean.setName("HystrixMetricsStreamServlet");
       return  registrationBean;
   }
1234567891011121314
```

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-348.png)

### 5.8.4 监控测试

启动9001，7001，8001

1. 9001监控8001
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-349.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-350.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-351.png)
2. 在浏览器输入`http://localhost:8001/payment/circuit/1`和`http://localhost:8001/payment/circuit/-1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-352.png)
   多次输入`http://localhost:8001/payment/circuit/-1`错误的访问。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-353.png)
   稍微等一会，然后输入正确的访问`http://localhost:8001/payment/circuit/1`，就会熔断就会关闭。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-354.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-355.png)
7色：
![在这里插入图片描述](尚硅谷-SpringCloud/20200609102823528.png)
1圈：
![在这里插入图片描述](尚硅谷-SpringCloud/20200609102909116.png)
1线：
![在这里插入图片描述](尚硅谷-SpringCloud/20200609102946569.png)
整图说明：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467359-356.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467360-357.png)
整图说明2：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467360-358.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682071467360-359.png)

# 6 Gateway新一代网关

https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/

## 6.1 简介

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586828-520.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586828-521.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200609211403820.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586828-522.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200609211728665.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-523.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-524.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-525.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-526.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-527.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-528.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-529.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-530.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-531.png)

## 6.2 三大核心概念

### 6.2.1 Route（路由）

![在这里插入图片描述](尚硅谷-SpringCloud/20200609221437913.png)

### 6.2.2 Predicate（断言）

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-532.png)

### 6.2.3 Filter（过滤）

![在这里插入图片描述](尚硅谷-SpringCloud/20200617094327739.png)

### 6.2.4 总体

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-533.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200617094535575.png)

## 6.3 Gateway工作流程

官网总结
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-534.png)
![在这里插入图片描述](尚硅谷-SpringCloud/2020061709485433.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586829-535.png)

## 6.4 入门配置

1. 新建模块cloud-gateway-gateway9527

2. pom

   ```xml
   <dependencies>
       <!--gateway-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-gateway</artifactId>
       </dependency>
       <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
       <dependency>
           <groupId>com.angenin.springcloud</groupId>
           <artifactId>cloud-api-commons</artifactId>
           <version>${project.version}</version>
       </dependency>
       <!--eureka client(通过微服务名实现动态路由)-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```

3. yml

   ```yml
   server:
     port: 9527
   
   spring:
     application:
       name: cloud-gateway
   
   eureka:
     instance:
       hostname: cloud-gateway-service
     client:
       fetch-registry: true
       register-with-eureka: true
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/
   ```

4. 主启动类GatewayMain9527

   ```java
   @SpringBootApplication
   @EnableEurekaClient
   public class GatewayMain9527 {
       public static void main(String[] args) {
           SpringApplication.run(GatewayMain9527.class, args);
       }
   }
   ```

5. 修改yml文件
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-536.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-537.png)

   ```yml
   server:
     port: 9527
   
   spring:
     application:
       name: cloud-gateway
     cloud:
       gateway:
         routes:
           - id: payment_route # 路由的id,没有规定规则但要求唯一,建议配合服务名
             #匹配后提供服务的路由地址
             uri: http://localhost:8001
             predicates:
               - Path=/payment/get/** # 断言，路径相匹配的进行路由
   
           - id: payment_route2
             uri: http://localhost:8001
             predicates:
               - Path=/payment/lb/** #断言,路径相匹配的进行路由
   
   eureka:
     instance:
       hostname: cloud-gateway-service
     client:
       fetch-registry: true
       register-with-eureka: true
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/
   ```

6. 测试，启动7001，cloud-provider-payment8001，9527
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-538.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200617104243634.png)
   `http://localhost:8001/payment/get/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-539.png)
   `http://localhost:9527/payment/get/1`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-540.png)

### 6.4.1 Gateway网关路由的两种配置方式

1. 在配置文件中配置
   在配置文件yml中配置

2. 在配置类中配置
   代码中注入RouteLocator的Bean ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-541.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-542.png)
   新建config.GatewayConfig

   ```java
   @Configuration
   public class GatewayConfig {
   
       @Bean
       public RouteLocator customRouteLocator(RouteLocatorBuilder routeLocatorBuilder){
           RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
   
           routes.route("path_route_angenin",  //id
                   r -> r.path("/guonei")  //访问 http://localhost:9527/guonei
                           .uri("http://news.baidu.com/guonei"));  //就会转发到 http://news.baidu.com/guonei
   
           routes.route("path_route_angenin2",  //id
                   r -> r.path("/guoji")  //访问 http://localhost:9527/guoji
                           .uri("http://news.baidu.com/guoji"));  //就会转发到 http://news.baidu.com/guonji
   
           return routes.build();
       }
   
   //    @Bean
   //    public RouteLocator customRouteLocator2(RouteLocatorBuilder routeLocatorBuilder){
   //        RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
   //
   //        routes.route("path_route_angenin2",  //id
   //                r -> r.path("/guoji")  //访问 http://localhost:9527/guoji
   //                        .uri("http://news.baidu.com/guoji"));  //就会转发到 http://news.baidu.com/guonji
   //
   //        return routes.build();
   //    }
   
   }
   ```

测试，启动7001，8001，9527
`http://localhost:9527/guonei`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-543.png)
`http://localhost:9527/guoji`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-544.png)

## 6.5 通过微服务名实现动态路由

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-545.png)
修改yml文件

```yml
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true  #开启从注册中心动态创建路由的功能，利用微服务名称进行路由(默认false)
      routes:
        - id: payment_route #路由的id,没有规定规则但要求唯一,建议配合服务名
#          uri: http://localhost:8001  #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/get/** #断言，路径相匹配的进行路由

        - id: payment_route2
#          uri: http://localhost:8001
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/lb/** #断言,路径相匹配的进行路由
1234567891011121314151617181920
```

enabled默认false：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-546.png)

测试，启动7001，8001，8002，9527
`http://localhost:9527/payment/lb`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-547.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200617113524542.png)

## 6.6 Predicate的使用

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-548.png)
官网：https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/#gateway-request-predicates-factories
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586830-549.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-550.png)

常用的Route Predicate
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-551.png)

### 6.6.1 After/Before/Between

新建测试类T2

```java
public class T2 {

    public static void main(String[] args) {
    	//获取当前时间串
        ZonedDateTime now = ZonedDateTime.now();
        System.out.println(now);
        //2020-06-17T11:53:40.325+08:00[Asia/Shanghai]
    }

}
12345678910
```

然后在yml中的`predicates:`加上

```yml
			#指定时间后才能访问（After）时间往后写一小时
            - After=2020-06-17T12:53:40.325+08:00[Asia/Shanghai]
12
```

测试，启动7001，8001，8002，9527
`http://localhost:9527/payment/lb`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-552.png)

Before和Between作用一样：

```yml
			#指定时间前才能访问（Before）
            - Before=2020-06-17T11:53:40.325+08:00[Asia/Shanghai]
            #指定时间内才能访问（Between）
            - Between=2020-06-17T11:53:40.325+08:00[Asia/Shanghai],2020-06-17T12:53:40.325+08:00[Asia/Shanghai]
1234
```

### 6.6.2 Cookie

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-553.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200617152501632.png)
在yml中的`predicates:`加上（记得把after的时间改成已经过去的时间，时间没到访问不了）

```yml
          - Cookie=username,angenin   #带Cookie，并且username的值为angenin
1
```

1. 不带cookie访问
   打开终端，输入`curl http://localhost:9527/payment/lb`（直接访问失败）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-554.png)
2. 带cookie访问
   输入`curl http://localhost:9527/payment/lb --cookie "username=angenin"`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-555.png)

### 6.6.3 Header

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-556.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200617155209547.png)
注释掉其他两个，加上Header

```yml
#            - After=2020-06-17T12:53:40.325+08:00[Asia/Shanghai]
#            - Cookie=username,angenin   #带Cookie，username的值为angenin
            - Header=X-Request-Id, \d+   #请求头要有 X-Request-Id属性并且值为整数的正则表达式
123
```

重启9527，然后在终端输入`http://localhost:9527/payment/lb -H "X-Request-Id:123"`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-557.png)

### 6.6.4 Host

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-558.png)
加上：

```yml
            - Host=**.angenin.com	#Host: xxx.angenin.com 请求是Host必须有**.angenin.com
1
```

重启9527
`http://localhost:9527/payment/lb -H "Host: www.angenin.com"`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-559.png)

### 6.6.5 Method

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-560.png)

```yml
- Method=GET	#只允许get请求访问
```

### 6.6.6 Path

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-561.png)

```yml
			#访问的url地址有 /payment/lb/ 才能访问
			- Path=/payment/lb/**	
12
```

已经用过了，这里不进行演示。

### 6.6.7 Query

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-562.png)

```yml
            - Query=username, \d+   #url请求地址必须带上username参数，并且值必须为整数
1
```

`http://localhost:9527/payment/lb?username=110`
![在这里插入图片描述](尚硅谷-SpringCloud/20200617163931254.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-563.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586831-564.png)

### 6.6.8 总结

![在这里插入图片描述](尚硅谷-SpringCloud/2020061716423856.png)

## 6.7 Filter的使用

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586832-565.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586832-566.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586832-567.png)
GatewayFilter（31种）
Global Filter（10种）

这里以`AddRequestParameter`为代表。

### 6.7.1 自定义过滤器

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586832-568.png)
新建filter.MyLogGateWayFilter

```java
@Component
@Slf4j
public class MyLogGateWayFilter implements GlobalFilter, Ordered {


    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("**************come in MyLogGateWayFilter：" + new Date());
        //获取request中的uname参数
        String uname = exchange.getRequest().getQueryParams().getFirst("uname");

        if(uname == null){
            log.info("*******用户名为null，非法用户！！");
            //设置响应，不被接受
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);

            return exchange.getResponse().setComplete();
        }

        //返回chain.filter(exchange)，放行
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        //返回值是过滤器的优先级，越小优先级越高（最小-2147483648，最大2147483648）
        return 0;
    }
}
1234567891011121314151617181920212223242526272829
```

启动7001，8001，8002，9527
`http://localhost:9527/payment/lb?uname=111`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586832-569.png)
`http://localhost:9527/payment/lb?xxx=111`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682148586832-570.png)

# 7 SpringCloud Config分布式配置中心

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348937-697.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-698.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-699.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-700.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-701.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200617210752450.png)

## 7.1 Config服务端配置与测试

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-702.png)

1. 在GitHub上新建一个名为springcloud-config的新Repository（需要是public的仓库，private的访问不了）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-703.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-704.png)
   创建后，点击复制，然后黏贴到终端（可以先新建一个springcloud2020目录，等下用来clone到本地的存放目录）。（需要在本地安装好git）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-705.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-706.png)
   此时，这个springcloud2020就是我们本地的一个新的git仓库了（git init）。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200617214131459.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348938-707.png)
   创建myorder文件夹
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-708.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-709.png)
   创建视频中的yml文件
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617213626943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70)![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-710.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-711.png)

2. clone到本地，复制地址，然后终端进入springcloud2020文件夹里，输入`git clone 你们的仓库地址`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-712.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-713.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-714.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-715.png)

3. 新建模块cloud-config-center-3344

4. pom

   ```xml
   <dependencies>
       <!--config server-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-config-server</artifactId>
       </dependency>
       <!--eureka client(通过微服务名实现动态路由)-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   12345678910111213141516171819202122232425262728293031323334353637
   ```

5. yml
   点击可以切换为ssh模式的地址（就是视频中的@git… ，不过ssh需要先配置才能使用，所以用https模式的地址比较方便）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-716.png)

   ```yml
   server:
     port: 3344
   
   
   spring:
     application:
       name: cloud-config-center #注册进Eureka服务器的微服务名
     cloud:
       config:
         server:
           git:
             uri: https://github.com/angenin/springcloud-config.git  #git的仓库地址
             search-paths:   #搜索目录
               - springcloud-config
         label: master   #读取的分支
   
   
   eureka:
     client:
       service-url: 
         defaultZone: http://localhost:7001/eureka   #服务注册到的eureka地址
   
   12345678910111213141516171819202122
   ```

6. 主启动类

   ```java
   @EnableConfigServer
   @SpringBootApplication
   public class ConfigCenterMain3344 {
       
       public static void main(String[] args) {
           SpringApplication.run(ConfigCenterMain3344.class, args);
       }
       
   }
   123456789
   ```

7. 修改hosts文件，增加映射
   终端输入`sudo vim /private/etc/hosts`，再输入本机密码，按i进入编辑模式，在最下行加入

   ```shell
   127.0.0.1       config-3344.com
   1
   ```

   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-717.png)
   按esc键，然后输入`:wq!`强制保存退出。

8. 在GitHub中的配置文件加入（点击这个文件，然后点击笔形状的按钮进行编辑）

   ```yml
   #config-dev.yml
   config:
     info: "master branch,springcloud-config/config-dev.yml version=1"
     
   #######################################
   
   #config-prod.yml
   config:
     info: "master branch,springcloud-config/config-prod.yml version=1"
   
   #######################################
   
   #config-test.yml
   config:
     info: "master branch,springcloud-config/config-test.yml version=1"
   123456789101112131415
   ```

9. 启动7001，3344，然后在浏览器输入`http://config-3344.com:3344/master/config-dev.yml`（成功获取到github上的配置文件数据）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-718.png)

### 7.1.1 配置的读取规则

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-719.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-720.png)
第一种（我们上面用的就是这一种）
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-721.png)
`http://config-3344.com:3344/master/config-test.yml`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-722.png)
第二种（分支不写，默认master分支）
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-723.png)
`http://config-3344.com:3344/config-test.yml`
![在这里插入图片描述](尚硅谷-SpringCloud/20200617230135830.png)
第三种（反着写）
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348939-724.png)
`http://config-3344.com:3344/config/test/master`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-725.png)

## 7.2 Config客户端配置与测试

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-726.png)

1. 新建模块cloud-config-client-3355

2. pom

   ```xml
   <dependencies>
       <!--config server-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-config</artifactId>
       </dependency>
       <!--eureka client(通过微服务名实现动态路由)-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   12345678910111213141516171819202122232425262728293031323334353637
   ```

3. bootstrap.yml（系统级别的配置文件）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-727.png)

   ```yml
   server:
     port: 3355
   
   
   spring:
     application:
       name: config-client
     cloud:
       config: #config客户端配置
         label: master   #分支名称
         name: config    #配置文件名称       这三个综合：master分支上的config-dev.yml的配置文件
         profile: dev    #读取后缀名称       被读取到http://config-3344.com:3344/master/config/dev
         uri: http://localhost:3344  #配置中心地址
   
   
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka   #服务注册到的eureka地址
   12345678910111213141516171819
   ```

4. 主启动类

   ```java
   @EnableEurekaClient
   @SpringBootApplication
   public class ConfigClientMain3355 {
       
       public static void main(String[] args) {
           SpringApplication.run(ConfigClientMain3355.class, args);
       }
       
   }
   123456789
   ```

5. controller（读取GitHub的配置文件）

   ```java
   @RestController
   public class ConfigClientController {
   
       @Value("${config.info}")	//spring的@Value注解
       private String configInfo;
   
       @GetMapping("/configInfo")
       public String getConfigInfo(){
           return configInfo;
       }
   
   }
   123456789101112
   ```

6. 测试，启动7001，3344，3355
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-728.png)
   3344测试，`http://config-3344.com:3344/master/config-dev.yml`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-729.png)
   3355测试，`http://localhost:3355/configInfo`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-730.png)

### 7.2.1动态刷新问题

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-731.png)

1. 修改GitHub上的config-dev.yml文件的版本号为2。
2. 刷新`http://config-3344.com:3344/master/config-dev.yml`，版本号发生改变。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200618100518819.png)
3. 刷新`http://localhost:3355/configInfo`，没有改变。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200618100603408.png)
4. 重启3355，刷新`http://localhost:3355/configInfo`，读取到最新的版本号。
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200618100734915.png)

## 7.3 Config客户端之动态刷新

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-732.png)

1. 往config客户端3355在pom中添加（上面已经加了）

   ```xml
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
   ```

2. 然后在bootstrap.yml中添加

   ```yml
   #暴露监控端点
   management:
     endpoints:
       web:
         exposure:
           include: "*"
   ```

3. 在ConfigClientController类上加上`@RefreshScope`注解。

4. 重启3355。

5. 修改GitHub上文件的版本号，然后访问3344和3355。（启动完成后再修改）
   `http://config-3344.com:3344/master/config-dev.yml`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-733.png)
   `http://localhost:3355/configInfo`（没读取到，需要发送post请求刷新3355才能生效）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-734.png)

6. 打开终端，输入`curl -X POST "http://localhost:3355/actuator/refresh"`
   ![在这里插入图片描述](尚硅谷-SpringCloud/2020061810264983.png)

7. 刷新`http://localhost:3355/configInfo`
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-735.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-736.png)

# 8 SpringCloud Bus消息总线

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-737.png)
Bus支持两种消息代理：RabbitMQ和Kafka。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-738.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348940-739.png)
RabbitMQ粗浅的理论和使用的[学习笔记](https://blog.csdn.net/qq_36903261/article/details/106517697)，有兴趣可以看一下。

## 8.1 Docker安装RabbitMQ

[Docker基础入门学习笔记](https://blog.csdn.net/qq_36903261/article/details/105870268)，有兴趣的可以看一下。

在linux的docker里拉取RabbitMQ镜像`docker pull rabbitmq:3.8.3-management`（management是带web的管理界面）。
5672是客户端和RabbitMQ进行通信的端口。
15672是管理界面访问web页面的端口。

运行RabbitMQ

```shell
docker run -d -p 5672:5672 -p 15672:15672 --name myRabbitMQ 容器id
1
```

在浏览器中输入`http://10.211.55.17:15672/`访问RabbitMQ的管理页面，用户名和密码默认guest。（10.211.55.17是我linux的IP地址）
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-740.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-741.png)

## 8.2 SpringCloud Bus动态刷新全局广播

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-742.png)

按照3355新建3366。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-743.png)
利用消息总线触发一个客户端/bus/refresh而刷新所有客户端的配置：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-744.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-745.png)
利用消息总线触发一个服务端 ConfigServer的/bus/refresh端点,而刷新所有客户端的配置：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-746.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-747.png)
图二的架构显然更加适合，图一不适合的原因如下：
![在这里插入图片描述](尚硅谷-SpringCloud/20200618115036173.png)

### 8.2.1 给服务端3344添加消息总线支持

pom添加：

```xml
  <!--添加消息总线RabbitMQ的支持-->
  <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-bus-amqp</artifactId>
 </dependency>
12345
```

yml添加：

```yml
  rabbitmq:
    host: 10.211.55.17  #本机写localhost，服务器的写服务器地址
    port: 5672   #客户端和RabbitMQ进行通信的端口
    username: guest #默认也是guest
    password: guest #默认也是guest


#RabbitMQ相关配置
management:
  endpoints:  #暴露bus刷新配置的端点
    web:
      exposure:
        include: 'bus-refresh'
12345678910111213
```

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-748.png)

### 8.2.2 给客户端3355和3366添加消息总线支持

pom添加：

```xml
  <!--添加消息总线RabbitMQ的支持-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-bus-amqp</artifactId>
  </dependency>
12345
```

yml添加：

```yml
  rabbitmq:
    host: 10.211.55.17  #本机写localhost，服务器的写服务器地址
    port: 5672   #客户端和RabbitMQ进行通信的端口
    username: guest #默认也是guest
    password: guest #默认也是guest
12345
```

spring的下一级，不要写错了。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-749.png)

### 8.2.3 测试

1. 启动7001，3344，3355，3366。
2. 修改GitHub上文件的版本号。
3. 在终端输入：`curl -X POST "http://localhost:3344/actuator/bus-refresh"`
4. 在浏览器输入`http://localhost:3355/configInfo`，`http://localhost:3366/configInfo`
   ![在这里插入图片描述](尚硅谷-SpringCloud/20200618143639248.png)
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-750.png)

项目先不停止，下面要用。

## 8.3 SpringCloud Bus动态刷新定点通知

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-751.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348941-752.png)

1. 修改GitHub的文件版本号。
2. 在终端输入：`curl -X POST "http://localhost:3344/actuator/bus-refresh/config-client:3355"`
   *多加了服务名:端口号即可定点通知。*

`http://localhost:3355/configInfo`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348942-753.png)
`http://localhost:3366/configInfo`
![在这里插入图片描述](尚硅谷-SpringCloud/20200618145348944.png)

总结：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159348942-754.png)

# 9 SpringCloud [Stream](https://so.csdn.net/so/search?q=Stream&spm=1001.2101.3001.7020)消息驱动

## 9.1 消费驱动概述

### 9.1.1 是什么

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777013-891.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777013-892.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777013-893.png)
官网：https://spring.io/projects/spring-cloud-stream#overview
![在这里插入图片描述](尚硅谷-SpringCloud/20200618151932271.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777013-894.png)
API：https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/

中文指导手册：https://m.wang1314.com/doc/webapp/topic/20971999.html

### 9.1.2 设计思想

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777013-895.png)
标准MQ：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777013-896.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777013-897.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200618183412345.png)

为什么用Cloud Stream？
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-898.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200618183618608.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-899.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-900.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-901.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-902.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200618184037800.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200618184152655.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-903.png)

### 9.1.3 Spring Cloud Stream标准流程套路

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-904.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-905.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777014-906.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200618184807765.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200618184820843.png)

### 9.1.4 编码API和常用注解

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-907.png)

## 9.2 案例说明

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-908.png)

## 9.3 消息驱动之生产者

1. 新建模块cloud-stream-rabbitmq-provider8801

2. pom

   ```xml
   <dependencies>
       <!--stream rabbit -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
       </dependency>
       <!--eureka client-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <!--监控-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   1234567891011121314151617181920212223242526272829303132333435363738
   ```

3. yml

   ```yml
   server:
     port: 8801
   
   spring:
     application:
       name: cloud-stream-provider
     cloud:
       stream:
         binders: #在此处配置要绑定的rabbitmq的服务信息
           defaultRabbit: #表示定义的名称，用于binding整合
             type: rabbit #消息组件类型
             environment: #设置rabbitmq的相关环境配置
               spring:
                 rabbitmq:
                   host: 10.211.55.17  #RabbitMQ在本机的用localhost，在服务器的用服务器的ip地址
                   port: 5672
                   username: guest
                   password: guest
         bindings: #服务的整合处理
           output: #这个名字是一个通道的名称
             destination: studyExchange #表示要使用的Exchange名称定义
             content-type: application/json #设置消息类型，本次为json，本文要设置为“text/plain”
             binder: defaultRabbit #设置要绑定的消息服务的具体设置（爆红不影响使用，位置没错）
   
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka
     instance:
       lease-renewal-interval-in-seconds: 2 #设置心跳的时间间隔（默认是30S)
       lease-expiration-duration-in-seconds: 5 #如果超过5S间隔就注销节点 默认是90s
       instance-id: send-8801.com #在信息列表时显示主机名称
       prefer-ip-address: true #访问的路径变为IP地址
   123456789101112131415161718192021222324252627282930313233
   ```

4. 主启动类

   ```java
   @SpringBootApplication
   public class StreamMQMain8801 {
   
       public static void main(String[] args) {
           SpringApplication.run(StreamMQMain8801.class, args);
       }
   
   }
   12345678
   ```

5. 业务类
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-909.png)

   1. 新建service.IMessageProvider接口

      ```java
      public interface IMessageProvider {
          public String send();
      }
      123
      ```

   2. 在service下新建impl.IMessageProviderImpl实现类

      ```java
      import com.angenin.springcloud.service.IMessageProvider;
      import org.springframework.cloud.stream.annotation.EnableBinding;
      import org.springframework.cloud.stream.messaging.Source;
      import org.springframework.integration.support.MessageBuilder;
      import org.springframework.messaging.MessageChannel;
      import javax.annotation.Resource;
      import java.util.UUID;
      
      @EnableBinding(Source.class)    //定义消息的推送管道（Source是spring的）
      public class IMessageProviderImpl implements IMessageProvider {
      
          @Resource
          private MessageChannel output;  //消息发送管道
      
          @Override
          public String send() {
              String serial = UUID.randomUUID().toString();
              output.send(MessageBuilder.withPayload(serial).build());     //MessageBuilder是spring的integration.support.MessageBuilder
              System.out.println("*******serial: " + serial);
              return null;
          }
      }
      12345678910111213141516171819202122
      ```

   3. 新建controller.SendMessageController

      ```java
      @RestController
      public class SendMessageController {
      
          @Resource
          private IMessageProvider iMessageProvider;
      
          @GetMapping("/sendMessage")
          public String sendMessage(){
              return iMessageProvider.send();
          }
      
      }
      123456789101112
      ```

6. 测试
   启动7001，RabbitMQ，启动8801

   然后在RabbitMQ后台可以看到新生成的交换机（在yml定义的）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-910.png)
   然后在浏览器输入：`http://localhost:8801/sendMessage`，多次刷新，后台打印的数据：
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-911.png)
   RabbitMQ后台：
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-912.png)

## 9.4 消息驱动之消费者

1. 新建模块cloud-stream-rabbitmq-consumer8802

2. pom

   ```xml
   <dependencies>
       <!--stream rabbit -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
       </dependency>
       <!--eureka client-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <!--监控-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   1234567891011121314151617181920212223242526272829303132333435363738
   ```

3. yml

   ```yml
   server:
     port: 8802
   
   spring:
     application:
       name: cloud-stream-provider
     cloud:
       stream:
         binders: #在此处配置要绑定的rabbitmq的服务信息
           defaultRabbit: #表示定义的名称，用于binding整合
             type: rabbit #消息组件类型
             environment: #设置rabbitmq的相关环境配置
               spring:
                 rabbitmq:
                   host: 10.211.55.17  #RabbitMQ在本机的用localhost，在服务器的用服务器的ip地址
                   port: 5672
                   username: guest
                   password: guest
         bindings: #服务的整合处理
           input: #这个名字是一个通道的名称
             destination: studyExchange #表示要使用的Exchange名称定义
             content-type: application/json #设置消息类型，本次为json，本文要设置为“text/plain”
             binder: defaultRabbit #设置要绑定的消息服务的具体设置（爆红不影响使用，位置没错）
   
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka
     instance:
       lease-renewal-interval-in-seconds: 2 #设置心跳的时间间隔（默认是30S)
       lease-expiration-duration-in-seconds: 5 #如果超过5S间隔就注销节点 默认是90s
       instance-id: receive-8802.com #在信息列表时显示主机名称
       prefer-ip-address: true #访问的路径变为IP地址
   123456789101112131415161718192021222324252627282930313233
   ```

4. 主启动类

   ```java
   @SpringBootApplication
   public class StreamMQMain8802 {
   
       public static void main(String[] args) {
           SpringApplication.run(StreamMQMain8802.class, args);
       }
   
   }
   12345678
   ```

5. 新建controller.ReceiveMessageListenerController

   ```java
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   import org.springframework.messaging.Message;
   import org.springframework.stereotype.Controller;
   
   
   @EnableBinding(Sink.class)
   @Controller
   public class ReceiveMessageListenerController {
   
       @Value("${server.port}")
       private String serverPort;
   
       @StreamListener(Sink.INPUT) //监听
       public void input(Message<String> message){
           System.out.println("消费者1号------>收到的消息：" + message.getPayload() + "\t port：" + serverPort);
       }
   
   }
   123456789101112131415161718192021
   ```

6. 测试
   启动7001，8801，8802

`http://localhost:8801/sendMessage`（8801发送消息）
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-913.png)
8802接收到消息：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-914.png)

## 9.5 分组消费与持久化

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-915.png)
按照8802，新建8803。

### 9.5.1 消费

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777015-916.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200619091611259.png)
8801发送后，8802和8803都能接收到数据（重复消费）；正常情况下应该是有一个消费者消费了8801的消息后，另外的其他消费者就不能消费。这里要把8802和8803看成一个集群，如果8802和8803都接收到了，就都会去做业务，然后本来8801只想让这个集群来做一次消费的，就会变成每个消费者都来消费一次。这是因为8802和8803不是在同一个组（队列）里，不同组可以重复消费，而同一个组里，只有一个消费者能消费，所以需要对消费者进行分组，把所有相同的消费者分到一个组里。（主题会给每个队列发送消息，而每个队列只有一个消费者可以获得消息（同组广播，不同组轮询））

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-917.png)![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-918.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-919.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-920.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-921.png)

### 9.5.2 分组(队列)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-922.png)

##### 设置不同分组

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-923.png)
修改8802的yml
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-924.png)
修改8803的yml
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-925.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-926.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-927.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-928.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-929.png)
结论：
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-930.png)

##### 设置相同分组

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777016-931.png)
修改8803的yml中group为angeninA，然后重启8003。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-932.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-933.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-934.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-935.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-936.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200619155623141.png)

### 9.5.3 持久化

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-937.png)

1. 停掉8802和8803，去掉8802的`group: angeninA`。
2. 然后8801发送4条消息。
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-938.png)
3. 启动8802，8802并没有去拿取消息。（因为8802去掉了`group: angeninA`，所以启动后会再新建一个队列）
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-939.png)
4. 启动8803，启动后获取到8801的消息。（因为8803没删除`group: angeninA`，angeninA队列是在8801发送消息前存在的，所以当8803停机后再启动，就可以获取到停机时8801发送的信息（如果此时同组（队列）里有别的消费者，那么消息会被别的消费者消费掉））
   ![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-940.png)

# 10 SpringCloud Sleuth[分布式](https://so.csdn.net/so/search?q=分布式&spm=1001.2101.3001.7020)请求链路追踪

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-941.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-942.png)
https://github.com/spring-cloud/spring-cloud-sleuth

https://cloud.spring.io/spring-cloud-sleuth/reference/html/

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-943.png)

## 10.1 搭建链路监控步骤

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-944.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-945.png)
下载jar包：http://dl.bintray.com/openzipkin/maven/io/zipkin/java/zipkin-server/
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777017-946.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-947.png)

下载完后，终端jar包的目录里，然后输入：`java -jar zipkin-server-2.12.9-exec.jar`运行。
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-948.png)

浏览器输入：`http://localhost:9411/zipkin/`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-949.png)

### 10.1.1 原理

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-950.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-951.png)
![在这里插入图片描述](尚硅谷-SpringCloud/20200619164359860.png)

### 10.1.2 服务提供者cloud-provider-payment8001

1. 在pom中添加：

   ```xml
      <!--包含了sleuth+zipkin-->
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-zipkin</artifactId>
      </dependency>
   12345
   ```

2. 在yml中添加：

   ```yml
     zipkin:
       base-url: http://localhost:9411
     sleuth:
       sampler:
         probability: 1  #采样率值介于0到1之间，1则表示全部采集（一般不为1，不然高并发性能会有影响）
   12345
   ```

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-952.png)

1. 在PaymentController中添加：

   ```java
      @GetMapping("/payment/zipkin")
      public String paymentZipkin(){
          return "paymentZipkin...";
      }
   1234
   ```

### 10.1.3 服务消费者cloud-consumer-order80

1. 在pom中添加（和提供者一样）

2. 在yml中添加（和提供者一样）

3. 在OrderController中添加：

   ```java
       @GetMapping("/consumer/payment/zipkin")
       public String paymentZipkin(){
           String result = restTemplate.getForObject("http://localhost:8001" + "/payment/zipkin", String.class);
           return result;
       }
   12345
   ```

### 10.1.4 测试

启动7001，8001，80。

浏览器输入：`http://localhost/consumer/payment/zipkin`
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-953.png)

![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-954.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-955.png)
![在这里插入图片描述](尚硅谷-SpringCloud/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMjYx,size_16,color_FFFFFF,t_70-1682159777018-956.png)
