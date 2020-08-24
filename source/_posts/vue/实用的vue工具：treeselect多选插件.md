---
title: 实用vue工具推荐：treeselect多选插件 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-04-02 11:35:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 软件设计 #分类

tags: 软件设计 #文章标签，可空，多标签请用格式，注意:后面有个空格

description: 23种设计模式，包括设计模式的UML图、定义、优缺点及java代码实现

thumbnail: http://cdn.ewinds.pw/kungfu.jpeg
---


<!-- more -->
### 常用属性
#### options属性
options属性是一个数组，每一个对象为多选框的一条记录。每条记录拥有如下三个属性：
* id:该记录的唯一标识符，选中后将该值存放进v-model绑定的数组内。
* label：用户看到的、显示的内容
* children:子数据。

#### normalizer属性
normalizer属性用于为options的id、label和children属性设置别名。格式如下：
```
normalizer(node){
    return {
        id:node.key,                // options数组中对象的key属性作为原id属性。
        label:node.name,            // options数组中对象的name属性作为原label属性。
        children:node.subOptions    // options数组中对象的children属性作为原children属性。
    }
}
```

