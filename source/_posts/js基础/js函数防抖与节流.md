---
title: js函数防抖与节流 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-08-28 13:57:00 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [js, 前端] #文章标签，可空，多标签请用格式，注意:后面有个空格
---

在前端开发中，很多场景会频繁地触发事件，出于效率或者业务等方面的考虑，我们不想频繁触发事件，这时候就需要用到函数的防抖和节流。

## 准备材料
```js
<div id="content" style="height:150px;line-height:150px;text-align:center; color: #fff;background-color:#ccc;font-size:80px;"></div>
 
<script>
    let num = 1;
    let content = document.getElementById('content');
 
    function count() {
        content.innerHTML = num++;
    };
    content.onmousemove = count;
</script>
```

## 函数防抖(debounce)
短时间内多次触发同一事件，只在最后一次执行目标函数，或者只在最开始的一次执行目标函数，中间的不执行。

实现方式是将目标函数传递给`debounce`函数进行处理，`debounce`函数返回一个带有防抖功能的新函数。实际上每次触发事件仍然会执行该函数，但是只有在满足某些条件时，才会执行原来的目标函数。

根据执行目标函数的时间区分，有立即执行版的函数防抖和非立即执行版的函数防抖。

* 立即执行版本的函数防抖：触发某事件后，立刻执行该事件的目标函数，但后续触发事件都不会使目标函数执行，直到最后一次触发事件然后经过指定时间，再次触发事件才可执行目标函数。
* 非立即执行版本的函数防抖：触发某事件后不会立即执行目标函数，直到最后一次触发事件然后经过指定时间，才会执行目标函数。

代码实现：
```js
funciton debounce(func,wait,immediate){
    let timer; // 存储定时器的编号
    
    // 返回添加了防抖功能的函数。
    return function(){
        let context = this,
            args = arguments;

        // 一定需要先清除已有的定时器。
        // 立即执行版的定时器，用于判断能否触发func。如果当前存在timer，说明已经执行过func，需要等待一定时间后才可再次执行该func，故一定要重新设置setTimeout。等待时间一到，此时再次触发该事件，才可执行func（设置timer=null）。
        // 非立即执行版的定时器，用于触发func。如果当前存在timer，需要清除该timer，并重新设置setTimeout，因为我们期望的是”用户最后一次触发事件然后经过指定时间，才可执行func“
        if(timer) {
            clearTimeout(timer);
        }

        if(immediate){
            if(!timer){
                func.apply(context,args);   // 一段时间未触发该事件，执行func。
            }
            timer = setTimeout(function(){timer=null},wait);
        }else{
            timer = setTimeout(function(){
                func.apply(context,arguments);
            },wait)
        }
    }
}
```

## 函数节流(throttle)
指连续触发事件但是在 n 秒中只执行一次目标函数。即 2n 秒内执行 2 次、3n秒内执行3次...以此类推。会稀释函数的执行频率。


有两种不同的实现方式，根据实现方式的不同，触发目标函数的时刻也不相同。
1. 时间戳实现，在时间段开始的时候触发函数。
2. 定时器实现，在时间段结束的时候触发函数。

```js
/**
 * @desc 函数节流
 * @param func 函数
 * @param wait 延迟执行毫秒数
 * @param type 1 表时间戳版，2 表定时器版
 */
function throttle(func,wait,type){
    let previous = 0,
        timeout;
    
    return function(){
        let context = this,
            args = arguments;
        
        if(type === 1){
            let now = Date.now(); // 记录当前时间
            if(now - previous > wait){ // 如果当前时间-上一次执行func的时间>时间间隔，则可以触发func
                func.apply(context,args);
                previous = now;
            }
        }else if(type === 2){
            if(!timeout){ // 如果当前没有执行过func或刚执行完毕func，则触发setTimeout，经过wait时间后调用func。
                timeout = setTimeout(function(){
                    timeout = null;
                    func.apply(context,args);
                },wait)
            }
        }
    }
}
```

## 关于节流/防抖函数中 context（this） 的指向解析
### 首先要明确一个问题，即节流/防抖函数执行后返回的匿名函数的this指向问题。
由于节流/防抖函数执行后返回的是一个匿名函数，如果无其他操作，该匿名函数内部的`this`应为window或其他全局对象。但是该匿名函数被赋给了`content.onmousemove`事件，形成了一个函数表达式，因此该函数内部的`this`此时指向绑定该事件的节点，即事件的`currentTarget`。

### 其次要明确，func函数内部的this指向问题。
在执行被节流/防抖的目标函数`func`时，如果直接调用(即`func()`)，其调用者为全局对象，因此`func`函数内部的`this`为`window`。如果我们需要`func`函数内部的`this`指向当前绑定该事件的元素，则需要使用`apply`方法手动改变`func`的`this`。

### 怎么改变func函数内部的this指向调用该事件的dom节点呢？
我们只需需要将匿名函数中的`this`传递给`func`，因为`throttle`/`debounce`返回的匿名函数中，`this`指向的即为调用该事件的dom节点。

在`func`无`setTimeout`包裹的情况下，直接使用`func.apply(this,arguments)`；但是如果有`setTimeout`包裹呢？

这里给出两种传递方法
1. 将匿名函数的this使用变量记录下来，然后使用`func.apply(context,args)`，即本文函数防抖与节流的方法。
2. 使用箭头函数的写法编写setTimeout的回调函数，如：`setTimeout(()=>{func.apply(this,arguments)},delay)`

#### 方法1：将匿名函数的this使用变量记录下来，然后使用`func.apply()`，即本文函数防抖与节流的方法。

为什么不直接写`func.apply(this,arguments)`呢？

《JavaScript高级程序设计》第二版中，写到：“超时调用的代码都是在全局作用域中执行的，因此函数中this的值在非严格模式下指向window对象，在严格模式下是undefined”。

因为setTimeout是一种超时调用，其回调函数的内部作用域便是全局作用域，所以不能直接写`func.apply(this,arguments)`。

我们可以利用闭包，将匿名函数的`this`用另一个变量`context`记录下来，然后在setTimeout中使用，即：`func.apply(context,args)`，便实现了改变`func`内部的`this`为匿名函数的`this`。

### 方法2：使用箭头函数的写法编写setTimeout的回调函数，如：`setTimeout(()=>{},delay)`
箭头函数没有自己的this。箭头函数的作用域为定义时箭头函数所在的作用域，即被绑定到创建它时的上下文环境中。

因此，如果我们使用箭头函数的写法，则`setTimeout`的回调函数的作用域为匿名函数的作用域，然后通过`func.apply(this,argumnets)`，便实现了改变`func`内部的`this`为匿名函数的`this`，而匿名函数的`this`指向调用该事件的dom节点，故实现了改变func函数内部的this指向调用该事件的dom节点。


## 参考资料
1. [js 函数的防抖(debounce)与节流(throttle)](https://juejin.im/post/6844903651278848014#heading-1)
2. [【javascript 技巧】谈谈setTimeout的作用域以及this的指向问题](https://www.cnblogs.com/hutaoer/p/3423782.html)
