---
title: 《你不知道的JavaScript 上册》第一部分 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-01-31 23:47:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 读书笔记 #分类

tags: [读书笔记, 前端] #文章标签，可空，多标签请用格式，注意:后面有个空格

description: 你不知道的JavaScript

---

第一部分：作用域和闭包

<!-- more -->

# 目录

1. [作用域是什么](#chapter1)
    1.1. [编译原理](#1.1)
    1.2. [理解作用域](#1.2)
2. [词法作用域](#chapter2)
    2.1. [词法阶段](#2.1)
    2.2. [欺骗词法](#2.2)
3. [函数作用域和块作用域](#chapter3)
    3.1. [函数中的作用域](#3.1)
    3.2. [隐藏内部实现](#3.2)
    3.3. [块作用域](#3.3)
4. [提升](#chapter4)
5. [作用域闭包](#chapter5)
    5.2. [实质问题](#5.2)
    5.4. [循环和闭包](#5.4)
    5.5. [模块](#5.5)
6. [附录A](#appendixA)
7. [附录B 块作用域的替代方案](#appendixB)
    B.1. [Traceur](#B.1)
    B.2. [隐式和显式作用域](#B.2)
    B.3. [性能](#B.3)
8. [附录C this词法](#appendixC)

---

## <div id="chapter1">第一章 作用域是什么</div>

### <div id="1.1">1.1 编译原理</div>
* 编译代码的三个步骤
    1. 分词/词法分析：将字符串分解成(对编程语言来说)有意义的代码块，这些代码块被称为词法单元(token)。
    2. 解析/语法分析：将词法单元流(数组)转换成一个由元素逐级嵌套所组成的代表了程序语法结构的树。
    3. 代码生成：将 AST 转换为可执行代码的过程称被称为代码生成。
编译器编译完毕后，由引擎执行生成的代码。

### <div id="1.2">1.2 理解作用域</div>
* LHS查询：变量出现在赋值操作的左侧时进行的查询。试图找到变量的容器本身，`set`操作，当执行非法操作时，比如对一个没有声明的变量进行赋值，非严格模式下的引擎会自动声明该变量，而非严格模式会报错`ReferenceError`。
* RHS查询：变量出现在赋值操作的右侧时进行的查询。retrieve his source value，取到它的值，类似`get`操作，可能会引发两种错误：1. `ReferenceError`同作用域判别失败相关 2. `TypeError`则代表作用域判别成功了，但是对结果的操作是非法或不合理的。

## <div id="chapter2">第二章 词法作用域</div>
作用域的两种工作模型：
* 词法作用域。
* 动态作用域。
JavaScript采用的是词法作用域。

### <div id="2.1">2.1 词法阶段</div>
* 标识符查找：从当前作用域，逐级向上，找到为止。
    * 引申：遮蔽效应(内部的标识符“遮蔽”了外部的标识符)
* 全局变量与全局对象的关系：全局变量会自动成为全局对象(比如浏览器中的 window 对象)的属性。

### <div id="2.2">2.2 欺骗词法</div>
欺骗词法即在运行时修改词法的作用域。
* 注意：不推荐进行欺骗词法。
    * 原因1：作用域会导致性能“下降”。引擎无法在编译时对作用域查找进行优化，因为引擎只能谨慎地认为这样的优化是无效的。
    * 原因2：会被严格模式所影响(限制)。`with`被完全禁止，而在保留核心功能的前提下，间接或非安全地使用`eval(..)`也被禁止了。

* 欺骗词法的两种方式：
    * eval函数：eval(..) 函数可以接受一个字符串为参数，并将其中的内容视为好像在书写时就存在于程序中这个位置的代码。
        * 注意：在严格模式的程序中，eval(..) 在运行时有其自己的词法作用域，意味着其 中的声明无法修改所在的作用域。
    * with关键字：重复引用同一个对象中的多个属性的快捷方式，可以不需要重复引用对象本身。
        * 注意：尽管`with`块可以将一个对象处理为词法作用域，但是这个块内部正常的`var`声明并不会被限制在这个块的作用域中，而是被添加到`with`所处的函数作用域中。使用let即可避免污染函数作用域。

## <div id="chapter3">第三章 函数作用域和块作用域</div>

### <div id="3.1">3.1 函数中的作用域</div>
* 函数作用域的概念：属于这个函数的全部变量都可以在整个函数的范围内使用及复用(事实上在嵌套的作用域中也可以使用)。

### <div id="3.2">3.2 隐藏内部实现</div>
* 为什么要隐藏内部实现？
    * 最小特权原则（最小授权原则、最小暴露原则）：软件设计中，应该最小限度地暴露必要内容，而将其他内容都“隐藏”起来，比如某个模块或对象的 API 设计。
    * 规避冲突。避免命名冲突，污染全局作用域。
* 隐藏内部实现的原理：将变量和函数用函数包裹起来，将其放置在函数作用域中。
* 隐藏内部实现的方法
    * 全局命名空间。在全局作用域中声明一个独特的变量，作为库的命名空间，所有需要暴露给外界的功能都是其属性。参考jquery（使用$符号），d3.js（使用d3）。
    * 模块管理。使用依赖管理器，将库的标识符显示地导入到特定的作用域中。
    * IIFE，代表立即执行函数表达式 (Immediately Invoked Function Expression)。（具名函数的 IIFE 是最佳实践）
* 函数表达式和函数声明的辨析
    * 函数表达式：函数被包含在一对 ( ) 括号内部，因此成为了一个表达式。
    * 函数声明：第一个单词必须是function。
    * 函数表达式可以是匿名的，而函数声明则不可以省略函数名。始终给函数表达式命名是一个最佳实践。
* IIFE的进阶用法
    * 把它们当作函数调用并传递参数进去。
    * 将一个参数命名为 undefined，但是在对应的位置不传入任何值，这样就可以 保证在代码块中 undefined 标识符的值真的是 undefined。
    * 倒置代码的运行顺序。

### <div id="3.3">3.3 块作用域</div>
* 形式上的块作用域，实际上并不是块作用域的写法，会导致变量污染到整个函数作用域。
```
    var foo = true;
    if (foo) {
        var bar = foo * 2;
        bar = something( bar ); console.log( bar );
    }
```
* 块作用域实现的几种方法
    * with。用 with 从对象中创建出的作用域仅在 with 声明中而非外 部作用域中有效。
    * try/catch。JavaScript 的 ES3 规范中规定 try/catch 的 catch 分句会创建一个块作用域，其中声明的变量（catch的实参？）仅在 catch 内部有效。如果在catch的代码块中使用var声明变量，在函数作用域中仍然能获取到声明的变量。
    * let。ES6引入的新关键字，实现块作用域的最好方式。
    * const。ES6引入的新关键字，同样可以实现块作用域，但变量的值不可以更改。
* 块作用域可以将无用的变量进行垃圾回收。

## <div id="chapter4">第四章 提升</div>
变量声明在作用域中出现的位置不同，会导致不同的影响。
* 提升的本质：代码的执行分为两个阶段：编译阶段和执行阶段。编译阶段编译器找到所有的声明，并用合适的作用域将它们关联起来。执行阶段引擎会直接执行代码。
* 提升的对象：只有声明本身会被提升（无论是变量声明还是函数声明），而赋值或其他运行逻辑会留在原地。
* 提升需要注意的点：
    * 即使是具名的函数表达式，名称标识符在赋值之前也无法在所在作用域中使用。
    * 函数会首先被提升，然后才是变量。
    * 函数会首先被提升，然后才是变量。
    * 尽量避免重复声明。

## <div id="chapter5">第五章 作用域闭包</div>
函数与对其状态即词法环境（lexical environment）的引用共同构成闭包（closure）。也就是说，闭包可以让你从内部函数访问外部函数作用域。在JavaScript，函数在每次创建时生成闭包。（from mdn）

### <div id="5.2">5.2 实质问题</div>
* 下面的这段代码清晰地展示了什么叫闭包：
```
    function foo() { 
        var a = 2;
        function bar() { 
            console.log( a );
        }
        
        return bar; 
    }    
    var baz = foo();
    baz(); // 2 —— 朋友，这就是闭包的效果。
```

### <div id="5.4">5.4 循环和闭包</div>
* 想要实现每隔一秒钟，按顺序打印出`1 2 3 4 5`。
    * 代码不能写成如下的形式，打印的结果会是每隔一秒钟输出一个6，原因是setTimeOut的回调函数是在循环结束了之后才执行。
    ```
    for (var i=1; i<=5; i++) { 
        setTimeout( function timer() {
        console.log( i ); }, i*1000 );
    }
    ```
    * 代码可以写成如下形式：
        * 第一种方式，通过IIFE，创建新的作用域，并在新的作用域中记录循环时的值：
        ```
        for (var i=1; i<=5; i++) { (function(j) {
            setTimeout( function timer() { console.log( j );
                }, j*1000 );
            })( i );
        }
        ```
        * 第二种方式，使用闭包的块作用域：
        ```
            for (let i=1; i<=5; i++) { 
                setTimeout( function timer() {
                console.log( i ); }, i*1000 );
            }
        ```
* 模块模式
```
    function CoolModule() {
        var something = "cool";
        var another = [1, 2, 3];
        
        function doSomething() { 
            console.log( something );
        }
        function doAnother() {
            console.log( another.join( " ! " ) );
        }
        
        return {
            doSomething: doSomething, 
            doAnother: doAnother
        }; 
    }
    var foo = CoolModule(); 
    foo.doSomething(); // cool
    foo.doAnother(); // 1 ! 2 ! 3
```
* 闭包经模块模式稍加改进后实现的单例模式：
```
    var foo = (function CoolModule() { var something = "cool";
        var another = [1, 2, 3];
        function doSomething() { 
            console.log( something );
        }
        function doAnother() {
            console.log( another.join( " ! " ) );
        }
        
        return {
            doSomething: doSomething, 
            doAnother: doAnother
        }; 
    })();
    foo.doSomething(); // cool 
    foo.doAnother(); // 1 ! 2 ! 3
```
### <div id="5.5">5.5 模块</div>
模块有两个主要特征:
1. 为创建内部作用域而调用了一个包装函数;
2. 包装函数的返回值必须至少包括一个对内部函数的引用，这样就会创建涵盖整个包装函数内部作用域的闭包。

### 5.5.1 现代的模块机制
* 一个模块管理器
```
    var MyModules = (function Manager() {
        var modules = {};
        
        function define(name, deps, impl) {
            for (var i=0; i<deps.length; i++) {
                deps[i] = modules[deps[i]];
            }
            modules[name] = impl.apply( impl, deps ); 
        }
        
        function get(name) { 
            return modules[name];
        }
        
        return {
            define: define,
            get: get 
        };
    })();
```

* 使用模块管理器来定义模块
```
    MyModules.define( "bar", [], function() { 
        function hello(who) {
            return "Let me introduce: " + who; 
        }
        
        return {
            hello: hello
        }; 
    });

    MyModules.define( "foo", ["bar"], function(bar) { 
        var hungry = "hippo";
        function awesome() {
            console.log( bar.hello( hungry ).toUpperCase() );
        }
        
        return {
            awesome: awesome
        }; 
    });
    
    var bar = MyModules.get( "bar" );
    var foo = MyModules.get( "foo" ); 
    console.log( bar.hello( "hippo" )); // Let me introduce: hippo 
    foo.awesome(); // LET ME INTRODUCE: HIPPO
```



### 5.5.2 未来的模块机制
* bar.js
```
    function hello(who) {
        return "Let me introduce: " + who;
    }

    export hello;
```
* foo.js
```
    // 仅从 "bar" 模块导入 hello() import hello from "bar";
    var hungry = "hippo";
    function awesome() { 
        sconsole.log(hello( hungry ).toUpperCase() );
    }
    
    export awesome;
```
* baz.js,导入完整的 "foo" 和 "bar" 模块
```
    module foo from "foo"; 
    module bar from "bar";
    
    console.log(
        bar.hello( "rhino" )
    ); // Let me introduce: rhino 
    
    foo.awesome(); // LET ME INTRODUCE: HIPPO
```
* import 可以将一个模块中的一个或多个 API 导入到当前作用域中，并分别绑定在一个变量 上(在我们的例子里是 hello)。
* module 会将整个模块的 API 导入并绑定到一个变量上(在 我们的例子里是 foo 和 bar)。
* export 会将当前模块的一个标识符(变量、函数)导出为公 共 API。
* 这些操作可以在模块定义中根据需要使用任意多次。

## <div id="appendixA">附录A 动态作用域</div>
动态作用域和词法作用域的区别如下：
* 词法作用域，关心函数是在何处声明以及如何声明。
* 词法作用域的定义过程发生在书写代码的阶段。
* 动态作用域，关心函数是如何调用的、从何处调用的。
* 动态作用域在运行时才被动态定义。

书中通过举例，比较了动态作用域和静态作用域之间的区别。

动态作用域的代码调用：
```
function foo() {
    console.log( a ); // 3(不是 2 !)
}

function bar() { 
    var a = 3;
    foo(); 
}

var a = 2;
bar();
```

静态作用域的代码调用：
```
function foo() { 
    console.log( a ); // 2
}
function bar() { 
    var a = 3;
    foo(); 
}

var a = 2; 
bar();
```

## <div id="appendixB">附录B 块作用域的替代方案</div>
### <div id="B.1">B.1 Traceur</div>
块作用域的替代方案：
* try catch。代码丑陋，但这是一种使不兼容es6的代码使用块作用域的一个手段。通常在代码中按照es6的块作用域方法去写，再由特定的工具（比如traceur）对块作用域转换为类似形式。

### <div id="B.2">B.2 隐式和显式作用域</div>
在第三章曾经提到过：

> 用 let 将变量附加在一个已经存在的块作用域上的行为是隐式的。在开发和修改代码的过 程中，如果没有密切关注哪些块作用域中有绑定的变量，并且习惯性地移动这些块或者将 其包含在其他的块中，就会导致代码变得混乱。

显式作用域的好处：
* 显式作用域，作用域更加突出，
* 显式作用域，在代码重构时表现得更加健壮。

在第三章中提出的由隐式作用域变为显式作用域的解决方案是在隐式作用域的周围添加大括号。还有一种方案，通过let声明，也可以显式地表明一块作用域，代码如下：
```
let (a = 2) {
    console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

但是**注意：let 声明并不包含在 ES6 中。**

使用es6语法的方案，方案一，仍然使用大括号。
```
/*let*/ { 
    let a = 2; 
    console.log( a );
}

console.log( a ); // ReferenceError

```

方案二，通过工具，把自己编写的let声明，转换为es6语法，即使用大括号围住显式作用域。

（感觉有点太麻烦了，还不如直接用隐式的作用域或者在不明显的地方用大括号。）

### <div id="B.3">B.3 性能</div>
* try catch的性能很差。
* 什么时候使用块作用域？
    * 你是否想要块作用域?如果你想要，上述的内容可以帮助你。如果不想要，继续使用 var 来写代码就好了!

### <div id="appendixC">附录C this词法</div>
* 箭头函数不仅仅意味着少些代码。
* 不建议箭头函数和词法作用域混用。
* 建议在使用this的地方使用bind。（bind() 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。 from mdn）。