---
title: js对象 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-08-23 23:39:00 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [js, 前端] #文章标签，可空，多标签请用格式，注意:后面有个空格
---

## 对象的属性
1. 数据属性
    * `configurable`：能否通过delete删除属性从而重新定义属性、能否修改属性的特性、能否把属性修改为访问器属性。对于直接在对象上定义的属性，默认为true。
    * `enumerable`：表示能否通过for-in循环返回属性。默认为true。
    * `writable`：表示能否修改属性的值。默认为true。
    * `value`：包含这个属性的数据值。默认为undefined。
2. 访问器属性
    * `configurable`：表示能否通过delete删除属性从而重新定义属性、能否修改属性的特性、能否把属性修改为数据属性。对于直接在对象上定义的属性，默认为true。
    * `enumerable`：表示能否通过for-in循环返回属性。默认为true。
    * `get`：在读取属性时调用的函数，默认值为undefined。
    * `set`：在写入属性时调用的函数，默认值为undefined。
3. 其他属性
    * `constructor`：对象的构造函数。
3. 相关方法
    * 定义属性描述符的方法1：`Object.defineProperty(对象, 属性名, 描述符对象)`
    * 定义属性描述符的方法2：`Object.defineProperties(对象, 描述符对象)`
    * 读取属性描述符的方法：`Object.getOwnPropertyDescriptor(对象, 属性名)`


## 创建对象

### 1. 工厂模式
```js
function createPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    }
    return o;
};
var person1 = createPerson("Nicholas", 29, "Software Engineer");
```

### 2. 构造函数模式
创建实例时实际经历的步骤：
1. 创建一个对象。
2. 将构造函数的作用域赋给新对象
3. 执行构造函数中的代码。
4. 返回新对象。
```js
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    }
}
var person1 = new Person("Nicholas",29,"Software Enigneer");
```
注意：
1. 构造函数始终应该以大写字母开头。
2. 构造出的不同对象的方法并不共用同一片内存。

### 3. 原型模式
创建新函数时，会为该函数创建一个prototype属性，该属性指向该函数的原型对象。默认情况下，所有的原型对象都会具有一个`constructor`属性，这个属性是一个指向`prototype`属性所在函数的指针，如下所示
```js
var NewFunction = function(){}
NewFunction.prototype.constructor === NewFunction // true.
```
#### 读取一个对象的属性，发生了什么？
1. 首先搜索对象实例本身。如果在实例中找到了具有给定名字的属性，则返回该属性的值。
2. 若第一步没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。

#### 原型添加属性和方法
1. 通过`prototype`属性。如:`NewFunction.prototype.a = 1`
2. 通过对象字面量的方法，此时原型的constructor不再指向原函数，但是`instanceof`操作符仍然会返回true。可以通过显示地向对象添加constructor属性，以保证原型的constructor指向原函数。另外需要注意，使用对象字面量的方法，可能会重写原型链。
    ```js
    function NewFunction(){};
    NewFunction.prototype = {
        a:1
    }
    NewFunction.prototype.constructor === NewFunction; // false
    var newFunctionObj = new NewFunction();
    newFunctionObj instanceof NewFunction // true
    NewFunction.prototype = {
        constructor:NewFunction,
        a:1,
    }
    NewFunction.prototype.constructor === NewFunction; // true
    ```
3. 使用`Object.defineProperty()`。

#### 原型的动态性
对原型对象所做的任何修改，都能立刻从实例上反映出来。

#### 原型相关的方法
1. `prototypeObject.prototype.isPrototypeOf(被测对象)` 用于prototypeObject是否存在于被测对象的原型链上。
2. `Object.getPrototypeOf()`，用于获取指定对象的原型
3. `obj.hasOwnProperty(属性名)`，方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性。不包括原型中的属性。
4. `"属性名" in obj`，`in`操作符可检测对象是否有相应属性，不论属性在实例中还是在原型中。
5. `Object.keys(obj)`,获取obj对象上的所有可枚举的实例属性。
6. `Object.getOwnPropertyNames(obj)`，获取obj对象上的所有实例属性，无论是否可枚举。

### 4. 组合使用构造函数模式和原型模式
使用构造函数模式定义实例属性，使用原型模式定义方法和共享的属性。

### 5. 动态原型模式
将实例属性、原型方法和原型属性都定义在构造函数中。如下：
```js
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    // 仅需检测一个原型中可能存在的属性或方法即可。若该属性/方法在原型中不存在，则将原型上应存在的所有属性和方法添加至原型。
    if(typeof this.sayName !== "function"){
        // 也可使用instanceof，如：
        // if(this.sayName instanceof Function)
        Person.prototype.sayName = function(){
            alert(this.name);
        }
        Person.prototype.sayHello = function(name){
            alert(`My name is ${this.name}, nice to meet you ${name}`)
        }
    }
} 
```

### 6. 寄生构造函数模式
创建一个函数，该函数仅封装创建对象的代码，然后再返回新创建的对象。主要用来为现有的引用类型增加其他的功能，且不污染现有的引用类型。

与工厂模式有两个区别：
1. 命名上，不再采用createXxxx。
2. 在实例化时，通过`new`运算符进行实例化，而不是仅调用方法。
```js
function SpecialArray(){
    var values = new Array();
    values.push.apply(values,arguments);
    values.toPipedString = function(){
        return this.join("|");
    }
    return values;
}
var colors = new SpecialArray("red","blue","green");
alert(colors.toPipedString());
colors instanceof SpecialArray // false
```

注意：寄生构造函数返回的对象与构造函数及构造函数的原型之间没有任何关系。

### 7. 稳妥构造函数模式
稳妥对象：没有公共属性，而且其方法也不引用this。

稳妥构造函数模式不适用`new`操作符调用构造函数。
```js
function Person(name,age,job){
    var o = new Object();

    // 在此处定义私有变量

    o.sayName = function(){
        alert(name);
    }
    return o;
}
```

## 原型链
基本思想：利用原型让一个引用类型继承另一个引用类型的属性和方法，是js语言实现继承的主要方法。

### 确定原型和实例的关系
1. 方法1：使用`instanceof`
2. 方法2：使用`isPrototypeOf`，如`Object.prototype.isPrototypeOf(instance)`;

### 原型链存在的问题
1. 原型上引用类型值的属性，会被所有实例共享。
2. 在不影响所有对象实例的情况下，无法给超类型的构造函数提供参数。

### 1. 借用构造函数
在子类型构造函数的内部调用超类型构造函数,便可以实现向超类型传递参数。

存在的问题：无法实现函数复用（因为超类型的原型中定义的方法，对于子类型而言是不可见的。）

### 2. 组合继承
一种继承模式，将原型链和借用构造函数的技术组合到一起。主要思路为使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现对实例属性的继承。

组合继承模式有两个关键步骤。在调用子类的构造函数实例化对象前，一定要先将超类挂载至子类的原型上，然后将子类原型的构造方法手动设置为子类的构造方法（因为在挂载原型时，构造方法会被覆盖）。

### 3. 原型式继承
借助原型，基于已有的对象创建新对象，同时也不必创建自定义类型。
```js
function object(o){
    function F(){};
    F.prototype = o;
    return new F();
}
```
ES6的`Object.craete(用作新对象原型的对象, 为新对象定义额外属性的对象)`api实现了原型式继承。


### 4. 寄生式继承
创建一个仅用于封装继承过程的函数，使用该函数实现继承。