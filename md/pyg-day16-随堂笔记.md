# 购物车

## 03_购物车需求分析

#### 目标

> 购物车需求分析

#### 小结

> + 当用户在未登录的情况下，将购物车数据存入Cookie。(用户浏览器)
> + 当用户在登录的情况下，将购物车数据存入Redis。(适合长时间存储，读写速度快)
> + 如果用户登录时，需要将Cookie中的购物车数据合并到Redis中存储，并删除Cookie中的购物车数据。

## 04_搭建购物车工程

#### 目标

> 完成购物车工程搭建: pinyougou-cart-service、pinyougou-cart-web

#### 小结

> 参考课程讲义

## 05_购物车工程(基本集成CAS)

#### 目标

> 完成购物车工程基本集成CAS

#### 实现步骤

+ 配置依赖jar(cas-client-core.3.4.1.jar)

+ 在web.xml文件中配置四个过滤器

+ 获取登录用户名

  ![1563587349742](assets/1563587349742.png)

  ![1563587494943](assets/1563587494943.png)

+ 用户退出

  http://sso.pinyougou.com/logout?service=xxx

## 06_购物车工程(高级集成CAS)

#### 目标

> 完成购物车工程高级集成CAS

#### 实现步骤

- 解决CAS重定向回来端口号问题

  ![1563588288132](assets/1563588288132.png)

- 解决不需要登录的请求URL,如果其它系统已登录，显示登录成功

  ```xml
  <!-- 配置身份认证过滤器(不需要跳转到CAS登录系统，只是用来判断是否登录成功的) -->
  <filter>
      <filter-name>anonAuthenticationFilter</filter-name>
      <filter-class>org.jasig.cas.client.authentication.AuthenticationFilter</filter-class>
      <init-param>
          <!-- 配置CAS服务端登录地址 -->
          <param-name>casServerLoginUrl</param-name>
          <param-value>http://sso.pinyougou.com</param-value>
      </init-param>
      <init-param>
          <!-- 配置服务名称-->
          <param-name>serverName</param-name>
          <param-value>http://cart.pinyougou.com</param-value>
      </init-param>
      <!-- 配置gateway参数，作用验证用户是否登录成功，如果没登录也不需要重定到CAS服务端，
          如果登录了，就可以获取登录用户名 -->
      <init-param>
          <param-name>gateway</param-name>
          <param-value>true</param-value>
      </init-param>
  </filter>
  <filter-mapping>
      <filter-name>anonAuthenticationFilter</filter-name>
      <url-pattern>*.html</url-pattern>
  </filter-mapping>
  ```

- 解决URL编码问题

  ![1563589770109](assets/1563589770109.png)

## 07_Cookie存储购物车(控制器层)

#### 目标

> 实现Cookie存储购物车控制器层代码

#### 实现步骤

+ 添加商品到购物车
+ 查询用户的购物车

#### 小结

> Cookie的值存储List<Cart>集合对应的json字符串，如果存在中文需要encode编码。

## 08_Cookie存储购物车(服务层)

#### 目标

> 实现Cookie存储购物车服务层代码

#### 小结

> 具体代码实现参考课程讲义

## 09_购物车列表显示

#### 目标

> 完成前端购物车列表显示

#### 实现步骤

+ 定义异步请求方法，查询用户的购物车。
+ 页面迭代用户的购物车集合

## 10_购物车数量增减与删除

#### 目标

> 完成前端购物车数量增减与删除

#### 实现步骤

+ 为增减、删除绑定点击事件
+ 定义事件方法，发送异步请求
+ 修改后台购物车服务实现类代码
+ 重新查询购物车

## 11_购买总数与总金额显示

#### 目标

> 完成前端购买总数与总金额显示

#### 实现步骤

+ 统计购物车中商品的总购买数量
+ 统计购物车中商品的总购买金额
+ 页面显示总数量与总金额

## 12_Redis存取购物车

#### 目标

> 完成后端Redis存储与获取购物车代码

#### 实现步骤

+ 修改控制器中的加入购物车与查询购物车两个方法
+ 把用户的购物车存储到Redis数据库
+ 从Redis数据库获取用户的购物车

## 13_购物车合并(Redis)

#### 目标

> 完成登录用户Cookie购物车合并到Redis购物车

#### 实现步骤

+ 修改控制器中查询购物车方法
+ 实现购物车合并的业务代码

## 14_门户系统集成CAS

#### 目标

> 完成门户系统集合CAS,实现单点登录

#### 小结

> 参考课程讲义
