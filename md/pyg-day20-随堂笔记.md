# Nginx负载均衡&MyCat分库分表

## 03_什么是任务调度?

#### 目标

> 了解什么是任务调度?

#### 小结

> 常见的任务调度框架有Quartz和SpringTask

## 04_品优购整合SpringTask

#### 目标

> 完成品优购整合SpringTask任务调度

#### 整合步骤

+ 引入task命名空间(spring-context.jar)

+ 配置task注解驱动

+ 配置task任务调度方式

  ```xml
  <!-- 配置任务注解驱动 -->
  <task:annotation-driven/>
  <!-- 配置任务调度方式  scheduler 指定cron时间表达式 -->
  <task:scheduler id="scheduler"/>
  ```

+ 开发任务调度类

  ![1564016403754](assets/1564016403754.png)

#### 小结

> 一般采用cron时间表达式,来触发任务调度。

## 05_SpringTask的Cron时间表达式

#### 目标

> 了解SpringTask的Cron时间表达式。

#### 小结

> cron时间表达式分六个Field: 【秒】 【分】 【小时】 【日】 【月】 【周】

## 06_查询超时未支付订单

#### 目标

> 实现查询超时5分钟还未支付的订单。
>
> ![1564017606470](assets/1564017606470.png)

#### 代码

![1564018027117](assets/1564018027117.png)

#### 小结

> 从Redis数据库查询所有未支付订单,判断哪些订单超时5分钟未支付。

## 07_调用微信关闭订单接口

#### 目标

> 实现微信关闭订单接口的调用。

#### 核心代码

```java
public Map<String,String> closePayTimeout(String outTradeNo){
    try{
        try{
            // 1. 定义Map集合封装请求参数
            Map<String,String> params = new HashMap<>();
            // 公众账号ID  appid
            params.put("appid", appid);
            // 商户号 mch_id
            params.put("mch_id",partner);
            // 商户订单号    out_trade_no
            params.put("out_trade_no",outTradeNo);
            // 随机字符串    nonce_str
            params.put("nonce_str", WXPayUtil.generateNonceStr());

            // 签名并生成XML格式请求参数
            String xmlParam = WXPayUtil.generateSignedXml(params, partnerkey);
            System.out.println("请求参数：" + xmlParam);

            // 2. 调用“查询订单接口”
            HttpClientUtils httpClientUtils = new HttpClientUtils(true);
            String xmlData = httpClientUtils.sendPost(closeorder, xmlParam);
            System.out.println("响应数据：" + xmlData);
            
            // 3. 获取响应数据
            // 3.1 把xml格式转化成Map集合
            return WXPayUtil.xmlToMap(xmlData);

        }catch (Exception ex){
            throw new RuntimeException(ex);
        }
    }catch (Exception ex){
        throw new RuntimeException(ex);
    }
}
```

#### 小结

> 参考: 统一下单接口调用。

## 08_删除超时未支付订单

#### 目标

> 完成超时未支付订单的删除,增加库存。

#### 实现步骤

```java
/** 删除超时未支付的秒杀订单 */
public void deleteOrderFromRedis(SeckillOrder seckillOrder){
    try{
        // 1. 删除超时未支付的秒杀订单
        redisTemplate.boundHashOps("seckillOrderList")
                .delete(seckillOrder.getUserId());

        // 2. 恢复库存
        // 2.1 从Redis数据库获取秒杀商品
        SeckillGoods seckillGoods = (SeckillGoods) redisTemplate.boundHashOps("seckillGoodsList")
                .get(seckillOrder.getSeckillId());
        if (seckillGoods != null){
            // 库存数量加1
            seckillGoods.setStockCount(seckillGoods.getStockCount() + 1);
        }else {
            // 秒光了
            seckillGoods = seckillGoodsMapper.selectByPrimaryKey(seckillOrder.getSeckillId());
            seckillGoods.setStockCount(1);
        }
        // 重新把秒杀商品存到Redis数据库
        redisTemplate.boundHashOps("seckillGoodsList")
                .put(seckillGoods.getId(), seckillGoods);
    }catch (Exception ex){
        throw new RuntimeException(ex);
    }
}
```

## 09_品优购整体部署方案

#### 目标

> 了解品优购项目整体部署方案

#### 小结

> 共需要65台应用服务器

## 10_集群与分布式介绍

#### 目标

> 了解集群与分布式

#### 小结

> 集群: 集群的节点完成的是相同的功能。
>
> 分布式: 分布式的节点完成的是不同的功能。

## 11_Linux安装Nginx服务器

#### 目标

> 完成Linux系统安装Nginx服务器。

#### 小结

> nginx命令:
>
> 1. 启动: ./nginx
> 2. 停止: ./nginx -s stop
> 3. 重启: ./nginx -s reload
>

## 12_Nginx集群&负载均衡

#### 目标

> 完成Nginx集群与负载均衡配置

#### 小结

> 负载均衡算法:
>
> 1. 轮询(默认)
> 2. weight(权重)
> 3. ip_hash(解决Session访问问题)

## 13_Mycat分库分表中间件

#### 目标

> 完成Mycat安装与配置,实现商品表分库分表。

![1564028249859](assets/1564028249859.png)

#### 小结

> Mycat核心配置文件:
>
> 1. server.xml 配置服务参数(连接mycat中间件端口、用户名、密码、逻辑库)
> 2. schema.xml 配置逻辑库约束(逻辑库、数据节点、数据主机)
> 3. rule.xml 配置分片规则(表分片规则、算法函数)

## 14_品优购项目总结

> 参考: 品优购项目【总结】.txt

## 15_小组项目实战需求

#### 目标

> 了解小组项目实战需求
>
> 参考: 【小组项目实战需求.docx】

