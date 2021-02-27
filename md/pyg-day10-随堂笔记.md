# ElasticSearch&数据导入&搜索系统

## 03_商品搜索思考

#### 目标

> 了解高并发下商品搜索解决方案
>
> 商城的搜索系统: 商品的搜索数据不能来自数据库?
>
> + 搜索系统是高并发系统(如果数据来自数据库，数据库的访问压力很大，商品数据很多不适合用Redis数据).
> + 数据库表中的列是整列创建索引，不能分词创建索引

#### 搜索解决方案

+ ElasticSearch搜索服务(分布式)
+ Solr搜索服务(不是分布式，需要用zookeeper才可以做成分布式)

#### 小结

> ES实时搜索性能更好，Solr实时搜索一般。

## 04_Linux安装ElasticSearch

#### 目标

> 在Linux系统中安装ElasticSearch

#### 小结

> 具体参考安装文档 [Linux安装ElasticSearch.docx][资料/Solr相关资料/Linux安装Solr.docx]

## 05_Linux部署ES-Head

#### 目标

> 在Linux系统中部署ES-Head

#### 小结

> 具体参考安装文档 [Linux安装ElasticSearch.docx][资料/Solr相关资料/Linux安装Solr.docx]

## 06_ElasticSearch集成IK分词器

#### 目标

> 完成ElasticSearch集成IK分词器

#### 小结

> 具体参考安装文档 [Linux安装ElasticSearch.docx][资料/Solr相关资料/Linux安装Solr.docx]

## 07_搭建SpringDataES测试模块

#### 目标

> 完成SpringDataElasticSearch测试模块搭建

#### 搭建步骤

+ 创建spring-data-es-test模块

+ 配置依赖jar

  ```xml
  <!-- spring-data-elasticsearch -->
  <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-elasticsearch</artifactId>
      <version>3.1.3.RELEASE</version>
  </dependency>
  ```

+ 提供log4j2.xml日志文件

#### 小结

> SpringDataElasticSearch框架的作用方便我们在Spring项目中操作ElasticSearch服务。

## 08_Spring整合SpringDataES

#### 目标

> 完成Spring整合SpringDataElasticSearch
>
> ![1562810916688](assets/1562810916688.png)
>
> ![1562810908942](assets/1562810908942.png)

#### 整合步骤

- 定义实体类

- 在applicationContext-elasticsearch.xml文件中配置SpringDataElasticsearch

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:elasticsearch="http://www.springframework.org/schema/data/elasticsearch"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/data/elasticsearch
          http://www.springframework.org/schema/data/elasticsearch/spring-elasticsearch.xsd">
  
      <!-- 配置Spring-Data-ElasticSearch -->
      <!-- 2. 配置传输客户端对象
          cluster-name: 集群名称
           cluster-nodes: 集群节点信息，多个用逗号分隔
       -->
      <elasticsearch:transport-client id="client"
                                      cluster-name="elasticsearch"
                                      cluster-nodes="192.168.12.131:9300"/>
  
      <!-- 1.配置ElasticsearchTemplate对象，对es服务做所有的操作 -->
      <bean id="elasticsearchTemplate" class="org.springframework.data.elasticsearch.core.ElasticsearchTemplate">
          <!-- 设置传输客户端对象 -->
          <constructor-arg name="client" ref="client"/>
      </bean>
  
      <!-- 3. 配置索引库数据访问接口(采用包扫描) -->
      <elasticsearch:repositories base-package="cn.itcast.es.dao"/>
  </beans>
  ```

- 定义索引数据访问接口

  ![1562812621187](assets/1562812621187.png)

#### 小结

> 注解：
>
> **@Document 文档**: 把实体对象映射成一篇文档数据
>
> **@Id** **主键**: 对应数据库表中的主键列
>
> **@Field** **字段**: 把数据库表中的列转化成文档中的Field

## 09_SpringDataES操作ElasticSearch

#### 目标

> 实现SpringDataElasticSearch操作ElasticSearch服务索引库的代码

#### 小结

> 创建索引与映射方法:

```java
// 创建索引
esTemplate.createIndex(Item.class);
// 创建映射
esTemplate.putMapping(Item.class);
```

> 添加与修改方法：

```java
// 添加或修改类型
itemDao.save(item);
// 批量添加或修改类型
itemDao.saveAll(itemList);
```

> 删除方法：

```java
// 主键id删除
itemDao.deleteById(1L);
// 条件删除
itemDao.delete(item);
// 删除全部
itemDao.deleteAll();
```

> 查询方法：

```java
// 主键id查询
Item item = itemDao.findById(1L).get();
// 查询全部
Iterable<Item> items = itemDao.findAll();
// 分页查询
Page<Item> page = itemDao.findAll(pageable);

