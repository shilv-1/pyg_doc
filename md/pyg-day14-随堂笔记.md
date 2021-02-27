# HttpClient&阿里大于短信&用户中心

## 03_HttpClient介绍

#### 目标

> 了解HttpClient框架

#### 小结

> HttpClient是封装了HTTP协议的客户端编程工具包，用java编程方式来完成http请求的发送。

## 04_HttpClient发送Get请求

#### 目标

> 利用HttpClient发送Get请求(不带请求参数、带请求参数)

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.3</version>
</dependency>
```

+ 第一步: 创建HttpClient客户端对象
+ 第二步: 创建请求方式对象HttpGet
+ 第三步：执行HTTP请求，得到响应对象
+ 第四步：判断响应状态码，获取响应数据
+ 第五步：关闭响应对象

#### 小结

> 核心api:
>
> + HttpClients: http客户端工具类
>
> + CloseableHttpClient: 可关闭的http客户端
> + HttpGet|HttpPost|HttpPut|HttpDelete: 请求方式
> + CloseableHttpResponse: 可关闭的http响应对象

## 05_HttpClient发送Post请求

#### 目标

> 利用HttpClient发送Post请求(不带请求参数、带请求参数)

#### 实现步骤

+ 第一步: 创建HttpClient客户端对象
+ 第二步: 创建请求方式对象HttpPost
+ 第三步：执行HTTP请求，得到响应对象
+ 第四步：判断响应状态码，获取响应数据
+ 第五步：关闭响应对象

#### 小结

> 请求参数封装：
>
> List<NameValuePair>: 封装请求参数集合
>
> UrlEncodedFormEntity: 表单实体

```java
// 定义List集合封装请求参数
List<NameValuePair> nvpList = new ArrayList<>();
// 添加请求参数
nvpList.add(new BasicNameValuePair("username","admin"));
// 添加请求参数
nvpList.add(new BasicNameValuePair("password","123456"));
// 创建form表单实体
UrlEncodedFormEntity formEntity = new UrlEncodedFormEntity(nvpList,"UTF-8");
// 设置请求参数实体
httpPost.setEntity(formEntity);
```

## 06_HttpClientUtils工具类

#### 目标

> 利用HttpClientUtils工具类发送Get或Post请求，简化HttpClient发送请求代码

![1563327274799](assets/1563327274799.png)

#### 小结

> 抽取了常用的四个发送请求方法：
>
> + HttpClientUtils.sendGet(url, map): 发送get请求
> + HttpClientUtils.sendPost(url, map): 发送post请求
> + HttpClientUtils.sendPut(url, map): 发送put请求
> + HttpClientUtils.sendDelete(url, map): 发送delete请求

## 07_阿里大于短信平台

#### 目标

> 了解阿里大于短信平台,下载SDK与Demo示例。

#### 小结

> 短信签名、短信模板、申请accesskey(访问key与访问密钥)

## 08_搭建短信工程

#### 目标

> 搭建短信工程: pinyougou-sms-service、pinyougou-sms-web

#### 搭建步骤

> 参考课程讲义

#### 小结

> 服务层搭建、表现层搭建跟以前一样,只是表现层没有视图页面,提供接口方式让其它系统调用。

## 09_短信发送(服务层)

#### 目标

> 在服务层实现短信发送功能

#### 小结

> 使用阿里大于提供SDK调用短信接口,发送短信。

## 10_短信发送接口(表现层)

#### 目标

> 在表现层实现短信发送接口代码

#### 实现步骤

+ 第一步: 定义SmsController.java控制器
+ 第二步: 定义发送短信方法(调用短信服务接口)

#### 小结

> 所谓的接口,指得是第三方应用能够接入的接口。我们提供的http请求,响应数据只要是json或xml格式,就是http请求的接口,跟开发语言、平台无关。
>
> java工程师为什么要写接口?
>
> 电商项目(java)
>
> android ios 手机客户端 90%

## 11_短信发送接口文档

#### 目标

> 如果编写短信发接口文档? 让调用者一看接口文档就知道如何调用。

#### 编写步骤

+ 第一步: 请求URL 

+ 第二步: 请求方式

+ 第三步: 请求参数

+ 第四步: 响应数据

+ 第五步: 调用示例

  ![1563334009641](assets/1563334009641.png)

#### 小结

> 调用URL、请求方式、请求参数、响应数据、调用示例。

## 12_短信接口调用测试

#### 目标

> 自已根据接口文档,写调用接口示例。

#### 小结

> 用HttpClient客户端调用接口,获取响应数据。

## 13_搭建用户中心工程

#### 目标

> 搭建用户中心工程: pinyougou-user-service、pinyougou-user-web

#### 小结

> 跟其它服务层、表现层搭建方式一样。

## 14_用户注册功能

#### 目标

> 实现用户注册功能(先不管短信验证码)

#### 实现步骤

+ 前端代码
  + 引入js文件
  + 数据绑定
  + 事件绑定
  + 异步请求方法
+ 后端代码
+ + 控制器
  + 服务接口
  + 服务接口实现类
  + 数据访问接口

#### 小结

> 往tb_user表中插入一条记录

## 15_发送短信验证码

#### 目标

> 实现发送短信验证码到用户手机。

#### 实现步骤

+ 前端代码: 

  ![1563337729683](assets/1563337729683.png)

+ 后端代码: 

  + 随机生成6位数字验证码
  + 调用短信发送接口
  + 存储验证码到Redis

  ![1563338880905](assets/1563338880905.png)

#### 小结

> 随机生成6位数字验证码,用HttpClientUtils工具类调用短信发送接口,实现短信发送,再把验证码存储到Redis数据库,有效时间90秒。

## 16_检验短信验证码

#### 目标

> 实现短信验证码检验功能

#### 实现步骤

+ 接收用户输入的验证码与Redis中验证码进行比较

#### 小结

> 接收用户输入的验证码跟Redis存储的验证码进行比较。
