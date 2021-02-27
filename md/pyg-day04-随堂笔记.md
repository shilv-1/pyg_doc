# 商品分类管理&FastDFS

## 03_商品分类管理需求分析

#### 目标

> 了解商品分类表结构、CRUD功能

![1561943214529](assets/1561943214529.png)

![1561943303320](assets/1561943303320.png)

#### 需求分析

+ 查询功能

+ 添加功能

+ 修改功能

+ 删除功能


## 04_查询商品分类

#### 目标

> 实现商品分类查询(三级分类)

SELECT * FROM `tb_item_cat` WHERE parent_id = ?

![1561943680774](assets/1561943680774.png)

#### 实现步骤

+ 一级分类列表查询

+ 二级分类列表查询

+ 三级分类列表查询

  ```java
   @Override
      public List<ItemCat> findItemCatByParentId(Long parentId) {
          try{
              // SELECT * FROM `tb_item_cat` WHERE parent_id = 0
              ItemCat itemCat = new ItemCat();
              itemCat.setParentId(parentId);
              // 条件查询
              return itemCatMapper.select(itemCat);
          }catch (Exception ex){
              throw new RuntimeException(ex);
          }
      }
  ```

#### 小结

> 一级分类parent_id为0、查询二级分类需通过一级分类的id、查询三级分类需通过二级分类的id。

## 05_面包屑导航

#### 目标

> 显示面包屑导航,点击上面的分类实现返回操作。

在三级分类时，需要隐藏 查询下级

![1561945782507](assets/1561945782507.png)

#### 实现步骤 

+ 在data中定义两个变量

+ 在定义级别变量，用于判断商品分类的级别

  ![1561947017831](assets/1561947017831.png)

#### 小结

> 需要把用户选择的一级分类、二级分类放入data数据模型中。

## 06_添加商品分类需求分析

#### 目标

> 添加商品分类需求分析

![1561947281900](assets/1561947281900.png)

#### 实现步骤

- 父级id
- 类型模板id

#### 小结

> 往tb_item_cat表添加数据

## 07_添加商品分类(前端代码)

#### 目标

> 完成添加商品分类前端代码

![1561947634746](assets/1561947634746.png)

#### 实现步骤

- 记住父级id

  ![1561947658326](assets/1561947658326.png)

- 数据绑定

## 08_类型模板下拉列表

#### 目标

> 异步加载类型模板产生下拉列表

#### 实现步骤

- 在vue的data中定义变量
- 在vue的methods中定义发送异步请求的方法，查询类型模板
- 在created生命周期方法，调用初始化方法
- v-for迭代产生下拉列表

#### 小结

> SELECT id,name FROM `tb_type_template`

## 09_添加商品分类(后端代码)

#### 目标

> 完成添加商品分类后端代码

#### 实现步骤

- 控制器ItemCatController
- 服务接口
- 服务接口实现类
- 数据访问接口

## 10_修改商品分类

#### 目标

> 完成修改商品分类

![1561951264549](assets/1561951264549.png)

#### 实现步骤

- 前端代码

  ![1561950891993](assets/1561950891993.png)

- 后端代码

  + 控制器
  + 服务接口
  + 服务接口实现类
  + 数据访问接口

## 11_删除商品分类

#### 目标

> 完成删除商品分类
>
> 当你删除一级分类时，二级分类，三级分类都需要删除

#### 实现步骤

- 前端代码

  ![1561951690767](assets/1561951690767.png)

- 后端代码

  + 服务接口实现类(递归查询)

    ```java
     @Override
        public void deleteAll(Serializable[] ids) {
            try{
                // DELETE FROM tb_item_cat WHERE id IN(?,?,?)
                // 1. 定义List集合封装需要删除的商品分类id
                List<Long> idList = new ArrayList<>();
    
                // 2. 封装删除的商品分类id
                for (Serializable id : ids) {
                    // 添加id
                    idList.add((long)id);
                    // 调用递归的方法
                    findLeafNode(id, idList);
                }
    
                // 3. 批量删除
                Example example = new Example(ItemCat.class);
                // 创建条件对象
                Example.Criteria criteria = example.createCriteria();
                // id IN(?,?,?)
                criteria.andIn("id", idList);
                // 条件删除
                itemCatMapper.deleteByExample(example);
            }catch (Exception ex){
                throw new RuntimeException(ex);
            }
        }
    
        /** 根据父节点查询子节点(递归查询) */
        private void findLeafNode(Serializable parentId, List<Long> idList) {
            // 查询下级分类
            List<ItemCat> itemCatList = findItemCatByParentId((long)parentId);
            // 递归退出条件
            if (itemCatList != null && itemCatList.size() > 0){
                for (ItemCat itemCat : itemCatList) {
                    // 封装子节点的id
                    idList.add(itemCat.getId());
                    // 递归查询
                    findLeafNode(itemCat.getId(), idList);
                }
            }
        }
    ```

#### 小结

> DELETE FROM `tb_item_cat` WHERE id IN(?,?,?)

## 12_FastDFS简介

#### 目标

> 了解FastDFS分布式文件服务器
>
> 分布式文件服务器：是解决高并发的一种解决方案。(减少Web服务器的访问压力)

#### 小结

> FastDFS包括: 追踪服务器、存储服务器。

![1561955331296](assets/1561955331296.png)

## 13_Java客户端操作FastDFS

#### 目标

> 利用FastDSF的java客户端操作FastDFS文件服务器

#### 操作步骤

+ 安装FastDFS客户端jar包至本地仓库(fastdfs-client)

+ 配置依赖FastDFS客户端jar包

  ```xml
  <!-- 配置FastDFS的java语言客户端 -->
  <dependency>
      <groupId>org.csource</groupId>
      <artifactId>fastdfs-client</artifactId>
      <version>1.25-RELEASE</version>
  </dependency>
  ```

+ 上传文件到FastDFS服务器

  + 提供fastdfs-client.conf配置文件(配置追踪服务器的连接地址)

    ![1561956379726](assets/1561956379726.png)

  + 编程部分

    + 获取fastdfs-client.conf配置文件

    + 初始化客户端全局对象(连接追踪服务器)

    + 创建存储服务器对应的客户端

    + 上传文件到FastDFS

      ```java
        // 1. 获取fastdfs-clinet.conf配置文件
              String path = this.getClass().getResource("/fastdfs-client.conf").getPath();
              System.out.println("path = " + path);
      
              // 2. 初始化客户端全局对象(连接tracker服务器)
              ClientGlobal.init(path);
      
              // 3. 创建存储客户端对象
              StorageClient storageClient = new StorageClient();
      
              // 4. 上传文件到FastDFS
              String[] arr = storageClient.upload_file("C:\\Users\\Administrator\\Pictures\\11.jpg", "jpg", null);
              /**
               * http://192.168.12.131/group1/M00/00/02/wKgMg10Zkc-AWwpcAAMGxOgwmic560.jpg
               * arr = [group1, M00/00/02/wKgMg10Zkc-AWwpcAAMGxOgwmic560.jpg]
               * 数组中第一个元素：组名
               * 数组中第二个元素：远程名称
               */
              System.out.println("arr = " + Arrays.toString(arr));
      
      ```

+ 下载FastDFS服务器的文件

+ 删除FastDFS服务器的文件

#### 小结

> storageClient.delete_file() : 删除文件
>
> storageClient.upload_file() : 上传文件
>
> storageClient.download_file() : 下载文件