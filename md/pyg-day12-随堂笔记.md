# FreeMarker&商品详情系统

## 03_Freemarker介绍

> 参考课程讲义
>
> FreeMarker Template Language (FTL)FreeMarker模板语言。
>
> Web项目视图：
>
> + html (性能最好)静态视图页面
> + ftl (性能居中) 动态视图页面 (经过web服务器处理后才可以访问)
> + jsp (性能最差) 动态视图页面 (经过web服务器处理后才可以访问)

## 04_FreeMarker入门示例

#### 实现步骤

+ 配置依赖jar包

  ```xml
  <!-- freemarker -->
  <dependency>
      <groupId>org.freemarker</groupId>
      <artifactId>freemarker</artifactId>
      <version>2.3.28</version>
  </dependency>
  ```

+ 编写模板文件xxx.ftl

+ 编写代码

  + 创建Configuration配置信息对象

  + 获取指定的模板文件对应的模板对象

  + 定义数据模型

  + 模板填充数据模型输出文件

    ```java
    // 1. 创建配置信息对象
    Configuration configuration = new Configuration(Configuration.VERSION_2_3_28);
    // 1.1 设置模板文件加载的基础路径
    configuration.setClassForTemplateLoading(FreeMarkerTest.class, "/ftl");
    // 1.2 设置模板文件默认的编码
    configuration.setDefaultEncoding("UTF-8");
    
    // 2. 获取模板文件对应的模板对象
    Template template = configuration.getTemplate("hello.ftl");
    
    // 3. 定义数据模型
    Map<String, Object> dataModel = new HashMap<>();
    dataModel.put("name","张三");
    dataModel.put("message","欢迎来到神奇的品优购世界！");
    
    // 4. 模板填充数据模型输出文件
    template.process(dataModel, new FileWriter("C:\\Users\\Administrator\\Desktop\\html\\1.html"));
    ```

#### 小结

> 核心API:
>
> + Configuration : 配置信息
> + Template: 模板

## 05_FreeMarker基础语法

> 模板文件中四种元素：
>
> 1、文本：直接输出的部分   
>
> 2、注释：即<#--...-->格式不会输出 
>
> 3、取值：即${..}部分,将使用数据模型中的部分替代输出 
>
> 4、FTL指令：FreeMarker指令，和HTML标记类似，名字前加#予以区分，不会输出。

## 06_FreeMarker常用指令

+ assign变量指令

  <#assign 变量名=变量值/>

+ include包含指令

  <#include 'xx.ftl'/>

+ if条件指令

  <#if true|false>

     输出内容

  <#elseif true|false>

  ​	

  <#else>

     

  </#if>

+ list迭代指令

  <#list 数组变量名 as 元素变量名>

     ${元素变量名}

  </#list>

## 07_FreeMarker内建函数

+ 获取集合大小 ${变量名?size}

+ 转换json字符串为对象 ${变量名?eval}

+ 日期格式化

  +  ${变量名?date} yyyy-MM-dd
  +  ${变量名?time} HH:mm:ss
  +  ${变量名?datetime} yyyy-MM-dd HH:mm:ss
  +  ${变量名?string('yyyy年MM月dd日 HH:mm:ss')}

+ 数字转换为字符串

   ${变量名?c}

## 08_FreeMarker空值处理

+ 判断某变量是否存在:“??”  <#if 变量名??>
+ 缺失变量默认值:“!” ${变量名!''}

## 09_FreeMarker运算符

+ 算术运算符 + - * / %
+ 逻辑运算符 && || !
+ 比较运算符 > < ... == !=

## 10_搭建商品详情工程

#### 搭建步骤

+ 创建pinyougou-item-web工程
+ 拷贝静态资源
+ 配置域名访问

> 伪静态: 有利于SEO(Search Engine Optimization)搜索引擎优化，可以提升排名。
>
> 伪静态: 
>
>   <https://item.jd.com/100002749549.html>  --> web服务器 --> item.ftl
>
> 真静态:
>
>   <https://item.jd.com/100002749549.html>  --> web服务器 --> 100002749549.html

## 11_SpringMVC整合FreeMarker

> FreeMarker模板文件作为web项目视图展示，取代jsp视图。

#### 整合步骤

+ 配置依赖jar(freemarker.jar、spring-context-support.jar)

  ```xml
  <dependency>
      <groupId>org.freemarker</groupId>
      <artifactId>freemarker</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
  </dependency>
  ```

+ 修改springmvc.xml(配置freemarker视图解析器)

  ```xml
  <!-- ############## springmvc整合freemarker ############## -->
  <!-- 配置FreeMarker配置信息对象 -->
  <bean id="freeMarkerConfigurer" class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
      <!-- 设置模板文件的默认编码 -->
      <property name="defaultEncoding" value="UTF-8"/>
      <!-- 设置模板文件加载的基础路径 -->
      <property name="templateLoaderPath" value="/WEB-INF/ftl/"/>
  </bean>
  <!-- 配置FreeMarker视图解析器 -->
  <bean class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
      <!-- 设置输出的内容类型 -->
      <property name="contentType" value="text/html;charset=UTF-8"/>
      <!-- 设置模板页面的后缀名 -->
      <property name="suffix" value=".ftl"/>
  </bean>
  ```

## 12_商品基本数据显示(后端代码)

> 根据SPU的ID，查询SPU商品表、商品描述表、SKU商品表

#### 实现步骤

+ 控制器
+ 服务接口
+ 服务接口实现类
+ 数据访问接口

## 13_商品基本数据显示(前端代码)

#### 实现步骤

+ 模板模块化引入
+ 生成简单数据
+ 生成图片列表
+ 生成扩展属性列表
+ 生成规格列表

## 14_商品分类面包屑显示

#### 实现步骤

+ 后端代码
  + 查询商品的一级分类名称、二级分类名称、三级分类名称
  + 把数据存储到数据模型中
+ 前端代码
  + 用${}取数据

## 15_购买数量加减操作

![1563077059302](assets/1563077059302.png)

#### 实现步骤

+ 事件绑定

  ![1563077392223](assets/1563077392223.png)

  

+ 事件方法

  ![1563077679530](assets/1563077679530.png)

## 16_规格选项选择

![1563077782745](assets/1563077782745.png)

#### 实现步骤

+ 记录用户选择的规格选项

  {"网络":"联通4G","机身内存":"64G"}

+ 用户选择的规格选项添加选中样式

  ```javascript
  // 判断是否选中
  isSelected : function (specName, optionName) {
      return this.spec[specName] == optionName;
  }
  ```

## 17_读取SKU信息(后端代码)

> 根据SPU的id查询多条SKU商品信息

SELECT * FROM tb_item WHERE goods_id = 149187842867973 ORDER BY is_default DESC

#### 实现步骤

+ 第一步：修改服务接口实现类中的getGoods()方法，查询SKU商品数据

## 18_读取SKU信息(前端代码)

#### 实现步骤

+ 第一步：页面生成SKU列表变量
+ 第二步：加载默认SKU，显示SKU标题与价格
+ 第三步：用户选择规格时，查询对应的SKU
+ 第四步：加入购物车按钮事件绑定

## 19_系统模块对接

> 搜索系统跳转到商品详情系统