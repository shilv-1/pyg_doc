# 商城前台&Redis缓存

## 03_网站前台分析

#### 目标

> 了解网站前台有哪些页面?

#### 小结

> 首页、搜索页、商品详情页、购物车页、支付页、订单页、用户中心页、秒杀页等。

## 04_搭建门户工程

#### 目标

> 搭建门户工程

#### 搭建步骤

> 参考课程讲义

#### 小结

![1562634076546](assets/1562634076546.png)

> pinyougou-portal-web访问域名: http://www.pinyougou.com 或 http://pinyougou.com

## 05_广告展示需求分析

#### 目标

> 如何在首页显示广告数据?

#### 需求分析

+ 前端代码

  + 引入js
  + 定义异步请求方法，获取响应数据，存入data中
  + 页面迭代(产生圆点、广告图片)

+ 后端核心部分

  SELECT * FROM tb_content WHERE category_id = 1 AND STATUS = 1 ORDER BY sort_order ASC

  List<Content> --> [{},{}]

#### 小结

> SELECT * FROM tb_content WHERE category_id = 1 AND STATUS = 1 ORDER BY sort_order ASC

## 06_广告展示(前端代码)

#### 目标

> 实现广告展示前端代码

#### 核心代码

![1562634935646](assets/1562634935646.png)

![1562635190227](assets/1562635190227.png)

## 07_广告展示(后端代码)

#### 目标

> 实现广告展示后端代码

#### 操作步骤

- 控制器
- 服务接口
- 服务接口实现类
- 数据访问接口

#### 小结

![1562635639462](assets/1562635639462.png)

## 08_项目常见问题思考

#### 目标

> 如何解决数据库访问压力大的问题? mysql访问压力大
>
> 10w /1s

#### 解决方案

+ 第一种: 数据缓存(Redis数据库)
+ 第二种: 网页静态化

#### 小结

> 数据缓存: 首选Redis数据库(内存存储)。
>
> 网页静态化: 利用FreeMarker把动态页面转化成静态的html页面。

## 09_Linux安装Redis单机版

#### 目标

> 介绍Linux系统如何安装Redis单机版

#### 具体安装

> 参考【Linux安装Redis单机版.docx】

#### 小结

> Redis数据库默认端口号为: 6379
>
> ![1562638342481](assets/1562638342481.png)

## 10_Spring Data Redis介绍

#### 目标

> 了解Spring Data Redis?

#### 小结

> Spring Data Redis框架方便我们在Spring应用中操作Redis数据库。

## 11_SpringDataRedis整合Jedis

#### 目标

> 完成SpringDataRedis整合Jedis。

#### 整合步骤

+ 配置依赖jar包

```xml
<!-- spring-data-redis -->
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>1.8.6.RELEASE</version>
</dependency>
<!-- jedis -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>
```

+ 提供applicationContext-redis.xml配置spring-data-redis整合jedis

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 加载属性文件 -->
    <context:property-placeholder location="classpath:redis-config.properties"/>

    <!-- ######## 配置Spring-Data-Redis整合Jedis(单机版) ######## -->
    <!-- 配置连接工厂 -->
    <bean id="connectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <property name="hostName" value="${redis.hostName}"/>
        <property name="port" value="${redis.port}"/>
    </bean>

    <!-- 配置RedisTemplate操作Redis数据库 -->
    <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <!-- 设置连接工厂 -->
        <property name="connectionFactory" ref="connectionFactory"/>
    </bean>
