---
title:  vue-cli配置代理进行跨域 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-04-09 11:35:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 前端 #分类

tags: [vue, 前端]  #文章标签，可空，多标签请用格式，注意:后面有个空格

description: vue-cli配置代理
---

记录通过vue-cli配置代理进行跨域的实践及感悟。

<!-- more -->

### 实践：通过vue-cli配置代理进行跨域。
1. 在项目根目录添加vue.config.js文件（如果已经有则跳过）。
2. 添加如下代码：
```javascript
module.exports = {
  devServer: {  // 该配置仅针对开发模式有效
    proxy: {    // 该对象下的内容为代理配置
      '/api': { // 代理具有'\api'前缀的请求
        target: 'http://localhost:8080/video_war/', // 将匹配的前缀（本示例中为'api'）改为target属性的内容。
        changeOrigin: true, // 
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
}
```