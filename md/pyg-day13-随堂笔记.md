# RocketMQ&同步索引&商品详情静态化

## 03_消息中间件简介

> 参考: 课程讲义

#### 小总

> 常用的消息中间件：ActiveMQ、RabbitMQ、RocketMQ、Kafka。

## 04_RocketMQ简介

> RocketMQ作为一款纯java、分布式、队列模型的开源消息中间件，支持事务消息、顺序消息、批量消息、定时消息、消息回溯等。RocketMQ由阿里巴巴开源。
>
> ![1563239947722](assets/1563239947722.png)

## 05_Linux安装RocketMQ

> 参考: 【资料\RocketMQ\Linux安装RocketMQ.docx】
>
> ![1563241365671](assets/1563241365671.png)

## 06_windows启动控制台

> 参考: 【资料\RocketMQ\Linux安装RocketMQ.docx】
>
> springboot开发的web项目
>
> TPS: Transaction Per Second 每秒事务率
>
> QPS: Query Per Second 每秒查询率 

## 07_RocketMQ简单消息

> 完成RocketMQ简单消息发送与消费

```xml
<!-- rocketmq-client -->
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>4.5.0</version>
</dependency>
```

#### 实现步骤

1. 消息生产者
   + 创建默认的MQ消息生产者
   + 创建消息对象
   + 消息生产者发送消息
   + 关闭消息生产者
2. 消息消费者
   + 创建默认的消息消费者
   + 注册消息监听器，消费消息
   + 启动消息消费者

## 08_RocketMQ顺序消息

> 完成RocketMQ顺序消息发送与消费

#### 实现步骤

1. 消息生产者
   + 创建默认的MQ消息生产者

   + 创建消息对象

   + 消息生产者发送消息

     ```java
     // 3. 发送消息
     producer.send(message, new MessageQueueSelector() {
         @Override
         public MessageQueue select(List<MessageQueue> list,
                                    Message message, Object i) {
             return list.get((Integer)i);
         }
     }, 1); // 设置存入第几个队列中,下标从0开始
     ```

   + 关闭消息生产者
2. 消息消费者
   + 创建默认的消息消费者
   + 注册消息监听器，消费消息
   + 启动消息消费者

## 09_RocketMQ广播消息

> 完成RocketMQ广播消息发送与消费

#### 实现步骤

1. 消息生产者

   - 创建默认的MQ消息生产者
   - 创建消息对象
   - 消息生产者发送消息
   - 关闭消息生产者

2. 消息消费者

   + 创建默认的消息消费者

     ```java
     // 1.4 指定消费模式: 集群模式/广播模式
     // MessageModel.CLUSTERING: 集群模式
     // MessageModel.BROADCASTING: 广播模式
     consumer.setMessageModel(MessageModel.BROADCASTING);
     ```

   + 注册消息监听器，消费消息

   + 启动消息消费者

#### 小结

> RocketMQ消息模式:
>
> + 集群模式: 一个消息生产者对应一个消息消费者(一对一)
> + 广播模式: 一个消息生产者对应多个消息消费者(一对多)

## 10_RocketMQ批量消息

> 完成RocketMQ批量消息发送与消费

#### 实现步骤

1. 消息生产者

   - 创建默认的MQ消息生产者

   - 创建消息对象

   - 消息生产者发送消息

     ```java
     // 2. 创建消息对象
     List<Message> messages = new ArrayList<Message>();
     for (int i = 0; i < 20 ; i++) {
         Message message = new Message(
             "batch_demo1", // 主题名称
             "tag" + i, // 标签名称
             "key" + i, // key名称
             ("RocketMQ，我来了，批量消息").getBytes( // 消息内容字节数组
                 RemotingHelper.DEFAULT_CHARSET));
         //将消息添加到集合中
         messages.add(message);
     }
     // 3. 发送消息
     producer.send(messages);
     ```

   - 关闭消息生产者

