---
title: js设计模式：发布订阅模式 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-4-8 21:35:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 设计模式 #分类

tags: [js, 设计模式]  #文章标签，可空，多标签请用格式，注意:后面有个空格

description: js设计模式
---

通用实现：
```
// 发布者。
var publisherModel = {
    subscriber:{},
    listen:function(key,fn){
        if(!this.subscriber[key]){
            this.subscriber[key] = [];
        }
        this.subscriber[key].push(fn);
    },
    publish:function(){
        let key = Array.prototype.shift.call(arguments),
            fns = this.subscriber[key];
        if(!fns || fns.length === 0){
            return false;
        }
        for(var i=0,fn;fn = fns[i++];){
            fn.apply(this,arguments);
        }
    }
}
// 生成发布者
var installPublisher = function(obj){
    for(var i in publisherModel){
        obj[i] = publisherModel[i];
    }
}
```