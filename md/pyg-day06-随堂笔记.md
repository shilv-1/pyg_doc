# 商品新增&选择分类&选择品牌

## 03_电商概念SPU&SKU

#### 目标

> 了解电商概念SPU与SKU

+ SPU: Standard Product Unit(标准产品单位)
+ SKU: Stock Keeping Unit(库存量单位)

#### 小结

> SKU = SPU + (规格、颜色、尺寸、等等)组合

## 04_商品表结构分析

#### 目标

> 了解商品表结构

#### 商品表

+ 商品SPU表(tb_goods)
+ 商品SKU表(tb_item)
+ 商品描述表(tb_goods_desc)

#### 小结

> + tb_goods(主表) 与 tb_goods_desc(从表) 一对一关系。
> + tb_goods(主表) 与 tb_item(从表) 一对多关系。

## 05_商品录入(需求分析)

#### 目标

> 如何完成商品录入功能?

#### 需求分析

+ 往三张表插入数据

  + tb_goods SPU表 (一行)  Goods.java 
  + tb_goods_desc 商品描述表 (一行)  GoodsDesc.java
  + tb_item SKU表 (多行)  Item.java

+ 前端数据封装

  axios.post(url, {});

  {

     goodsName : '',

     price : '',

     ...,

     goodsDesc : {packageList:'',...},

     items : [{title:'',...},{},{}]

  }

+ 后端请求参数接收

  + Goods.java (主表对应的实体类封装数据)

    private GoodsDesc goodsDesc;

    private List<Item> items;

    ![1562202674595](assets/1562202674595.png)

  + GoodsDesc.java

  + Item.java

#### 小结

> 前端数据封装格式: 
>
> {
>
>    goodsName : '',
>
>    price : '',
>
>    ...,
>
>    goodsDesc : {packageList:'',...},
>
>    items : [{title:'',...},{},{}]
>
> }
>
> 后端请求参数接收: 
>
> Goods.java

## 06_商品录入(基本信息)

#### 目标

> 完成商品基本信息录入
>
> 商品名称、副标题、价格: tb_goods
>
> 包装列表、售后服务: tb_goods_desc

#### 实现步骤

+ 前端代码
  + 引入js文件
  + 数据绑定
  + 保存按钮绑定点击事件
  + 发送异步请求
+ 后端代码
  + 往tb_goods、tb_goods_desc表插入数据

#### 小结

> 页面上封装的请求参数格式：
>
> {
>
>  goodsName : '', price : '', ...
>
>  goodsDesc : {}
>
> }

## 07_商品录入(商品介绍)

#### 目标

> 完成商品介绍信息录入

#### 实现步骤

+ 引入kindeditor

  ```javascript
   <!-- 富文本编辑器 -->
  <link rel="stylesheet" href="/plugins/kindeditor/themes/default/default.css"/>
  <script src="/plugins/kindeditor/kindeditor-min.js"></script>
  <script src="/plugins/kindeditor/lang/zh_CN.js"></script>
  <!-- 富文本编辑器 -->
  ```

  ![1562205580832](assets/1562205580832.png)

+ 商品介绍信息有文字、图片、字体有样式(使用富文本编辑器)

+ 富文本编辑器kindeditor使用

#### 小结

> 富文本编辑器kindeditor: 
>
> + editor.html()获取富文本编辑器中的内容
> + editor.html('')清空富文本编辑器中的内容

## 08_商品图片上传(前端代码)

#### 目标

> 实现商品图片上传前端代码

![1562206153517](assets/1562206153517.png)

#### 实现步骤

+ 为上传按钮绑定点击事件
+ 定义事件方法，发送异步请求(请求参数为图片的字节数据)
+ 获取响应数据，显示上传的图片

#### 代码

```javascript
 // 图片异步上传
uploadFile : function () {
    // 创建表单数据对象封装图片数据(html5)
    var formData = new FormData();
    // 追加文件
    // 第一个参数：请求参数名
    // 第二个参数：文件对象
    formData.append("file", file.files[0]);
    // 发送异步请求
    axios({
        url : '/upload', // 请求URL
        method : 'post', // 请求方式
        data : formData  // 请求参数
        //headers : {"Content-Type" : "multipart/form-data"}   // 请求头
    }).then(function(response){
        // 获取响应数据 response.data: {status : 200, url : ''}
        if (response.data.status == 200){
            alert(response.data.url);
        }else{
            alert("上传图片失败！");
        }
    });
}
```

