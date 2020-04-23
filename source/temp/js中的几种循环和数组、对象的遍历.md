### 1. for循环
最简单的循环。写起来麻烦，但几乎什么啥都能干。

```
for(循环体运行之前执行，一般为赋值语句;循环条件判断语句;循环体运行完毕后执行，){
    循环体
}
```


### 2. foreach
语法
```
数组.foreach(callback(currentValue[,index[,array]])[,thisArg]);
```
参数：
* callback:为数组中每个元素执行的函数，该函数接收三个参数：
* currentValue:数组中正在处理的当前元素。
* index 可选:数组中正在处理的当前元素的索引。
* array 可选:forEach() 方法正在操作的数组。
* thisArg 可选 可选参数。当执行回调函数 callback 时，用作 this 的值。

返回值：
* undefined

其他注意事项：
* forEach() 遍历的范围在第一次调用 callback 前就会确定。调用 forEach 后添加到数组中的项不会被 callback 访问到。
* 除了抛出异常以外，没有办法中止或跳出 forEach() 循环

### 3. 
