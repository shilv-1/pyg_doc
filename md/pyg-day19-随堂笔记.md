# 秒杀方案

## 03_秒杀业务分析

#### 目标

> 了解秒杀相关业务
>
> 秒杀商品数据来自哪里? 数据库
>
> 不是索引库

+ 秒杀时大量用户会在同一时间同时进行抢购，网站瞬时访问流量激增。

+ 杀一般是访问请求数量远远大于库存数量，只有少部分用户能够秒杀成功。

+ 秒杀业务流程比较简单，一般就是下订单减库存。

  

  秒杀技术实现核心思想是运用缓存减少数据库瞬间的访问压力！读取商品详细信息时运用缓存，当用户点击抢购时减少缓存中的库存数量，当库存数为0时或活动期结束时，同步到数据库。 产生的秒杀预订单也不会立刻写到数据库中，而是先写到缓存，当用户付款成功后再写入数据库。

#### 小结

> **秒杀商品通常有两种限制：库存限制、时间限制。**

## 04_搭建秒杀工程

#### 目标

> 完成秒杀工程搭建

#### 小结

> pinyougou-seckill-service服务层、pinyougou-seckill-web表现层

## 05_秒杀频道首页(需求分析)

#### 目标

> 了解秒杀频道首页需求

#### 需求分析

+ 秒杀频道首页显示正在秒杀的商品

+ 需要从tb_seckill_goods表查询数据

  SELECT * FROM `tb_seckill_goods` WHERE STATUS = 1 AND stock_count > 0 AND start_time <= NOW() AND end_time >= NOW()

## 06_秒杀频道首页(前端代码)

#### 目标

> 实现秒杀频道首页前端代码

#### 实现步骤

+ 单点登录(获取登录用名、登录、退出)
+ 定义异步请求的方法，查询正在秒杀的商品
+ 迭代显示秒杀商品

## 07_秒杀频道首页(后端代码)

#### 目标

> 实现秒杀频道首页后端代码

#### 实现步骤

+ 控制器

+ 服务接口

+ 服务接口实现类

  ![1563933727790](assets/1563933727790.png)

+ 数据访问接口

## 08_秒杀频道首页(缓存处理)

#### 目标

> 完成秒杀频道首页,秒杀商品缓存处理。

#### 实现步骤

+ 把秒杀商品存储到Redis数据库
+ 从Redis数据库获取秒杀商品

## 09_秒杀详细页

#### 目标

> 实现秒杀详细页,秒杀商品信息显示。

#### 实现步骤

+ 从秒杀频道首页跳转到秒杀商品的详情页(请求参数：秒杀商品id)
+ 秒杀商品的详情页获取秒杀商品id
+ 发送异步请求，获取秒杀商品对象
+ 数据显示
+ 后端秒杀商品需要从Redis数据库中获取

## 10_秒杀倒计时

#### 目标

> 实现秒杀倒计时功能

![1563936927145](assets/1563936927145.png)

#### 实现步骤

+ 根据秒杀商品的结束时间来显示倒计时

#### 代码

```javascript
// 显示倒计时
downcount : function (endTime) {
    // 计算出相差毫秒数
    var milliseconds = endTime - new Date().getTime();
    // 计算出相差秒数
    var seconds = Math.floor(milliseconds / 1000);

    if (seconds >= 0) {

        // 计算出相差分钟
        var minutes = Math.floor(seconds / 60);
        // 计算出相差小时
        var hours = Math.floor(minutes / 60);

        // 定义数组封装时间
        var arr = [];
        // 小时
        arr.push(this.calc(hours) + ":");
        // 分钟
        arr.push(this.calc(minutes - hours * 60) + ":");
        // 秒
        arr.push(this.calc(seconds - minutes * 60));

        this.timeStr = arr.join("");

        // 开启定时器
        setTimeout(function () {
            vue.downcount(endTime);
        }, 1000);
    }else{
        this.timeStr = "秒杀已结束！";
    }
},
// 不够两位前面补零
calc : function (num) {
    return num > 9 ? num : "0" + num;
}
```

## 11_秒杀下单(前端代码)

#### 目标

> 实现秒杀下单前端代码

#### 实现步骤

+ 为立即抢购按钮绑定点击事件
+ 定义事件方法
+ 判断用户是否登录
+ 如果已登录，发送异步请求，实现秒杀下单
+ 如果下单成功，就跳转到支付页面

## 12_秒杀下单(后端代码)

#### 目标

> 实现秒杀下单后端代码

```java
@Test
public void setNx(){
    boolean s = redisTemplate.opsForValue().setIfAbsent("name", "admin2");
    System.out.println("s = " + s);
    redisTemplate.delete("name");
}
```

#### 实现步骤

+ 控制器
+ 服务接口
+ 服务接口实现类

## 13_秒杀支付(生成支付二维码)

#### 目标

> 实现秒杀支付二维码生成

#### 小结

> 参考: pinyougou-cart-web系统中微信支付二维码生成

## 14_秒杀支付(支付成功保存订单)

#### 目标

> 实现秒杀支付成功保存订单功能

#### 实现步骤

+ 控制器
+ 服务接口
+ 服务接口实现类
