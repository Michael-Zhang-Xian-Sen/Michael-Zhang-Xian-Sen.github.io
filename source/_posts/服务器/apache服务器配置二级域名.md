---
title: apache2.4 配置二级域名 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-11-21 20:23:50 #文章生成时间，一般不改，当然也可以任意修改
categories: apache #分类
tags: [二级域名] #文章标签，可空，多标签请用格式，注意:后面有个空格
---

1. 添加DNS解析。如若添加phpmyadmin作为二级域名，则添加一条A记录，其中主机记录为phpmyadmin.yourwebsite.com，记录值为主机所在的IP地址。
2. 编辑apache的站点配置文件。若为apache2.4版本，编辑`/etc/apache2/sites-available/000-default.conf`，编辑内容如下所示：
    ```conf
        # www子域名，指向个人博客
        <VirtualHost *:80>
            DocumentRoot /var/www/blog
            ServerName www.nice2meetu.site
            ServerAdmin 1920395012@qq.com
            ErrorLog ${APACHE_LOG_DIR}/blog/error.log
            CustomLog ${APACHE_LOG_DIR}/blog/access.log combined
            <Directory /var/www/blog>
                    AllowOverride All
                    Require all granted
                    DirectoryIndex index.html
            </Directory>
        </VirtualHost>
        # phpmyadmin子域名，指向phpmyadmin
        <VirtualHost *:80>
            DocumentRoot /var/www/phpmyadmin
            ServerName phpmyadmin.nice2meetu.site
            ServerAdmin 1920395012@qq.com
            ErrorLog ${APACHE_LOG_DIR}/phpmyadmin/error.log
            CustomLog ${APACHE_LOG_DIR}/phpmyadmin/access.log combined
            <Directory /var/www/phpmyadmin>
                    Options FollowSymLinks
                    AllowOverride All
                    Require all granted
                    DirectoryIndex index.html index.php
            </Directory>
        </VirtualHost>
    ```
3. 重新启动apache服务器：`/etc/init.d/apache2 restart`
4. 此时二级域名与对应的网站便成功绑定。
