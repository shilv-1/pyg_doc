# CORS跨域请求&结算页&保存订单

## 03_什么是跨域请求?

#### 目标

> 了解什么是跨域请求?

![1563670252257](assets/1563670252257.png)

#### 小结

> JavaScript在不同域名之间请求数据，域名不同或者端口不同的请求都叫跨域请求。
>
> 注意：只要**协议、域名、端口**有任何一个不同，都被当作是不同的域。

## 04_JS跨域请求测试

#### 目标

> 完成JS跨域请求测试

#### 实现步骤

+ pinyougou-item-web商品详情系统发送异步请求

+ pinyougou-cart-web处理异步请求

  ```javascript
  /** 发送跨域的异步请求
   *  http://item.pinyougou.com 异步请求  http://cart.pinyougou.com
   * */
  axios.get("http://cart.pinyougou.com/cart/addCart?itemId="
      + this.sku.id + "&num=" + this.num).then(function(response){
          // 获取响应数据
        if (response.data){
            // 跳转到购物车系统
            location.href = "http://cart.pinyougou.com";
        }else{
            alert("加入购物车失败！");
        }
  });
  ```

#### 小结

> 跨域请求错误信息：
>
> ![1563671055214](assets/1563671055214.png)

## 05_跨域请求解决方案

#### 目标

> 记住JS跨域请求解决方案

#### 小结

> 两种跨域请求方案：
>
> + JSONP(JSON with Padding)是JSON的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。
> + CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin Resource Sharing）。CORS需要浏览器和服务器同时支持。

## 06_CORS跨域解决方案

#### 目标

> CORS解决JS跨域请求

#### 实现步骤

+ 编码方式

  ```java
  // 设置允许跨域访问的域名(98%)
  response.setHeader("Access-Control-Allow-Origin", "http://item.pinyougou.com");
  // 设置允许跨域访问Cookie(2%)
  response.setHeader("Access-Control-Allow-Credentials", "true");
  ```

  ![1563672531010](assets/1563672531010.png)

+ 注解方式

  ```java
  @CrossOrigin(origins = {"http://item.pinyougou.com"},
          allowCredentials = "true")
  ```

#### 小结

> SpringMVC提供的跨域注解是：@CrossOrigin

## 07_收件人地址列表

#### 目标

> 完成收件人地址列表显示

![1563673984796](assets/1563673984796.png)

+ 前端代码

  + 定义异步请求方法，查询当前登录用户的收件地址
  + 页面显示收件地址

+ 后端代码

  List<Address>

   SELECT * FROM `tb_address` WHERE user_id = 'itcast' ORDER BY is_default DESC

#### 小结

> 根据当前登录用户查询tb_address表的收件地址，在页面显示。

## 08_地址选择&样式控制

#### 目标

> 实现收件地址选择及样式控制

![1563675308375](assets/1563675308375.png)

#### 实现步骤

+ 绑定事件

+ 提供事件方法，记录用户选择的地址

+ 在data数据模型中定义变量

  ```
  // 选择收件地址
  selectAddress : function (item) {
      this.address = item;
  },
  // 判断迭代中的地址是否为选中的地址
  isSelectedAddress : function (item) {
      return this.address == item;
  }
  ```

#### 小结

> 存储地址的变量为json对象

## 09_默认收件地址显示

#### 目标

> 完成默认收件地址显示

#### 实现步骤

![1563675821350](assets/1563675821350.png)

## 10_支付方式选择

#### 目标

> 完成支付方式选择: 微信支付、线下支付

#### 实现步骤

+ 数据封装 order : {paymentType  : 1}

  ![1563676189122](assets/1563676189122.png)

## 11_商品清单显示

#### 目标

> 完成购物车商品清单显示

#### 实现步骤

+ 从购物车中获取商品显示

## 12_搭建订单服务工程

#### 目标

> 完成订单服务工程搭建: pinyougou-order-service

#### 小结

> 参考课程讲义

## 13_订单数据库表分析

#### 目标

> 完成订单数据库表结构分析
>
> 保存订单：
>
> tb_order: 订单主表(总计)
>
> tb_order_item: 订单明细表(明细)

+ tb_order 订单表(数据量很多)

   order_id: 主键列(没用数据库表的自增长)

+ tb_order_item 订单明细表(数据量很多)

  id: 主键列(没用数据库表的自增长)

  多台mysql数据库存储一张表的数据(mysql分布式存储)

  tb_order表 3亿条记录(分库分表)

  + 第一台mysql(tb_order表):  1亿条记录
  + 第二台mysql(tb_order表):  1亿条记录
  + 第三台mysql(tb_order表):  1亿条记录

## 14_分布式ID生成器

#### 目标

> 使用分布式ID生成器
>
> java生成主键id
>
> UUID: 字符串（索引性能低）、无序 (对于分布式项目，主键id也可能重复)
>
> 当前时间毫秒数: 容易重复
>
> 分布式id生成器: Long类型、有序 26w/1s

#### 使用步骤

+ 拷贝IdWorker.java

+ 配置IdWorker

  ```xml
  <!-- 分布式id生成器 -->
  <bean id="idWorker" class="com.pinyougou.common.util.IdWorker">
      <!-- 数据中心id(数据库相关) -->
      <constructor-arg name="datacenterId" value="0"/>
      <!-- 工作机器id(部署的服务器相关) -->
      <constructor-arg name="workerId" value="0"/>
  </bean>
  ```

## 15_保存订单(前端代码)

#### 目标

> 实现保存订单前端代码

#### 实现步骤

+ 为提交订单按钮绑定点击事件

+ 定义事件方法，发送异步请求(请求参数封装)

  ![1563682316237](assets/1563682316237.png)

## 16_保存订单(后端代码)

#### 目标

> 实现保存订单后端代码

#### 实现步骤

+ 控制器
+ 服务接口
+ 服务接口实现类
+ 数据访问接口

