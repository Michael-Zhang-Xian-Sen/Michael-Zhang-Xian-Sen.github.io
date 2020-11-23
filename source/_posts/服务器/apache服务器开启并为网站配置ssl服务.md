---
title: apache2.4 开启并为网站配置ssl服务 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-11-23 17:32:50 #文章生成时间，一般不改，当然也可以任意修改
categories: apache #分类
tags: [ssl] #文章标签，可空，多标签请用格式，注意:后面有个空格
---
大致流程为：先申请SSL证书并上传到服务器，然后在服务器配置ssl站点信息，最后启用ssl站点和ssl服务就可以了。

### 1. 申请SSL证书，并将证书文件上传到服务器
1. 申请/购买SSL证书。博主的域名是在阿里云注册的，于是直接在阿里注册了SSL证书。在购买时选择单个域名、DV域名级SSL，有免费的证书。后续可能要绑定域名等。
2. 下载SSL证书文件。我们需要以下三种文件：
    * 证书文件：以`.crt`为后缀或文件类型。证书不仅是证书，还有公钥存在其中，使用命令`openssl x509 -in xxxx.crt -pubkey`可以查看公钥。
    * 证书链文件：以`.crt`为后缀或文件类型。名称中会包含`chain`。证书链是一系列CA证书发出的证书序列，最终以根CA证书结束。web浏览器内置有一组浏览器自动信任的根CA证书，其他证书授权机构的证书必须附带证书链。
    * 密钥文件：以`.key`为后缀或文件类型。该秘钥为私钥，用来对经过公钥加密后的密文进行解密。
3. 将下载后的文件上传至服务器。博主保存在了`/etc/apache2/cert/`。

### 2. 配置ssl站点。
apache2有一个默认的ssl站点配置文件，但并没有启用，我们可以直接编辑该文件。
1. 进入到可用站点目录`cd /etc/apache2/sites-available/`。
2. 编辑默认ssl站点配置文件`vim ./default-ssl.conf`。
3. 将默认的三种文件路径注释掉，替换为我们自己的文件。
    ```conf
        # SSLCertificateFile     /etc/ssl/certs/ssl-cert-snakeoil.pem
        # SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key 
        # SSLCertificateChainFile /etc/apache2/cert/4510898_www.nice2meetu.site_chain.crt
        SSLCertificateFile /etc/apache2/cert/xxxxx_www.xxxxxx.xxxx_public.crt
        SSLCertificateKeyFile /etc/apache2/cert/xxxxx_www.xxxxxx.xxxx.key
        SSLCertificateChainFile /etc/apache2/cert/xxxxx_www.xxxx.xxxxx_chain.crt
    ```
4. 开启SSL引擎。设置`SSLEnigne on`（应该是默认设置）。
5. 设置网站的根目录。修改`DocumentRoot /var/www/blog`。即url中域名及端口后的路径，在我们服务器进行映射时的根目录。如：`https://www.xxxx.xxx/a/b/c/d`，用户实际访问的便是服务器上的`/var/www/blog/a/b/c/d`。
6. 添加网站管理员邮箱（可选的修改）。修改`ServerAdmin 1920395012@qq.com`。该邮箱会在网站出问题时显示在网页上，方便访问者联系网站管理员。

### 3. 启用apache2 ssl站点和服务
apache2.4默认是没有开启ssl配置的，需要我们手动打开。
1. 开启ssl模块。在服务器上，输入并执行`a2enmod ssl`。（执行命令后会提示，浏览`/usr/share/doc/apache2/README.Debian.gz`获取如何配置SSL的教程，想要深入了解可以看一下。）
2. 使用默认的SSL站点。输入并执行`a2ensite default ssl`
3. 重新启动apache2服务`/etc/init.d/apache2 restart`

至此，我们便可以使用https访问站点啦。