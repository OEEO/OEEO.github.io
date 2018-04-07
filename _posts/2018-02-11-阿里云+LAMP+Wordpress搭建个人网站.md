---
layout: post
title:  "阿里云+LAMP+Wordpress 搭建个人网站"
date:   2018-02-11
categories: Linux
tags: Linux wordpress
---

## 服务器的购买及域名备案
### 1、购买服务器。
我购买的是阿里云的 ECS，所以就以阿里云为例了。直接上阿里云官网，选择自己想要的服务器配置购买即可，第一次买挺便宜的。
### 2、注册域名。
上万网，注册一个域名，选定域名后，需要填写一些相关信息，这时候需要进入阿里云的控制台，填写一个域名的信息模板，进行实名认证。
### 3、域名信息模板实名认证。
信息模板实名认证后，三个工作日后，才可以进行域名备案，我备案的时候不知道，就被驳回了，工作人员说是要等三个工作日。
### 4、域名备案。
我在网上搜到的都说备案挺麻烦的。但事实上，现在域名备案简单了很多，不用邮寄资料了，也不需要幕布，直接在阿里云的控制台上，点击右上角菜单栏中的备案=》ICP 备案系统，按步骤操作即可。整个流程相当简单。按照上面的提示操作即可。整个备案过程，我用了 12 天左右，大家可以参考一下。
注：信息模板的实名认证跟域名备案是两件事。
## LAMP环境的搭建
### LAMP就是 Liunx + Apache + MySql(Mariadb) + PHP。其中 Mariadb 是 MySql 的一个分支，CentOS7 后的系统都默认用的 Mariadb。

环境搭建时，可以选择编译案装，也可以选择云安装。
编译案装复杂一点，自由度高，可以安装最新的版本。
云安装一步到位，只能安装云上面及相关社区上有的资源。
下面我就介绍一下简单的云安装。

在开始案装前先要下载一个 Xshell 工具，用于远程连接服务器。
连接到服务器后

### 1、更新 yum 资源
`yum update -y`

### 2、更新 yum 资源相关社区的云资源
`yum install -y epel-release`

`rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`

### 3、yum 安装 php7.0 及相关依赖
`
yum install -y php70w.x86_64 php70w-cli.x86_64 php70w-common.x86_64 php70w-gd.x86_64 php70w-ldap.x86_64 php70w-mbstring.x86_64 php70w-mcrypt.x86_64 php70w-mysql.x86_64 php70w-pdo.x86_64
`
### 4、编辑 mariaDB 新版的 yum 资源地址
用vi命令在 /etc/yum.repos.d/ 下创建一个名为 MariaDB.repo 的文件

`vi /etc/yum.repos.d/MariaDB.repo`

文件填入以下内容
```html
# MariaDB 10.2 CentOS repository list - created 2017-10-01 08:48 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.2/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```
### 5、安装 MariaDB、启动服务、设置开机自启

`yum install -y mariadb-server mariadb`
`systemctl start mariadb.service`
`systemctl enable mariadb.service`

### 6、设置数据库根帐户密码
`mysql_secure_installation`

### 7、安装Apache Web服务器
`yum install -y httpd`

`systemctl start httpd.service`

`systemctl enable httpd.service`

### 到这里 LAMP 环境就搭建完毕了。输入以下命令查看版本。
`php -v` 查看 php 的版本。
`mysql -V` 这里是大写的 V 。查看 mariadb 的版本。
`httpd -v` 查看 apache 的版本。

## Wordpress 的安装
### 1、在数据库中创建 wordpress 的关联数据库实例
wordpress 是需要与数据库连接的。所以安装 Wordpress前 要先在数据库下给它创建一个数据库实例。步骤如下：
输入如下命令，`enter` ，然后输入安装数据库时设置的密码，进入到数据库

`mysql -uroot -p`

输下如下命令，创建名为 wordpress 的数据库实例。（不要漏掉最后的分号）

`create database wordpress;`

输入如下命令，查看数据库的实例列表，看是否创建成功。

`show  databases;`

### 2、Linux 中下载并解压 wordpress 到网站根目录。
先上 wordpress 官网上，查看最后的安装包，右键复制连接地址。
然后 Xshell 连接到服务器。
然后 cd 到网站的根目录下（在 `/etc/httpd/conf/httpd.conf` 中设置网站根目录路径）
通过如下命令，下载 wordpress。

`wget https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz`

通过如下命令，解压

`tar -zxvf wordpress-4.9.4-zh_CN.tar.gz`

例如：我的网站根目录设置的是 /www/wordpress，我就进入到 /www 目录下载 wordpress ，解压，然后再把压缩包删掉，就完成了。
### 3、授权 apache，wordpress 目录及其下文件的修改权限。 
这一步非常重要，否则后面的步骤会不能写入，以及装插件，主题，需要 ftp 连接。
cd到 wordpress 所在的目录，输入如下命令，把 wordpress 文件夹及其下所有文件的所有者设置为 apache。

`chown -R apache wordpress/`

### 4、安装 wordpress 并连接到数据库
完成以上步骤后，在网页中输入你的网站 ip 或域名。
会出现安装 wordpress 的提示，按照步骤填写，安装即可。
### 5、插件
wordpress 下有很多优秀的插件可以用。这里我推荐一个 WP Editor.md，一个支持 markdown 以及代码高亮的插件。 
