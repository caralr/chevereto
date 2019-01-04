在 VPS 上搭建自己的图床，这次我们使用 chevereto 这个程序。你需要在服务器上有 Nginx、PHP、MySQL，我们使用一键安装包进行安装。

## 安装 LNMP

```bash
yum -y install wget screen curl python git
wget http://mirrors.linuxeye.com/lnmp-full.tar.gz
tar xzf lnmp-full.tar.gz
cd lnmp
screen -S lnmp
./install.sh
```

这个过程比较漫长，可参考视频中的选项进行配置，安装成功后记录你的配置文件，如：

```
Nginx install dir:              /usr/local/nginx

Database install dir:           /usr/local/mysql
Database data dir:              /data/mysql
Database user:                  root
Database password:              HKYsdhHG

PHP install dir:                /usr/local/php

phpMyAdmin dir:                 /data/wwwroot/default/phpMyAdmin
phpMyAdmin Control Panel URL:   http://45.63.59.2/phpMyAdmin

Index URL:                      http://45.63.59.2/
```

## 配置虚拟主机

```bash
cd lnmp
./vhost.sh
```

根据视频中操作添加虚拟主机，如果没有域名的话就不用添加了！！

配置好虚拟主机后记录下你的站点配置，如：
 
```bash
Your domain:                  tc.2333.blog
Virtualhost conf:             /usr/local/nginx/conf/vhost/tc.2333.blog.conf
Directory of:                 /data/wwwroot/tc.2333.blog
Let's Encrypt SSL Certificate:/usr/local/nginx/conf/ssl/tc.2333.blog.crt
SSL Private Key:              /usr/local/nginx/conf/ssl/tc.2333.blog.key
```

## 安装 Chevereto

下载源码（记得安装 git 哦）

```bash
git clone https://github.com/Chevereto/Chevereto-Free && mv Chevereto-Free/* ./ && rm -rf Chevereto-Free
```

给 `www` 用户授权

```bash
chown -R www:www /data/wwwroot/tc.2333.blog
```

修改 `nginx` 配置（`/usr/local/nginx/conf/vhost/你的站点.conf`），添加如下配置：

```conf
location / {
  try_files $uri $uri/ /index.php?$query_string;
}
```

让配置文件生效

```bash
service nginx reload
```

创建数据库 `chevereto`

然后就开始安装了，安装视频操作即可！
