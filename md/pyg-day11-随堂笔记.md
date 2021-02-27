# 高亮显示&搜索过滤&排序&分页

## 03_高亮显示需求分析

> 搜索字键字高亮显示，也就是在关键字前后加上html标签进行格式化。【参考京东商城】
>
> ![1562892807559](assets/1562892807559.png)

## 04_高亮显示(后端代码)

> 用Spring Data ElasticeSearch的API实现搜索关键字高亮显示

#### 操作步骤

+ 第一步：搜索查询构建对象设置根据多条件匹配查询

+ 第二步：创建高亮字段对象

+ 第三步：搜索查询构建对象设置高亮字段对象

  ```java
  // 设置根据多条件匹配查询:
  builder.withQuery(QueryBuilders.multiMatchQuery(keywords,
          "title", "category", "brand", "seller"));
  
  // 创建高亮字段对象
  HighlightBuilder.Field field = new HighlightBuilder
                  .Field("title") // 设置需要高亮的字段
                  .preTags("<font color='red'>") // 设置高亮前缀
                  .postTags("</font>") // 设置高亮后缀
                  .fragmentSize(50); // 设置文本截断
  // 设置高亮字段对象
  builder.withHighlightFields(field);
  ```

+ 第四步：分页查询，对搜索结果进行转化(获取标题高亮内容)

## 05_高亮显示(前端代码)

> 目标：在html页面显示高亮内容

#### 小结

> 注意：v-html指令用于显示html内容

## 06_搜索业务规则分析

#### 业务规则

+ 根据关键字搜索

+ 搜索关键字高亮显示

+ 根据商品分类过滤

+ 根据商品品牌过滤

+ 根据规格选项过滤

+ 根据商品价格过滤

  ![1562896109341](assets/1562896109341.png)

+ 搜索分页

  ![1562896174865](assets/1562896174865.png)

+ 搜索排序(综合、销量、新品、评价、价格)

  ![1562896255564](assets/1562896255564.png)

## 07_添加过滤条件(前端代码)

> 封装过滤条件请求参数

#### 实现步骤

+ 第一步：在data中定义过滤条件封装变量
+ 第二步：为过滤条件绑定点击事件
+ 第三步：定义事件方法，封装过滤条件
+ 第四步：在面包屑上显示过滤条件

## 08_删除过滤条件(前端代码)

> 减少过滤条件请求参数

#### 实现步骤

+ 第一步：绑定点击事件

+ 第二步：定义事件方法，删除过滤条件

  ```javascript
  var json = {'name'  : 'admin', age : 20};
  alert(JSON.stringify(json));
  delete json.age;
  alert(JSON.stringify(json));
  ```

  ![1562898406419](assets/1562898406419.png)


## 09_隐藏搜索面板(前端代码)

> 页面动画效果，使用v-if指令

#### 实现步骤

+ 使用v-if控制div过滤条件，显示或隐藏

## 10_过滤查询(后端代码)

{ "keywords": "小米", "category": "手机", "brand": "苹果", "price": "1000-1500", "spec": { "网络": "电信3G", "机身内存": "128G" } } 

#### 实现步骤

+ 第一步：创建组合查询构建对象，组合多个过滤条件
+ 第二步：按商品分类过滤
+ 第三步：按商品品牌过滤
+ 第四步：按商品规格过滤(嵌套查询)
+ 第五步：按商品价格过滤(范围查询)
+ 第六步：原生搜索查询对象添加过滤查询

## 11_过滤查询整体测试

> 测试分类过滤、品牌过滤、规格过滤、价格过滤

## 12_搜索分页需求分析

> 在index.html搜索页面，显示页码
>
> 前端：当前页码(page = 1)
>
> 后端：搜索分页
>
> ![1562902196846](assets/1562902196846.png)

## 13_搜索分页(后端代码)

> 接收分页参数：当前页码

#### 实现步骤

+ 第一步：获取当前页码参数

+ 第二步：搜索查询对象设置分页对象

+ 第三步：获取总页数

## 14_搜索分页(前端代码)

#### 实现步骤

+ 显示页码(根据总页数生成页码数组)
+ 显示搜索条件与总记录
+ 根据页码查询
+ 显示省略号
+ 页码不可用样式
+ 搜索起始页码处理

## 15_搜索排序(后端代码)

> 实现搜索排序通用代码
>

#### 实现步骤

+ 第一步：获取排序参数

+ 第二步：创建排序对象 new Sort(ASC|DESC, filed)

+ 第三步：搜索查询对象添加排序对象 searchQuery.addSort(sort);

  ```java
  String sortField = (String)params.get("sortField");
  String sortValue = (String)params.get("sortValue");
  if (StringUtils.isNoneBlank(sortField) && StringUtils.isNoneBlank(sortValue)){
      // 创建排序对象
      Sort sort = new Sort("ASC".equals(sortValue) ?
              Sort.Direction.ASC : Sort.Direction.DESC, sortField);
      // 添加排序对象
      query.addSort(sort);
  }
  ```

## 16_搜索排序(前端代码)

> 封装排序请求参数

#### 实现步骤

- 第一步：按商品价格排序
- 第二步：按上架时间排序

## 17_搜索页与首页对接

#### 实现步骤

+ pinyougou-portal-web将用户搜索关键字传入搜索系统

+ pinyougou-search-web接收传入的搜索关键字，再调用search()方法

