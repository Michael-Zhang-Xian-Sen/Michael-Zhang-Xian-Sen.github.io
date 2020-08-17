---
title:  版本管理工具：svn小记 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-8-3 21:35:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 工具 #分类

tags: [svn, 版本管理工具]  #文章标签，可空，多标签请用格式，注意:后面有个空格

description: svn小记
---

## mac使用svn(Subvision)
mac自带svn，在控制台可直接使用，无需下载。

## 常用命令
* 下载项目：`svn checkout url(svn项目全路径) project_dir(本地项目全路径) --username=用户名 --password=密码`
* 查看最近5条svn log日志：`svn log -l 5`
该项目已遗失 (被非 svn 命令所删除) 或是不完整
* svn status：执行SVN up和svn merge等命令出现在首位置的各字母含义如下：
    * “ ” 无修改
    * “A” 新增
    * “C” 冲突
    * “D” 删除
    * “G” 合并
    * “I” 忽略
    * “M” 改变
    * “R” 替换
    * “X” 未纳入版本控制，但被外部定义所用
    * “?” 未纳入版本控制
    * “!” 该项目已遗失 (被非 svn 命令所删除) 或是不完整
    * “~” 版本控制下的项目与其它类型的项目重名
    * L abc.c # svn已经在.svn目录锁定了abc.c

### svn update
* A  已添加
* D  已删除
* U  已更新
* C  合并冲突
* G  合并成功
* E  已存在

## 设置忽略文件
### 全局设置忽略文件
找到svn的全局配置文件：`~/.subversion/config`，将`[miscellany]`段中`global-ignores`前的注释符号去掉即可。还可增加一些自己想要忽略的文件类型。

此处推荐添加的一些额外忽略文件：
```
# Editor directories and files
.idea .vscode *.iml *.suo
*.ntvs* *.njsproj *.sln *.sw?
```

### 工程目录下设置忽略文件和目录
使用`svn propedit svn:ignore <dir>`命令。

## 相关资料
* 官网文档：https://subversion.apache.org/docs/
* svnbook：http://svnbook.red-bean.com/