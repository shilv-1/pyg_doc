# Shiro&商家管理

## 03_Shiro回顾

#### 目标

> 了解Shiro开发的整体思路

#### 操作步骤

+ 第一步: 配置shiro的依赖jar(shiro-spring)
+ 第二步: 在web.xml文件配置委派过滤器代理与ContextLoaderListener监听器
+ 第三步: 提供Spring配置文件配置shiro

#### 小结

> + 配置shiro过滤器工厂
> + 配置安全管理器
> + 配置认证域

## 04_运营商后台(登录功能)

#### 目标

> 实现运营商后系统登录功能

#### 实现步骤

+ 整合shiro

  + 配置依赖jar(shiro-spring-1.4.0.jar)

    ```xml
    <!-- shiro-spring -->
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-spring</artifactId>
    </dependency>
    ```

  + 修改web.xml

    ```xml
    <!-- 配置Spring委派过滤器代理 -->
    <filter>
       <filter-name>shiroFilter</filter-name>
       <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
       <filter-name>shiroFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!-- 配置Spring MVC前端控制器(核心控制器) -->
    <servlet>
       <servlet-name>pinyougou</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:springmvc.xml,
             classpath:applictionContext-shiro.xml</param-value>
       </init-param>
       <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
       <servlet-name>pinyougou</servlet-name>
       <url-pattern>/</url-pattern>
    </servlet-mapping>
    ```

  + 提供spring的配置文件整合shiro

  + 认证域Realm(shiro.ini)

    ![1562030304608](assets/1562030304608.png)

+ 修改login.html

+ 提供登录控制器，登录方法

#### 小结

> 整合Shiro实现用户登录。

## 05_运营商后台(登录用户名&退出)

#### 目标

> 用户登录成功,显示登录用户名。实现用户退出登录。

#### 实现步骤

+ 前端页面，发送异步请求，获取登录用户名，把用户名存储到data数据模型中，页面显示

+ 后端定义处理请求的方法，从主体中获取登录用户名，返回数据

  ![1562032364836](assets/1562032364836.png)

#### 小结

> 获取登录用户名: SecurityUtils.getSubject().getPrincipal()
>
> 用户退出请求地址: /logout。

## 06_搭建商家管理后台

#### 目标

> 搭建商家管理后台系统,商家登录该后台可以管理商家的商品。

## 07_商家申请入驻

#### 目标

> 实现商家申请入驻品优购电商平台。
>
> tb_seller表

#### 实现步骤

+ 前端代码

  ![1562034122537](assets/1562034122537.png)

+ 后端代码

  + 控制器
  + 服务接口
  + 服务接口实现类
  + 数据访问接口

#### 小结

> 往tb_seller商家表插入入驻商家数据。

## 08_商家待审核列表

#### 目标

> 在运营商管理后台多条件分页查询待审核商家数据。

#### 实现步骤

前端：

![1562036863635](assets/1562036863635.png)

后端：

SELECT * FROM `tb_seller` WHERE STATUS = 0 + 页面上的查询条件

#### 小结

> 商家审核状态为待审核。

## 09_商家详情

#### 目标

> 显示某个待审核商家的详细信息。

#### 实现步骤

![1562039283472](assets/1562039283472.png)

#### 小结

> 点击查询商家详情,获取当前行的数据放入数据模型data中,在页面取该对象数据进行显示。

## 10_商家审核

#### 目标

> 实现商家审核: 审核通过、审核不通过、关闭商家。

#### 实现步骤

前端：

![1562039580175](assets/1562039580175.png)

后端：

UPDATE tb_seller SET STATUS = ? WHERE sellerId = ?

#### 小结

> 修改商家表审核状态: 审核通过状态码为1、审核不通过状态码为2、关闭商家状态码为3。

## 11_商家后台登录(登录功能)

#### 目标

> 实现商家后台系统登录功能。

#### 实现步骤

+ 整合Shiro(依赖jar、修改web.xml、spring配置文件，自定义认证域)
+ 修改shoplogin.html
+ 登录控制器

#### 小结

> 整合Shiro实现商家登录。

## 12_商家后台登录(密码加密)

#### 目标

> 实现商家申请入驻密码加密，配置Shiro密码凭证匹配器。

#### 实现步骤

+ 商家的密码需要 加密、加盐、加迭代次数
+ 配置shiro的密码凭证匹配器

#### 小结

> 使用Shiro提供的加密方式。

## 13_商家后台登录(登录名&退出)

#### 目标

> 实现商家后台系统,获取登录用户名及退出功能

#### 小结

> 参考运营商后台获取登录用户名与退出
