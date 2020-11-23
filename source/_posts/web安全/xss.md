---
title: web安全：xss攻击 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-09-02 21:35:30 #文章生成时间，一般不改，当然也可以任意修改
categories: web安全 #分类
tags: web安全 #文章标签，可空，多标签请用格式，注意:后面有个空格
---

# xss（Cross-Site Scripting）攻击
XSS 的本质是：恶意代码未经过滤，与网站正常的代码混在一起；浏览器无法分辨哪些脚本是可信的，导致恶意脚本被执行。

### 不可信内容
* 来自用户的 UGC（User Generated Content） 信息
* 来自第三方的链接
* URL 参数
* POST 参数
* Referer （可能来自不可信的来源）
* Cookie （可能来自其他子域注入）

### XSS 注入的方法
1. 在 HTML 中内嵌的文本中，恶意内容以 script 标签形成注入。
2. 在内联的 JavaScript 中，拼接的数据突破了原本的限制（字符串，变量，方法名等）。
3. 在标签属性中，恶意内容包含引号，从而突破属性值的限制，注入其他属性或者标签。
4. 在标签的 href、src 等属性中，包含 javascript: 等可执行代码。
5. 在 onload、onerror、onclick 等事件中，注入不受控制代码。
6. 在 style 属性和标签中，包含类似 background-image:url("javascript:..."); 的代码（新版本浏览器已经可以防范）。
7. 在 style 属性和标签中，包含类似 expression(...) 的 CSS 表达式代码（新版本浏览器已经可以防范）。
8. 总之，如果开发者没有将用户输入的文本进行合适的过滤，就贸然插入到 HTML 中，这很容易造成注入漏洞。攻击者可以利用漏洞，构造出恶意的代码指令，进而利用恶意代码危害数据安全。

### XSS攻击的两大要素：
1. 攻击者提交恶意代码。
2. 浏览器执行恶意代码。

### 前端应对方案：
1. 输入过滤。并不是终极方案，且不是特别好。
2. 纯前端渲染。
3. 避免将不可信的数据传递给以下能把字符串作为代码执行的api：
    1. DOM 中的内联事件监听器，如 location、onclick、onerror、onload、onmouseover 等。
    2. <a> 标签的 href 属性。
    3. JavaScript 的 eval()、setTimeout()、setInterval() 等
4. 严格进行CSP防范。
5. 输入内容长度控制。
6. HTTP-only Cookie: 禁止 JavaScript 读取某些敏感 Cookie，攻击者完成 XSS 注入后也无法窃取此 Cookie。
7. 验证码：防止脚本冒充用户提交危险操作。
7. 避免拼接 HTML。

### 如何减少xss漏洞
1. 利用模板引擎。
2. 避免内联事件。
3. 避免拼接 HTML。
4. 时刻保持警惕。
5. 增加攻击难度，降低攻击后果。
6. 主动检测和发现。
