---
title: MySQL小记 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-03-15 09:40:30 #文章生成时间，一般不改，当然也可以任意修改

categories: mysql #分类

tags: [mysql] #文章标签，可空，多标签请用格式，注意:后面有个空格

description: 记录使用mysql时常用的语法、函数和语句。

---

记录使用mysql时常用的语法、函数和语句。

<!-- more -->

## 语法

### case when语句
* 用于计算条件列表并返回多个可能结果表达式之一。
* 有两种形式，**简单case函数**和**case搜索函数**

#### 1. 简单case函数
语法如下：
```
CASE input_expression
    WHEN when_expression THEN result_expression
        [ ...n ]
    [
        ELSE else_result_expression
    END
```

#### 2. case搜索函数
```
CASE   
WHEN Boolean_expression THEN result_expression
        [ ...n ]
    [
        ELSE else_result_expression
    END
```

## 常用函数
### 1. convert()
把字段转换成指定类型。

### 2. concat()
* 作用：拼接字符串。
* 语法：concat(str1,str2)

### 3. dayofweek()
* 作用：返回指定日期的工作日索引（即指定日期是一周中的第几天）。其中1是周日，2是周一，以此类推...7是周六。
* 语法：dayofweek(date)

### 4. dayofmonth()
* 作用：返回指定日期所在月的天数的索引（即指定日期是一个月中的第几天）。范围为1到31。
* 语法：dayofmonth(date)

### 5. dayofyear()
* 作用：返回指定日期date所在年的天数的索引（即指定日期是一年中的第几天）。范围为1到366。
* 语法：dayofyear(date)

### 6. weekofyear()
* 作用：返回指定日期所在的星期是这一年的第几个星期。范围为1到53.
* 语法：weekofyear(date)

### 7. adddate(date,INTERVAL expr unit)
* 作用：修改时间。向指定的日期添加指定的时间。
* 语法：adddate(date,INTERVAL expr unit)

### 8. curdate()
* 作用：返回当前的日期。
* 语法：curdate()

### 9. year()
* 作用：返回指定日期的年份。
* 语法：year(date)

### 10. month()
* 作用：返回指定日期的月份。
* 语法：month(date)

### 11. quarter()
* 作用：返回指定日期在一年中的季度。
* 语法：quarter(date)

### 12. sign()
* 作用：根据X是负数、零或正数，将参数的符号返回为-1、0、或1
* 语法：sign(x)

## 常用语句
1. 修改自增字段的起始值。
```
ALTER table table_name
AUTO_INCREMENT=起始值 
```