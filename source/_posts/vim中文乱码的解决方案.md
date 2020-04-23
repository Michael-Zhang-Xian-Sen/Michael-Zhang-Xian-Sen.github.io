---
title: vim中文乱码的解决方案
date: 2017-10-27 15:11:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 工具 #分类
tags: [工具, vim] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: vim中文乱码解决方案。
thumbnail: http://ewinds.pw/vim2.jpg
---

「封面图：VIM图标」

<!-- more -->

### 解决方案	

出现上述现象是编码出了问题。

1. 执行`sudo vim ~/.vimrc`
2. 输入

    set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
    set termencoding=utf-8
    set encoding=utf-8

成功解决乱码。
---

### 原理

#### 为什么要改变vimrc文件

`vimrc`文件用于初始化vim。使用vim时，vim会到指定目录下寻找vimrc并用其进行初始化。更多的内容见`:help vimrc`，通过`version`，可以看到vim规定的vimrc文件路径。例如我的：

    system vimrc file: "$VIM/vimrc"
    user vimrc file: "$HOME/.vimrc"
    2nd user vimrc file: "~/.vim/vimrc"
    user exrc file: "$HOME/.exrc"
    fall-back for $VIM: "/usr/share/vim"
    
#### 编辑内容的含义是什么？

（1） 磁盘文件的字符编码 
存放在磁盘上的文本文件，是按照一定的字符编码进行保存的，不同的文件可能使用了不同的字符编码。 
这在VIM中被叫做：fileencoding。

（2） VIM缓冲区以及界面的字符编码 
VIM运行时，其菜单、标签、以及各个缓冲区统一使用一种字符编码方式。 
这在VIM中被叫做：encoding。

（3) 终端使用的字符编码 
终端同一时刻只能使用一种字符编码，并按照这种编码从接收到的字节流中识别字符，并显示，终端的字符编码是可以动态调整的。 
这在VIM中被叫做：termencoding。

可以看出，VIM涉及到的3种字符编码之间的转换： 
读：fileencoding—–> encoding 
显：encoding ——> termencoding 
写：encoding ——-> fileencoding

#### 那些字符编码

UTF-8（8-bit Unicode Transformation Format）：是一种针对Unicode的可变长度字符编码，又称万国码。 支持中文。

UCS（通用字符集）：包含了用于表达所有已知语言的字符，保证了与其他字符集的双向兼容， 是所有其他字符集标准的一个超集。支持中文。Unicode规范中推荐的标记字节顺序的方法是BOM。BOM是Byte Order Mark。在UCS编码中有一个叫做"ZERO WIDTH NO-BREAK SPACE"的字符，它的编码是FEFF。而FFFE在UCS中是不存在的字符，所以不应该出现在实际传输中。UCS规范建议我们在传输字节流前，先传输字符"ZERO WIDTH NO-BREAK SPACE"。这样如果接收者收到FEFF，就表明这个字节流是Big-Endian的；如果收到FFFE，就表明这个字节流是Little-Endian的。因此字符"ZERO WIDTH NO-BREAK SPACE"又被称作BOM。

gb18030、gbk、gb2312肯定是支持中文的编码。
  
CP936其实就是GBK，IBM在发明Code Page的时候将GBK放在第936页，所以叫CP936。

参考资料： http://blog.csdn.net/smstong/article/details/51279810
参考资料： http://www.fmddlmyy.cn/text6.html
参考资料： https://www.zhihu.com/question/35609295
