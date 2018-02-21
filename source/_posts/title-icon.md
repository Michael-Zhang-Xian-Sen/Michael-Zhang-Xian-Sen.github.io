---
title: 动态选项卡title及图标 
date: 2017-05-24 21:12:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [选项卡,title,图标] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 记录一次实现一个小效果的过程。
---


很久之前在一个人的博客上，看到了一个很有意思的效果——当我在他的页面浏览时，选项卡图标和title是一个模样，当我离开他的页面以后，他的博客所在选项卡图标又变成了另一幅模样。当时感觉非常有趣，但没有花时间去考虑如何实现。

<!-- more -->

最近读《JavaScript高级程序设计》第十三章事件时，看到了unload事件。在书中对其的介绍如下：
     与load事件对应的是unload事件，这个事件在文档完全被卸载后触发。只要用户从一个页面转换到另一个页面，就会发生onunload事件。

首先映入脑海的是使用onunload来实现切换选项卡时，选项卡的图标和title进行变换的效果。首先要学习一下onunload事件。

---

#### 第一次尝试：

测试onunload方法：
test2.html
```
<!DOCTYPE html>
<html lang="en">     
<head>
    <meta charset="UTF-8">
    <title>onunload事件</title>
</head>
<body onunload="alert('Unloaded！')">
</body>
</html>
```

在chrome浏览器中，关闭test2.html文档或者切换该选项卡，onunload未起作用。

![啊图片](http://opqksc9nz.bkt.clouddn.com/chromef123.png)

首先，高程上的意思有点磨棱两可。在网上一通谷歌后才明白，“文档完全被卸载”，“用户从一个页面转换到另一个页面”实际是关掉选项卡再进行的切换。实在是坑啊，看来我们方向找错了。
其次，我们来探究一下onunload为何未起作用。在onunload位置打上断点。并将右侧的pause on exceptions打开。如上图所示。这里有一个小知识点。



通过调试，在第五次调试时推出该页面，并提示错误信息“Blocked alert('Unloaded！') during unload.”前四次中，前两步为渲染过程，后两步为网页卸载过程，但是仍不知为何会抛出这个错误，还请大神指点迷津。

---

#### 第二次尝试

搜索一番，更正思路，以下是实现代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>被发现啦(*´∇｀*)</title>
    <link href="../img/1.ico"  rel="icon" type="image/vnd.microsoft.icon"/>
</head>
<body>
<script>
    //浏览器选项卡变换
    //判断hidden属性是否存在于document属性中。
    var hiddenProperty = 'hidden' in document ? 'hidden' :
        'webkitHidden' in document ? 'webkitHidden' :
            'mozHidden' in document ? 'mozHidden' :
                null;
    //用于在字符串中用字符串替代指定字符。第一个参数为原字符串中被替换字符串的正则表达式，第二个参数为用于替换的文本或字符串。
    var visibilityChangeEvent = hiddenProperty.replace(/hidden/i, 'visibilitychange');
    //构造匿名函数，为更换标签页状态事件所要执行的函数。
    var onVisibilityChange = function(){
        //获取link元素
        var links = document.head.getElementsByTagName("link");
        if (!document[hiddenProperty]) {
            document.title='被发现啦(*´∇｀*)';
            links[0].href = "../img/1.ico";
        }else{
            document.title='藏好啦(つд⊂)';
            links[0].href="../img/2.ico";
        }
    };
    //用于向指定元素添加事件句柄，第一个参数为事件名，第二个参数为指定要事件触发时执行的函数。
    document.addEventListener(visibilityChangeEvent, onVisibilityChange);
</script>
</body>
</html>
```
有几点需要注意。
变量hiddenProperty获取的是当前浏览器的hidden属性，使用了三个三元运算符来保证其兼容性。通过replace替换成为visibilitychange+属性名中除hidden外字符串。在这里要着重注意一下visibilitychange这个事件，在我们的这个效果中起到了关键的作用。
通过dom获取link标签。更改title和href。
为元素添加事件句柄使用addEventListener()函数。
最后我们可以发现，实现这个效果，真的是，相当，相当简单。

---

#### 小知识点1：关于chrome的调试

在界面下方能看到按钮，它是设置程序运行时遇到异常时是否中断的开关。点击该按钮会在3种状态间切换：
![all_catch](http://opqksc9nz.bkt.clouddn.com/all_catch.png) 遇到所有异常都会中断，包括已捕获的情况。（两条白色竖杠包含在蓝色八边形中
![some_catch](http://opqksc9nz.bkt.clouddn.com/some_catch.png) 仅在出现未捕获的异常时才中断。（两条白色竖杠包含在紫色八边形中 ）
![no_catch](http://opqksc9nz.bkt.clouddn.com/no_catch.png) 默认遇到异常不中断。（两条白色竖杠包含在黑色八边形中）

#### 小知识点2：visibilitychange事件

浏览器标签页被隐藏或显示的时候会触发visibilitychange事件.
示例程序：

```
document.addEventListener("visibilitychange", function() {
  console.log( document.visibilityState );});
```

该事件具有四个属性：

* target属性：事件的目标。
* type属性：被触发的事件的类型。
* bubbles：表明事件是否冒泡。
* cancelable：表明是否可以取消事件的默认行为。

#### 另外附上一对选项卡图标
觉得这一对相当有趣，怎么用就看客官自由发挥了。
![1](http://opqksc9nz.bkt.clouddn.com/1.ico)
![2](http://opqksc9nz.bkt.clouddn.com/2.ico)