// 条件分页搜索
AggregatedPage<Item> page = esTemplate.queryForPage(query, Item.class);
```

## 10_配置复制Field

#### 目标

> 完成复制Field的配置

#### 操作步骤

+ 修改实体类在@Field注解copyTo属性
+ 搜索条件就可以用keywords

#### 小结

> 复制Field：它是把多个Field复制到一个Field，方便搜索。

## 11_配置嵌套Field

#### 目标

> 完成嵌套Field的配置
>
> 规格需要用到嵌套Field

#### 操作步骤

- 修改实体类定义规格属性

  ![1562817269662](assets/1562817269662.png)

- 测试类中使用

#### 小结

> 嵌套Field：它是把一个Field分成多个Field，规格Field(因为有多种规格)。

## 12_查询SKU商品数据

#### 目标

> 查询tb_item表中的数据

#### 操作步骤

- 创建pinyougou-es-import模块
- 查询SKU商品数据

#### 小结

> 从tb_item表中的查询全部商品数据

## 13_数据导入ElasticSearch索引库

#### 目标

> 把查询出来的SKU商品数据导入到ElasticSearch索引库

#### 操作步骤

- 整合spring-data-elasticsearch
  + 配置依赖jar
  + 在applicationContext.xml文件中配置spring-data-elasticsearch
  + 提供实体类
  + 提供索引库数据访问接口
- 编程部分: 索引库数据访问接口中的saveAll()

#### 小结

> 我们需要把tb_item表中的商品数据查询出来导入到ElasticSearch服务器的索引库中。

## 14_搭建搜索工程

#### 目标

> 搭建搜索工程: pinyougou-search-service、pinyougou-search-web

#### 搭建步骤

> 参考课程讲义

## 15_关键字搜索需求分析

#### 目标

> 如何完成关键了搜索功能?
>
> 打开搜索页面，在搜索框输入要搜索的关键字，点击搜索按钮即可进行搜索，显示搜索结果 

#### 需求分析

+ 前端(发送异步请求，迭代数据)
+ 后端(elasticSearchTemplate中的queryForPage()实现搜索)

#### 小结

> + 前端传入搜索关键字,显示搜索结果
> + 后端接收关键,然后调用esTemplate.queryForPage()完成搜索,返回搜索结果。

## 16_关键字搜索(前端代码)

#### 目标

> 实现关键搜索前端代码

#### 操作步骤

![1562821112387](assets/1562821112387.png)

![1562821532768](assets/1562821532768.png)

#### 小结

> 用户输入关键字，点击搜索，页面显示数据

## 17_关键字搜索(后端代码)

#### 目标

> 实现关键字搜索后端代码

#### 操作步骤

+ 控制器
+ 服务接口
+ 服务接口实现类

#### 小结

> 接收请求参数，根据关键字到ElasticSearch服务器的索引库中查询数据，将数据响应给浏览器

## 18_Nginx+Tomcat动静分离

#### 目标

> 解决商品图片访问问题?

#### 配置步骤

+ nginx  web服务器 (静态资源) css、js、images、html

  ![1562823221000](assets/1562823221000.png)

+ tomcat web服务器 (动态资源) jsp、数据来自后台数据库

#### 小结

> nginx处理用户请求的静态资源，tomcat处理用户请求动态资源，来实现动静分离，nginx处理静态资源效率远高于tomcat，这样一来就能更好的提高并发，处理性能。 
>
> nginx服务器处理项目中的静态资源：js、images、css、html