2. 消息消费者

   - 创建默认的消息消费者
   - 注册消息监听器，消费消息
   - 启动消息消费者

## 11_Spring整合RocketMQ

> 完成Spring整合RocketMQ

#### 整合步骤

+ 第一步：配置依赖jar包(rocketmq-client、spring-context)
+ 第二步：提供整合配置文件applicationContext-rocketmq.xml
+ 第三步：配置消息生产者
+ 第四步：利用MQProducer发送消息到MQ服务器
+ 第五步：配置消息消费者
+ 第六步：自定义消息监听器类接收消息

#### 消息生产者

```xml
    <!-- 配置消息生产者 -->
    <bean id="mqProducer" class="org.apache.rocketmq.client.producer.DefaultMQProducer"
          init-method="start"
          destroy-method="shutdown">
        <!-- 设置组名 -->
        <property name="producerGroup" value="producerGroup5"/>
        <!-- 设置名称服务器的连接地址，如果是集群环境，多个之间用分号分隔 -->
        <property name="namesrvAddr" value="192.168.12.131:9876"/>
    </bean>
```

#### 消息消费者

```xml
<!-- 配置消息消费者 -->
    <bean class="org.apache.rocketmq.client.consumer.DefaultMQPushConsumer"
          init-method="start"
          destroy-method="shutdown">
        <!-- 设置组名 -->
        <property name="consumerGroup" value="consumerGroup5"/>
        <!-- 设置名称服务器的连接地址，如果是集群环境，多个之间用分号分隔 -->
        <property name="namesrvAddr" value="192.168.12.131:9876"/>
        <!-- 设置消息模式 -->
        <property name="messageModel" value="CLUSTERING"/>
        <!-- 设置订阅的主题与标签 -->
        <property name="subscription">
            <map>
                <!-- key : topic主题  value: tags标签 -->
                <entry key="demo5" value="*"/>
            </map>
        </property>
        <!-- 设置消费消息最大数量 -->
        <property name="consumeMessageBatchMaxSize" value="1"/>
        <!-- 设置消息监听器 -->
        <property name="messageListener" ref="messageListener"/>
    </bean>

    <!--配置自定义的消息监听器 -->
    <bean id="messageListener" class="cn.itcast.rocketmq.spring.listener.MessageListener"/>
```

## 12_商品索引生成(消息生产者)

![1563249748475](assets/1563249748475.png)

> 当商品上架成功时，发送消息到消息服务器。(集群模式)
>
> pinyougou-shop-web 商家管理后台

#### 实现步骤

+ 集成RocketMQ(配置依赖jar包、spring配置文件)

  ```xml
  <dependency>
      <groupId>org.apache.rocketmq</groupId>
      <artifactId>rocketmq-client</artifactId>
  </dependency>
  ```

+ 修改商品上架控制器中的方法(发送消息)

  ```java
  // 发送消息到消息中间件，同步商品的索引到索引库
  mqProducer.send(new Message("ES_ITEM_TOPIC", "UPDATE",
          JSON.toJSONString(ids).getBytes("UTF-8")));
  ```

## 13_商品索引生成(消息消费者)

> 自定义消息监听器接收消息服务器上的消息，生成该商品的索引数据。(集群模式)
>
> pinyougou-search-web 搜索系统

#### 实现步骤

+ 集成RocketMQ(配置依赖jar包、spring配置文件)

  ```
  <dependency>
      <groupId>org.apache.rocketmq</groupId>
      <artifactId>rocketmq-client</artifactId>
  </dependency>
  ```

+ 提供消息监听器(接收消息，查询上架的SKU商品，生成索引数据)

## 14_商品索引删除(集群)

> 当商品下架成功时，发送消息到消息服务器。消息消费者接收服务器上的消息，删除该商品的索引数据。
>
> pinyougou-shop-web: 商家管理后台
>
> pinyougou-search-web： 搜索系统

#### 实现步骤