## 09_商品图片上传(后端代码)

#### 目标

> 实现商品图片上传后端代码

#### 实现步骤

+ SpringMVC文件上传

  + 配置commons-fileupload.jar、commons-io.jar依赖jar

    ```
    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
    </dependency>
    ```

  + 配置文件上传解析器

    ```xml
    <!-- 配置文件上传解析器 -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!-- 设置默认的编码 -->
        <property name="defaultEncoding" value="UTF-8"/>
        <!-- 设置上传文件的大小 2M 字节 1024 * 1024 * 2 -->
        <property name="maxUploadSize" value="2097152"/>
    </bean>
    ```

  + 定义控制器

+ FastDFS文件服务器

  + java客户端(配置依赖jar)
  + 提供fastdfs-client.conf文件
  + 编程部分

小结

> 

## 10_商品图片操作(前端代码)

#### 目标

> 实现商品图片前端操作功能
>
> 上传的商品图片保存到tb_goods_desc表的item_images:
>
> [{"color":"金色","url":"http://image.pinyougou.com/jd/wKgMg1qtKEOATL9nAAFti6upbx4132.jpg"},{"color":"深空灰色","url":"http://image.pinyougou.com/jd/wKgMg1qtKHmAFxj7AAFZsBqChgk725.jpg"},{"color":"银色","url":"http://image.pinyougou.com/jd/wKgMg1qtKJyAHQ9sAAFuOBobu-A759.jpg"}]
>
> goods.goodsDesc.itemImages = [{},{}];

#### 实现步骤

+ 显示上传商品图片
+ 商品图片列表
+ 移除商品图片

#### 小结

> 数据封装: 上传的商品图片数据最终需保存到tb_goods_desc表的item_images列。

## 11_查询商品一级分类

#### 目标

> 实现商品一级分类下拉列表
>
> SELECT * FROM `tb_item_cat` WHERE parent_id = ?

#### 实现步骤

+ 前端代码
  + 定义查询商品分类的方法，获取响应数据 [{},{}] List<ItemCat>
  + 页面迭代产生option (v-for)
+ 后端代码
  + 控制器

#### 小结

> 查询父级id为0的商品分类。

## 12_查询商品二、三级分类

#### 目标

> 实现商品二级分类下拉列表、实现商品三级分类下拉列表

#### 实现步骤

![1562213410810](assets/1562213410810.png)

#### 小结

> **watch **监控变量值发生改变。

## 13_查询类型模板ID

#### 目标

> 实现类型模板id查询

#### 实现步骤

![1562214196930](assets/1562214196930.png)

#### 小结

> 从商品三级分类数组中当前用户选中的商品分类对象,再获取类型模板id。

## 14_品牌下拉列表框

#### 目标

> 实现品牌下拉列表框数据显示
>
> 当选择了一级分类，查询二级分类，查询三分类，得到类型模板id,根据类型模板id查询类型模板表中的一行数据，就样就可以得到该分类关联品牌、规格、扩展属性
>
> ![1562214345484](assets/1562214345484.png)

#### 实现步骤

+ 监控类型模板id，发生改变，查询类型模板对象
+ 页面迭代产生品牌

#### 小结

> 品牌下拉列表框的数据来自类型模板表中的brand_ids列。

## 15_查询扩展属性

#### 目标

> 实现扩展属性数据的显示
>
> tb_type_template表
>
> [{"text":"分辨率"},{"text":"摄像头"},{"text":"核数"}]
>
> 
>
> 最终保存到tb_goods_desc: 表的custom_attribute_items列
>
>  [{"text":"分辨率","value":"1920*1080(FHD)"},{"text":"摄像头","value":"1200万像素"}]
>
> goods.goodsDesc.customAttributeItems = [{},{}];

![1562215641271](assets/1562215641271.png)

#### 实现步骤

![1562216184244](assets/1562216184244.png)

#### 小结

> 扩展属性数据来自类型模板表中的custom_attribute_items列。