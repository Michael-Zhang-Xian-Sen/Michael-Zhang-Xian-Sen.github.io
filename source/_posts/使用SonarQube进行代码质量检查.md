---
title:  使用SonarQube进行代码质量检查 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-04-09 11:35:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 工具 #分类

tags: [代码质量检查, SonarQube]  #文章标签，可空，多标签请用格式，注意:后面有个空格

description: 使用SonarQube进行代码质量检查
---

SonarQube的安装、插件配置及个人的一些感受

<!-- more -->

## 目录
1. [安装SonarQube](#1)
2. [配置相关插件](#2)
    1. [汉化插件](#2.1)
    2. [导出代码质量检测报告插件](#2.2)
3. [其他SonarQube相关操作](#3)
    1. [查看当前SonarQube版本](#3.1)

## <span id="1">1. 安装SonarQube</span>
1. 前往SonarQube官网进行下载（推荐下载Community版本，免费，功能够用）：https://www.sonarqube.org/downloads/
2. 下载完成后解压该文件，此处最好不要使用root用户进行解压。该步骤在官网文档的说明十分详细：https://docs.sonarqube.org/latest/setup/get-started-2-minutes/ 
3. 解压后在命令行下进入解压后的文件夹，然后进入`bin`目录，该文件夹包含了不同平台下的脚本文件。如：
    ```
    jsw-license		macosx-universal-64
    linux-x86-64		windows-x86-64
    ```
4. 根据自己的平台cd到相应文件夹下（博主是macos），然后输入`sonar.sh start`运行SonarQube。
5. 进入SonarQube客户端界面：http://localhost:9000 

## <span id="2">2. 配置相关插件</span>

### <span id="2.1">2.1 汉化插件</span>
1. 下载汉化插件。一定要下载相应版本，否则可能会无法启动SonarQube：https://github.com/SonarQubeCommunity/sonar-l10n-zh
2. 将下载后的插件移动到`sonarQube根目录/extensions/plugins`
3. 重启SonarQube

### <span id="2.2">导出代码质量检测报告插件</span>
1. 使用该仓库的python程序导出：https://github.com/ximone/Sonar_Report_Generator

## <span id="3">3. 其他SonarQube相关操作</span>
### <span id="3.1">1. 查看当前SonarQube的版本</span>
在SonarQube的客户端界面选择Administration->System

## 个人感受
使用不多，课程要求才进行了初步尝试。
个人感觉该软件对一个软件开发团队而言意义更大一些。
1. 可以更好地帮助一个软件开发团队维护代码，使得代码编写符合内部的编码规范。
2. 可以方便地review代码。