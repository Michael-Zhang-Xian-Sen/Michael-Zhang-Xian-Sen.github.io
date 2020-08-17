---
title: ES6学习笔记（二）：字符串的扩展 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-04-29 14:03:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [前端, es6] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: es6：字符串扩展内容的总结。
---

## normalize()方法
参数如下，注意参数必须大写！
* NFC(default)。标准等价合成（Normalization Form Canonical Composition）。先规范分解，然后进行规范组合。
* NFD。标准等价分解（Normalization Form Canonical Decomposition）。规范分解。
* NFKC。兼容等价合成（Normalization Form Compatibility Composition）。先兼容性分解，然后进行规范组合。
* NFKD。兼容等价分解（Normalization Form Compatibility Decomposition）。兼容性分解。

个人理解，针对字母后跟音标符号的组合，如`\u004F\u030C`，与一个标有音标符合的字符`\u01D1`，属于**规范等价**，因此可以用NFC和NFD。
兼容等价（NFKC和NFKD）包含了规范等价，但比规范等价额外包含了**兼容**的部分，是规范等价的超集。若两个字符完全相同的组合，如`\u0066\u0066`，与他们的单个字符表现形式`\uFB00`，他们的语义相同，则二者为兼容等价。可以使用规范化将二者变为同一形式。

### 示例：
1. NFC和NFD比较
```javascript
var str1 = '\u01D1' // Ǒ
var str2 = '\u004F\u030C' // Ǒ
console.log(`str1的原始值（即赋予str1的值）为${String.raw`\u01D1`}，显示的值为${str1},长度为${str1.length}`);  // Ǒ
console.log(`str2的原始值（即赋予str2的值）为${String.raw`\u004F\u030C`}，显示的值为${str2},长度为${str2.length}`);  // Ǒ
console.log(`str1与str2是否相等？${str1 === str2}`);

var params = [{
    param:'NFD',
    beNorm:'str1',
    beNormStr:str1,
    beCompared:'str2',
    beComparedStr:str2
},{
    param:'NFC',
    beNorm:'str2',
    beNormStr:str2,
    beCompared:'str1',
    beComparedStr:str1
},{
    param:'NFKD',
    beNorm:'str1',
    beNormStr:str1,
    beCompared:'str2',
    beComparedStr:str2
},{
    param:'NFKC',
    beNorm:'str2',
    beNormStr:str2,
    beCompared:'str1',
    beComparedStr:str1
}]

params.forEach((cur,idx)=>{
    console.log(`${idx+1}. 对${cur.beNorm}进行${cur.param}规范化`);
    console.log(`规范化后的${cur.beNorm}与${cur.beCompared}是否相等？${cur.beNormStr.normalize(cur.param) === cur.beComparedStr}`);
    console.log(`规范化前后${cur.beNorm}的值分别为：${cur.beNormStr},${cur.beNormStr.normalize(cur.param)}`)
    console.log(`规范化前后${cur.beNorm}的长度分别为：${cur.beNormStr.length},${cur.beNormStr.normalize(cur.param).length}`)
})
```

2. NFKC和NFKD比较
```javascript
var str1 = '\uFB00' // Ǒ
var str2 = '\u0066\u0066' // Ǒ
console.log(`str1的原始值（即赋予str1的值）为${String.raw`\u01D1`}，显示的值为${str1},长度为${str1.length}`);  // Ǒ
console.log(`str2的原始值（即赋予str2的值）为${String.raw`\u004F\u030C`}，显示的值为${str2},长度为${str2.length}`);  // Ǒ
console.log(`str1与str2是否相等？${str1 === str2}`);

var params = [{
    param:'NFD',
    beNorm:'str1',
    beNormStr:str1,
    beCompared:'str2',
    beComparedStr:str2
},{
    param:'NFC',
    beNorm:'str2',
    beNormStr:str2,
    beCompared:'str1',
    beComparedStr:str1
},{
    param:'NFKD',
    beNorm:'str1',
    beNormStr:str1,
    beCompared:'str2',
    beComparedStr:str2
},{
    param:'NFKC',
    beNorm:'str2',
    beNormStr:str2,
    beCompared:'str1',
    beComparedStr:str1
},{
    param:'NFKC',
    beNorm:'str1',
    beNormStr:str1,
    beCompared:'str2',
    beComparedStr:str2
}]

params.forEach((cur,idx)=>{
    console.log(`${idx+1}. 对${cur.beNorm}进行${cur.param}规范化`);
    console.log(`规范化后的${cur.beNorm}与${cur.beCompared}是否相等？${cur.beNormStr.normalize(cur.param) === cur.beComparedStr}`);
    console.log(`规范化前后${cur.beNorm}的值分别为：${cur.beNormStr},${cur.beNormStr.normalize(cur.param)}`)
    console.log(`规范化前后${cur.beNorm}的长度分别为：${cur.beNormStr.length},${cur.beNormStr.normalize(cur.param).length}`)
})
```
第四次比较中为什么str2没有进行组合？暂时还未解决该疑惑。
