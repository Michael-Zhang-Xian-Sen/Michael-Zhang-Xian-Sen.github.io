---
title: 搭建mysql环境（mac版本） #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-03-15 09:24:30 #文章生成时间，一般不改，当然也可以任意修改

categories: mysql #分类

tags: [环境搭建, mysql] #文章标签，可空，多标签请用格式，注意:后面有个空格

description: 搭建mysql环境（mac版本）

---

简要地记录了mac系统搭建mysql数据库的过程。

<!-- more -->

## 从官网下载安装包并进行安装
1. [官网](https://dev.mysql.com/downloads/mysql/)根据期望安装的版本号下载mysql。建议下载dmg包。
2. 下载完成后，根据提示步骤安装。
    * 注意：mysql可能会生成默认密码，生成时有相应提示，请把默认密码记录下来。
3. 安装完毕，重启mac，可以发现系统偏好设置中添加了mysql的图标。
4. 启动mysql服务后，在控制台输入`ps aux | grep mysql`，查找到mysql的运行路径。
5. 根据运行路径找到mysql文件夹，并找到mysql程序的`bin`目录（即可运行的二进制文件目录）。笔者的路径为：`/usr/local/mysql/bin`。
7. 添加mysql的路径至环境变量：`vim ~/.bash_profile`，添加如下内容：
    ```
        export MYSQL_HOME=/usr/local/mysql
        export PATH=$PATH:$MAVEN_HOME/bin:$MYSQL_HOME/bin
    ```
    其中 `export MYSQL_HOME=/usr/local/mysql`和`export PATH=$PATH:$MYSQL_HOME/bin`是重点。`$MAVEN_HOME/bin:`可以忽略，这个仅仅是笔者电脑中又添加了MAVEN的环境变量。
8. 运行`mysql -u root -p`，输入密码，登陆mysql，大功告成。

## 推荐工具

### 图形化客户端

Navicat Premium for Mac