</beans>
```

#### 小结

> 关键点: Spring容器中要有RedisTemplate对象。

## 12_SpringDataRedis操作Redis(单机版)

#### 目标

> 完成SpringDataRedis操作Redis单机版代码

#### 操作Redis四种数据类型

+ 字符串类型
+ set类型
+ list类型
+ hash类型

#### 小结

> + redisTemplate.boundValueOps(key).xxx();
>
> + redisTemplate.boundSetOps(key).xxx();
>
> + redisTemplate.boundListOps(key).xxx();
>
> + redisTemplate.boundHashOps(key).xxx();
>
>   ![1562639527946](assets/1562639527946.png)

## 13_Redis集群原理介绍

#### 目标

> 介绍Redis集群原理、分布式缓存。
>
> Redis-Cluster: Redis集群 | 分布式缓存.
>
> 集群: 多个节点node(代表一台Redis)
>
> master: 主节点
>
> slave: 从节点

#### 具体文档

> 参考【Linux安装Redis单机版.docx】

#### 小结

> Redis Cluster分布式缓存： 最多16384台Redis服务器、最少6台Redis服务器

## 14_客户端连接Redis集群

#### 目标

> 实现客户端连接Redis集群

#### 操作步骤

+ redis客户端命令连接
+ RedisDesktopManager客户端软件连接

#### 小结

> redis-cli -h 192.168.12.131 -p 7006 -c

## 15_SpringDataRedis操作Redis(集群版)

#### 目标

> 使用SpringDataRedis操作Redis集群版

#### 操作步骤

+ 配置依赖jar

  ```xml
  <!-- spring-data-redis -->
  <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-redis</artifactId>
      <version>1.8.6.RELEASE</version>
  </dependency>
  <!-- jedis -->
  <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>2.9.0</version>
  </dependency>
  ```

+ 配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd">
      
  
      <!-- ######## 配置Spring-Data-Redis整合Jedis(集群版) ######## -->
      <!-- 配置属性源(集群节点) -->
      <bean id="propertySource" class="org.springframework.core.io.support.ResourcePropertySource">
          <constructor-arg name="location" value="classpath:redis-cluster.properties"/>
      </bean>
  
      <!-- 配置clusterConfig集群信息对象 -->
      <bean id="clusterConfig" class="org.springframework.data.redis.connection.RedisClusterConfiguration">
          <!-- 设置集群节点信息(属性文件) -->
          <constructor-arg name="propertySource" ref="propertySource"/>
      </bean>
  
      <!-- 配置连接工厂 -->
      <bean id="connectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
          <!-- 设置集群配置信息 -->
          <constructor-arg name="clusterConfig" ref="clusterConfig"/>
      </bean>
  
      <!-- 配置RedisTemplate操作Redis数据库 -->
      <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
          <!-- 设置连接工厂 -->
          <property name="connectionFactory" ref="connectionFactory"/>
      </bean>
  </beans>
  
  
  # 配置集群中的节点信息 host:port,host:port
  spring.redis.cluster.nodes=192.168.12.131:7001,192.168.12.131:7002,192.168.12.131:7003,192.168.12.131:7004,192.168.12.131:7005,192.168.12.131:7006
  ```

+ 代码部分

  > 与操作Redis单机版相同。

#### 小结

> 高并发: 三个主节点
>
> 高可用: 三个从节点

## 16_品优购集成SpringDataRedis

#### 目标

> 实现品优购项目集成SpringDataRedis访问Redis单机版或Redis集群版。
>
> pinyougou-common公共层整合spring-data-redis(单机版与集群版)

#### 整合步骤

+ 配置依赖jar

  ```xml
  <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-redis</artifactId>
  </dependency>
  ```

+ 提供整合的spring配置文件

#### 小结

> 提供applicationContext-redis.xml配置文件整合Redis单机版或Redis集群版。

## 17_增加广告缓存

#### 目标

> 实现增加广告缓存功能
>
> pinyougou-content-service整合redis

#### 操作步骤

+ 导入applicationContext-redis.xml
+ 注入RedisTemplate

#### 小结

> 往Redis数据库增加广告数据 
>
> + key: content 
> + value: List<Content>

## 18_更新广告缓存

#### 目标

> 实现更新广告缓存功能

#### 操作步骤

+ 添加广告更新缓存
+ 修改广告更新缓存
+ 删除广告更新缓存

#### 小结

> 从Redis数据库中删除广告缓存数据对应的key。