1. 消息生产者(商家管理后台)

   + 商品下架生产消息

   ```java
    // 发送消息到消息中间件，删除商品的索引数据
                   mqProducer.send(new Message("ES_ITEM_TOPIC", "DELETE",
                           JSON.toJSONString(ids).getBytes("UTF-8")));
   ```

2. 消息消费者(搜索系统)

   + 自定义消息监听器

   ```java
    /** 删除商品的索引 */
       public void delete(List<Long> goodsIds){
           try{
               // 创建删除查询对象
               DeleteQuery deleteQuery = new DeleteQuery();
               // 设置索引库
               deleteQuery.setIndex("pinyougou");
               // 设置类型
               deleteQuery.setType("item");
               // 设置删除条件
               deleteQuery.setQuery(QueryBuilders.termsQuery("goodsId", goodsIds));
               // 条件删除
               esTemplate.delete(deleteQuery);
           }catch (Exception ex){
               throw new RuntimeException(ex);
           }
       }
   ```

## 15_静态化介绍

> 静态化: 把动态页面转化成静态页面，提升网站访问效率。
>
> 参考: 课程讲义

## 16_商品详细页生成(广播)

> 当商品上架成功时，发送消息到消息服务器，消息消费者接收服务器上的消息，生成该商品的静态详情页面。
>
> pinyougou-shop-web: 商家管理后台
>
> pinyougou-item-web： 商品详情系统

#### 实现步骤

1. 消息生产者(商家管理后台)

   - 商品上架生产消息

   ```java
   // 发送消息到消息中间件，生成商品的静态页面
   mqProducer.send(new Message("PAGE_ITEM_TOPIC", "CREATE",
                               JSON.toJSONString(ids).getBytes("UTF-8")));
   ```

2. 消息消费者(商品详情系统)

   + 整合RocketMQ

   + 自定义消息监听器

   ```java
     // 获取item.ftl的模板对象
   Template template = freeMarkerConfigurer.getConfiguration().getTemplate("item.ftl");
   // 循环生成多个静态页面
   for (Long goodsId : goodsIds) {
       // 获取数据模型
       Map<String,Object> dataModel = goodsService.getGoods(goodsId);
   
       // 输出流
       OutputStreamWriter writer = new OutputStreamWriter(new
                                                          FileOutputStream(pageDir + goodsId + ".html"), "UTF-8");
       // 填充模板，输出文件
       template.process(dataModel, writer);
       // 关闭
       writer.close();
   }
   ```

## 17_Nginx静态资源服务器

#### 核心配置

```xml
# 配置内部的web服务器	
server {
        listen       80;
        server_name  item.pinyougou.com;

	   proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;

        location / {
	      root F:/static06/item;
	      proxy_connect_timeout 600;
	      proxy_read_timeout 600;
	      # 如果当前请求的文件路径不存在，
	      # 则跳转到tomcat服务器9105 exits
	      if (!-e $request_filename){    
		    proxy_pass  http://127.0.0.1:9105;  
	      }
        } 
    }

```

## 18_商品详细页删除(广播)

> 当商品下架成功时，发送消息到消息服务器，消息消费者接收服务器上的消息，删除该商品的静态详情页面。
>
> pinyougou-shop-web: 商家管理后台
>
> pinyougou-item-web： 商品详情系统

#### 实现步骤

1. 消息生产者(商家管理后台)

   + 商品下架生产消息

   ```java
     // 发送消息到消息中间件，删除商品的静态页面
                   mqProducer.send(new Message("PAGE_ITEM_TOPIC", "DELETE",
                           JSON.toJSONString(ids).getBytes("UTF-8")));
   ```

2. 消息消费者(商品详情系统)

   + 自定义消息监听器

   ```java
    if ("DELETE".equals(tags)){// 删除商品的静态页面
                   for (Long goodsId : goodsIds) {
                       File file = new File(pageDir + goodsId + ".html");
                       if (file.exists() && file.isFile()){
                           file.delete();
                       }
                   }
               }
   ```

   