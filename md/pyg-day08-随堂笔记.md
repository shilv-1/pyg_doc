# 商品审核&删除&上下架&广告管理

## 03_待审核商品列表

#### 目标

> 实现多条件分页查询全部商家待审核商品列表

#### 实现步骤

+ 前端代码

  ![1562546862241](assets/1562546862241.png)

+ 后端代码

  + 控制器

#### 小结

> 商品审核状态为: 未审核。

## 04_商品审核与驳回

#### 目标

> 实现商品审核通过与审核不通过功能

#### 实现步骤

- 前端代码

  ![1562547857894](assets/1562547857894.png)

  ![1562548348525](assets/1562548348525.png)

- 后端代码

  UPDATE `tb_goods` SET audit_status = ? WHERE id IN(?,?,?)

#### 小结

> 修改tb_goods表audit_status列的数据

## 05_商品逻辑删除

#### 目标

> 实现商品逻辑删除功能

#### 实现步骤

+ 前端代码

  ![1562550149394](assets/1562550149394.png)

+ 后端代码

  UPDATE `tb_goods` SET is_delete = ? WHERE id IN(?,?,?)

#### 小结

> 修改tb_goods表is_delete列的数据

## 06_排除已删除记录

#### 目标

> 查询商品时，需排除已删除的商品。

#### 实现步骤

![1562551438361](assets/1562551438361.png)

#### 小结

> 修改多条件查询商品的SQL语句。

## 07_商品上下架

#### 目标

> 理解商品上下架目的

#### 实现步骤

+ 前端代码

  ![1562551871105](assets/1562551871105.png)

+ 后端代码

  UPDATE tb_goods SET is_marketable = ? WHERE id IN(?,?,?)

> 修改tb_goods表的列数据。

## 08_搭建内容服务工程

#### 目标

> 完成搭建内容服务工程

#### 搭建步骤

> 参考课程讲义

## 09_代码生成器

#### 目标

> 用代码生成器生成广告分类、广告内容两张表的CRUD代码。

#### 生成代码

+ 生成实体类(已生成)
+ 生成数据访问接口(已生成)
+ 生成数据访问SQL映射文件(已生成)
+ 生成服务接口(已生成)
+ 生成服务接口实现类
+ 生成控制器类
+ 生成前端js文件

#### 小结

> 如果只是CRUD功能，生成相关代码后，就能实现快速实现CRUD功能。

## 10_网站前台首页分析

#### 目标

> 了解网店前台首页有哪些功能?

#### 首页分析

+ 首页海报（轮播图）
+ 今日推荐
+ 猜你喜欢
+ 楼层广告

#### 小结

> 网站前台首页  又可以称为  门户系统
>
> 门户系统: 一般就是电商平台的入口。

## 11_广告类型管理

#### 目标

> 实现广告类型表CRUD功能

#### 实现步骤

+ 分页查询广告类型
+ 添加广告类型
+ 修改广告类型
+ 删除广告类型

#### 小结

> 后端代码无须修改

## 12_分页查询广告内容

#### 目标

> 实现分页查询广告内容

#### 实现步骤

+ 前端代码
+ 后端代码(已生成)

#### 小结

> 前端发异步请求、显示页码、迭代数据显示。

## 13_广告类型下拉列表

#### 目标

> 实现广告类型下拉列表功能

#### 实现步骤

- 前端代码
- 后端代码(已生成)

#### 小结

> v-for指令迭代产生下拉列表框。

## 14_添加广告内容

#### 目标

> 实现添加广告内容功能

#### 实现步骤

- 前端代码
- 后端代码(已生成)

#### 小结

> 广告图片异步上传(参考商品图片异步上传)。

## 15_修改广告内容

#### 目标

> 实现修改广告内容功能

#### 实现步骤

- 前端代码
- 后端代码(已生成)

## 16_删除广告内容

#### 目标

> 实现删除广告内容功能

#### 实现步骤

- 前端代码
- 后端代码(已生成)

#### 小结

> 传多个广告内容id到后端。

