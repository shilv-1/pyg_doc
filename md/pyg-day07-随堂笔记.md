# 选择规格&SKU列表&商品列表

## 03_显示规格需求分析

#### 目标

> 显示该商品分类所关联的所有规格选项数据。

#### 需求分析

![1562288038677](assets/1562288038677.png)

#### 小结

> 前端需要的数据: 
>
>  [{"id":27,"text":"网络","options": [{},{}]},
>
>   {"id":32,"text":"机身内存","options": [{},{}]}]
>
> options: 根据规格的id从规格选项表查询
>
> 后端返回的数据: 
>
> List<Map<String,Object>> data;

## 04_显示规格选项(前端代码)

#### 目标

> 实现显示规格选项前端代码

#### 实现步骤

+ 监控类型模板id发生改变查询规格选项数据.

#### 小结

> 数据格式:  [{"id":27,"text":"网络","options": [{},{}]},
>
>   {"id":32,"text":"机身内存","options": [{},{}]}]

## 05_显示规格选项(后端代码)

#### 目标

> 实现显示规格选项后端代码

#### 实现步骤

+ 控制器

+ 服务接口

+ 服务接口实现类

  + 根据类型模板id查询类型模板对象,获取spec_ids的数据

    [{"id":27,"text":"网络"},{"id":32,"text":"机身内存"}]

  + 把specIds转化成List<Map>

  + 迭代规格集合，根据id查询规格选项，把查询出来的放到map集合，最后返回List

+ 数据访问接口

#### 小结

> 数据格式: 
>
> List<Map<String,Object>> data;
>
> [{"id":27,"text":"网络","options": [{},{}]},
>
>   {"id":32,"text":"机身内存","options": [{},{}]}]

## 06_保存选中规格选项(需求分析)

#### 目标

> 用户选中的规格选项数据如何封装?

![1562290698406](assets/1562290698406.png)

#### 需求分析

+ 数据保存到tb_goods_desc: specification_items

  [{"attributeValue":["联通4G","移动4G","电信4G"],"attributeName":"网络"},{"attributeValue":["64G","128G"],"attributeName":"机身内存"}]

+ 前端数据封装 

  goods.goodsDesc.specificationItems = []

#### 小结

> 数据最终保存到tb_goods_desc表的specification_items列。

## 07_保存选中规格选项(前端代码)

#### 目标

> 实现保存选中规格选项前端代码

#### 实现步骤

+ 为checkbox绑定点击事件
+ 提供事件方法，做数据封装

#### 小结

> ![1562291250667](assets/1562291250667.png)
>
> 数据格式: goods.goodsDesc.specificationItems = [] 
>
> [{"attributeValue":["联通4G","移动4G","电信4G"],"attributeName":"网络"},{"attributeValue":["64G","128G"],"attributeName":"机身内存"}]

## 08_SKU商品信息需求分析

#### 目标

> SKU商品信息怎么封装? 
>
> goods.items = 
>
> [{price : '', num : 999, status : '1', idDefault : '1', spec: {}}]
>
> SKU商品信息怎么生成?
>
> + 不断对原来的SKU数组进行扩充

#### 需求分析

+ 前端数据封装

  goods.items = 

  [{price : '', num : 999, status : '1', idDefault : '1', spec: {}}]

#### 小结

> 关键点: 采用goods.items数组封装。

## 09_生成SKU列表(前端代码)

#### 目标

> 实现生成SKU列表前端代码

#### 实现步骤

```javascript
// 生成SKU数组
createItems : function () {
    /** 定义SKU数组变量，并初始化 */
    this.goods.items = [{spec:{}, price:0, num:9999,status:'0', isDefault:'0'}];
    /**
     * [{"attributeValue": ["移动4G", "联通3G", "联通4G" ], "attributeName": "网络" },
     *  {"attributeValue": ["64G", "128G" ], "attributeName": "机身内存" } ]
     */
    // 获取用户选中的规格选项数组
    var specItems = this.goods.goodsDesc.specificationItems;
    // 根据用户选择规格选项生成SKU数组
    for (var i = 0; i < specItems.length; i++){
        // 获取一个数组元素
        // { "attributeValue": [ "移动4G", "联通3G", "联通4G" ], "attributeName": "网络" }
        var specItem = specItems[i];
        // 克隆原来的SKU数组，产生新的SKU数组
        this.goods.items = this.swapItems(this.goods.items,
            specItem.attributeValue, specItem.attributeName);
    }
},
// 克隆原来的SKU数组，产生新的SKU数组
swapItems : function (items, attributeValue, attributeName) {
    // items : [{spec:{}, price:0, num:9999,status:'0', isDefault:'0'}];
    // attributeValue : [ "移动4G", "联通3G", "联通4G" ]
    // attributeName : "网络"
    // 定义新的SKU数组
    var newItems = [];

    for (var i = 0; i < items.length; i++){
        // item : {spec:{}, price:0, num:9999,status:'0', isDefault:'0'}
        var item = items[i];
        //  [ "移动4G", "联通3G", "联通4G" ]
        for (var j = 0; j < attributeValue.length; j++){
            // 克隆item产生新的item
            // {spec:{}, price:0, num:9999,status:'0', isDefault:'0'}
            var newItem = JSON.parse(JSON.stringify(item));
            // 设置规格选项 spec : {"网络":"联通4G","机身内存":"64G"}
            newItem.spec[attributeName] = attributeValue[j];
            newItems.push(newItem);
        }
    }
    return newItems;
}
```

#### 小结

> 需要不断的对原SKU数组进行扩充。

## 10_显示SKU列表(前端代码)

#### 目标

> 实现显示SKU列表前端代码

#### 实现步骤

```javascript
<tr v-for="item in goods.items">
   <td v-for="s in goods.goodsDesc.specificationItems">
      {{item.spec[s.attributeName]}}
   </td>

   <td><input class="form-control"
            v-model="item.price"
            placeholder="价格">
   </td>
   <td><input class="form-control"
            v-model="item.num"
            placeholder="库存数量">
   </td>
   <td><input type="checkbox"
            :true-value="1"
            :false-value="0"
            v-model="item.status"></td>
   <td><input type="checkbox"
            :true-value="1"
            :false-value="0"
            v-model="item.isDefault"></td>
</tr>
```

#### 小结

> 需用到:true-value、:false-value指令改变checkbox默认值。

## 11_SKU商品信息(后端代码)

#### 目标

> 保存SKU商品信息到tb_item表

#### 实现步骤

+ 修改服务接口实现类中的save方法

#### 小结

> 循环往tb_item表中插入数据。

## 12_商品是否启用规格

#### 目标

> 实现商品是否启用规格功能
>
> 商品是否启用规格?

#### 实现步骤

goods.isEnableSpec 

![1562299250771](assets/1562299250771.png)

#### 小结

> + 启用规格代表tb_item表中可以插入多条数据。
> + 不启用规格代表tb_item表中只能插入一条数据。

## 13_查询商品列表(前端代码)

#### 目标

> 实现多条件分页查询商品列表的前端代码

#### 实现步骤

![1562300249564](assets/1562300249564.png)

#### 小结

> 分页参数、查询条件封装、发送异步请求、迭代数据显示。

## 14_查询商品列表(后端代码)

#### 目标

> 实现多条件分页查询商品列表后端代码

#### 实现步骤

+ 控制器
+ 服务接口
+ 服务接口实现类
+ 数据访问接口

#### 小结

> 接收请求参数、GET请求中文转码、动态查询、响应数据。