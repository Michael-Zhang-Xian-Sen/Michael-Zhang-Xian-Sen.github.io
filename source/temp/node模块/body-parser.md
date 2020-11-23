Node.js body parsing middleware.

Node.js请求体解析中间件。

在处理器进行处理之前，在一个中间件中对请求体进行解析，请求的内容可以通过`req.body`属性获得。

注意：`req.body`的具体数据结构是由用户输入决定的。这个对象的所有属性和值都是不可信的，应该在使用前对属性是否存在进行检验。例如，`req.body.foo.toString()`方法在多种情况下可能会失败。例如`req.body.foo`属性不存在，或者不是一个字符串类型，亦或是`toString`不是一个函数。
