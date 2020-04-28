# Spring Cloud

**负载均衡：`Ribbon`**

在使用`Ribbon`作为负载均衡的时候，需要手动注入一个`RestTemplate`的Bean到Ioc，并且为该Bean加入`@LoadBalanced`开启负载均衡，使其能够以查询到服务注册中心的服务。

例如：

```java
@Bean
@LoadBalanced
public RestTemplate getRestTemplate(){
	return new RestTemplate();
}
```

---

**负载均衡：`Feign`**

使用`Feign`作为负载均衡的时候，需要先在启动类使用`@EnableFeignClients`来开启，而后创建一个接口，使用`@FeignClient(value = "DATA-SERVER")`标签来指定访问的服务

例如：

```java
@FeignClient(value = "DATA-SERVER")
public interface IProductClientFeign {
    /**
     * 请求数据
     * @return 结果
     */
    @GetMapping("/product/all")
    List<Product> allProduct();
}
```

---

**配置服务`ConfigServer`**

pom配置服务包

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

配置文件`properties`，配置服务参数

```properties
server.port=8030
spring.application.name=config-server
eureka.client.service-url.defaultZone=http://localhost:8080/eureka
spring.cloud.config.label=master
spring.cloud.config.server.git.uri=https://github.com/Huangjie-Github/spring-cloud-config-server
spring.cloud.config.server.git.search-paths=config
spring.cloud.config.server.git.username=Huangjie-Github
spring.cloud.config.server.git.password=h19981228j
```

在启动类上开启服务

```java
@EnableEurekaClient
@EnableConfigServer
@EnableDiscoveryClient
```

---

**使用`ConfigServer`**

pom引入服务使用包

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

配置文件中声明配置服务参数

**注意：**在使用配置服务的时候，下列参数一定要新建一个bootstrap.properties/bootstrap.yml的配置文件，因为bootstrap的配置文件会比application的配置文件先启动加载，需要先启动加载好配置服务才能进行本服务的相关数据注入

```properties
#分支
spring.cloud.config.label=master
#启用的模式
spring.cloud.config.profile=dev
#是否发现启用
spring.cloud.config.discovery.enabled=true
#注册中心的名称
spring.cloud.config.discovery.serviceId=CONFIG-SERVER
eureka.client.service-url.defaultZone=http://localhost:8080/eureka
```

配置之后，只需要在引用的类上使用`@RefreshScope`标签开启，需要引用的参数上使用`@Value`标签即可

---



**消息总线bus：`RabbitMQ`**

pom使用jar

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>   
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency> 
```

配置文件修改

bootstrap.properties

```properties
#与配置服务绑定
spring.cloud.config.label=master
spring.cloud.config.profile=dev
spring.cloud.config.discovery.enabled=true
spring.cloud.config.discovery.serviceId=CONFIG-SERVER
eureka.client.service-url.defaultZone=http://localhost:8080/eureka
#bus总线配置
spring.cloud.bus.enabled=true
spring.cloud.bus.trace.enabled=true
#配置RabbitMQ
spring.rabbitmq.host=127.0.0.1
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```

application.properties

```properties
server.port=8011
spring.application.name=feign-server
#路线访问允许/actuator/refresh
management.endpoints.web.exposure.include=*
management.endpoints.web.cors.allowed-origins=*
management.endpoints.web.cors.allowed-methods=*
```

访问需要更新的微服务http://localhost:801/actuator/refresh

该访问不支持Get访问，只支持Post访问

```java
public static void main(String[] args) {
        HashMap<String,String> headers =new HashMap<>();
        headers.put("Content-Type", "application/json; charset=utf-8");
        System.out.println("因为要去git获取，还要刷新config-server, 会比较卡，所以一般会要好几秒才能完成，请耐心等待");
 
        String result = HttpUtil.createPost("http://localhost:8012/actuator/bus-refresh").addHeaders(headers).execute().body();
        System.out.println("result:"+result);
        System.out.println("refresh 完成");
    }
```



