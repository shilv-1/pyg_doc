# 单点登录&Pac4j集成CAS

## 03_什么是单点登录

#### 目标

> 什么是单点登录SSO?

#### 小结

> **单点登录**（Single Sign On）简称为SSO，SSO的定义是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。

## 04_什么是CAS

#### 目标

> 什么是CAS?

#### 小结

> **CAS(Central Authentication Service中央身份认证服务)**是 Yale 大学发起的一个开源项目。
>
> **TGT（Ticket Grangting Ticket）**: TGT是CAS为用户签发的登录票据，存入Session。
>
> **ST（Service Ticket）**: ST是CAS为用户签发的访问某一服务的票据，回传给CAS客户端(只使用一次，10秒过期)，作为该服务在CAS服务器的唯一标识符。
>
> **TGC（Ticket Grangting Cookie）**: TGC是CAS在浏览器存储用户登录凭证的Cookie。

## 05_CAS服务端部署

#### 目标

> 实现CAS服务端部署

#### 小结

> 将cas-server-webapp-4.2.7.war包部署到tomcat服务器。

## 06_CAS服务端配置

#### 目标

> 实现CAS服务端配置

#### 小结

> 核心配置文件: **cas.properties**
>
> + 修改tomcat端口号、配置用户名与密码、去除https、配置域名访问

## 07_开发单点登录客户端

#### 目标

> 开发单点登录客户端

#### 实现步骤

+ 配置cas客户端依赖jar

  ```xml
  <!-- cas的java语言客户端
      cas-client-core
   -->
  <dependency>
      <groupId>org.jasig.cas.client</groupId>
      <artifactId>cas-client-core</artifactId>
      <version>3.4.1</version>
  </dependency>
  ```

+ 配置四个过滤器(web.xml)

#### 小结

> **CAS客户端四个核心过滤器：**
>
> + **SingleSignOutFilter**：单点退出过滤器(用户退出)。
> + **AuthenticationFilter**：身份认证过滤器(登录才能访问的资源)。
> + **Cas20ProxyReceivingTicketValidationFilter**：票据验证过滤器(验证ST)。
> + **HttpServletRequestWrapperFilter**：请求包裹过滤器(获取登录用户名)。

## 08_CAS服务端数据源配置

#### 目标

> 配置CAS服务端登录用户名与密码来自数据库

#### 小结

> deployerConfigContext.xml
>
> + 配置数据源、配置密码加密方式、配置身份认证管理器
> + 拷贝依赖jar包

## 09_CAS服务端登录界面改造

#### 目标

> 改造CAS服务端登录界面

#### 小结

> casLoginView.jsp
>
> - 配置表单、用户名文本框、密码文本框、登录按钮、错误提示。

## 10_Pac4j安全引擎

#### 目标

> 了解Pac4j安全引擎。

#### 小结

> Pac4j是一个简单而强大的安全引擎，用于Java对用户进行身份验证、管理授权，以确保web应用程序安全。它提供了一套完整的概念和组件。它基于Java 8，并在Apache 2许可下使用。它可用于大多数框架/工具和支持大多数认证/授权机制。

## 11_Shiro工程搭建

#### 目标

> 搭建Shiro测试工程，用Pac4j整合CAS。

#### 小结

> 参考课程讲义。

## 12_Pac4j集成CAS

#### 目标

> 完成pac4j-cas集成CAS

#### 小结

> Pac4j-cas整合Shiro并集成CAS单点登录。

## 13_获取登录用户名&退出

#### 目标

> 获取登录用户名与用户单点退出

```java
Pac4jPrincipal principal = (Pac4jPrincipal) SecurityUtils
                .getSubject().getPrincipal();
String loginName = principal.getName();
```

#### 小结

> 获取登录用户名&退出。
>
> 注意：需要在web.xml文件中配置单点退出过滤器SingleSignOutFilter

## 14_用户中心集成CAS

#### 目标

> 完成用户中心用Pac4j集成CAS

#### 小结

> 具体参考课程讲义

## 15_获取登录用户名与退出

#### 目标

> 用户中心登录成功后,显示登录用户名以及用户退出

#### 实现步骤

+ 前端代码: 发异步请求、显示登录用户名、退出登录请求/logout。
+ 后端代码: 提供LoginController控制，定义获取登录用户名方法

#### 小结

> 获取登录用户名&退出。
