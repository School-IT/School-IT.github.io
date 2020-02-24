# 汕头侨中School IT 服务器初始化文档

> Author: Rd

# （一）nvm 安装 nodejs

* 下载 nvm

```
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash
```
* 运行

```
$ source ~/.bashrc
```

* 安装node

```
$ nvm install stable #安装最新稳定版
```

* 查看当前 nodejs 版本


```
$ node -v
```

![](http://ooad0wrk3.bkt.clouddn.com/14993330866333.jpg)

* 额外说明

```
nvm install stable #安装最新稳定版 node 
nvm install 4.2.2 #安装 4.2.2 版本 
nvm install 0.12.7 #安装 0.12.7 版本
```

```
nvm use 0 #切换至 0.12.7 版本
npm install -g mz-fis #安装 mz-fis 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v0.12.7/lib/mz-fis
nvm use 4 #切换至 4.2.2 版本
npm install -g react-native-cli #安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v4.2.2/lib/react-native-cli
```

* nvm切换版本

如果你的默认 node 版本（通过 nvm alias 命令设置的）与项目所需的版本不同，则可在项目根目录或其任意父级目录中创建 .nvmrc 文件，在文件中指定使用的 node 版本号，例如：

```
cd <项目根目录>  #进入项目根目录
echo 4 > .nvmrc #添加 .nvmrc 文件
nvm use #无需指定版本号，会自动使用 .nvmrc 文件中配置的版本
node -v #查看 node 是否切换为对应版本
```
* 安装 cnpm 


```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org  
```

![](http://ooad0wrk3.bkt.clouddn.com/14993342506216.jpg)

* 安装 yarn 

```
npm install -g yarn
```



# （二）github 安装

```
yum install git
```
* 查看 git 版本

```
git --version
```
![QQ20180709-143934](http://ooad0wrk3.bkt.clouddn.com/QQ20180709-143934.png)

* 设置 git 记住账号密码

```
git config --global credential.helper store
```

* 创建 qzha 目录，并进入

```
mkdir qzha 
cd qzha/
```

* 拉取qzha-nodejs项目

```
git clone https://gitee.com/schoolits/qzha-nodejs.git
```
![QQ20180709-144303](http://ooad0wrk3.bkt.clouddn.com/QQ20180709-144303.png)

# （三）MySQL yum 安装

## 1.下载mysql的repo源

```
$ wget http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm
```
![QQ20180709-144403](http://ooad0wrk3.bkt.clouddn.com/QQ20180709-144403.png)

## 2.安装mysql-community-release-el7-5.noarch.rpm包

```
$ yum localinstall -y mysql57-community-release-el7-7.noarch.rpm
```

## 3.安装mysql

```
$ yum install -y mysql-community-server
```

## 4.启动 MySQL

```
$ systemctl start mysqld.service
```

MySQL5.7加强了root用户的安全性，因此在第一次安装后会初始化一个随机密码，以下为查看初始随机密码的方式


```
$ grep 'temporary password' /var/log/mysqld.log
```
![](http://ooad0wrk3.bkt.clouddn.com/15311187318362.jpg)

### 4.重置mysql密码

```
$ mysql -u root -p
```

登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：

```
$ sudo chown -R root:root /var/lib/mysql
```

重启mysql服务

```
$ service mysqld restart

// 重启出现问题（Job for mysqld.service failed because the control process exited with error code）的话
$ rm -rf /var/lib/mysql 
然后重新查看初始化随机密码，重新进入
```

接下来登录重置密码：

```
$ mysql -u root -p  //直接回车输入密码进入mysql控制台

SET PASSWORD = PASSWORD('密码');
ALTER USER 'root'@'localhost' PASSWORD EXPIRE NEVER;
flush privileges;
```
### 5.linux下mysql开启远程访问权限 防火墙开放3306端口
> 修改mysql库的user表，将host项，从localhost改为%。%这里表示的是允许任意host访问，如果只允许某一个ip访问，则可改为相应的ip，比如可以将localhost改为192.168.1.123，这表示只允许局域网的192.168.1.123这个ip远程访问mysql。

#### 0、进入 mysql：

```
mysql -u root -p
```
#### 1、使用 mysql库：

```
use mysql;
```
![](http://ooad0wrk3.bkt.clouddn.com/15311187837737.jpg)

#### 2、查看用户表 ：

```
SELECT `Host`,`User` FROM user;
```
![](media/14993317558152/15311187977985.jpg)

#### 3、更新用户表 ：

```
UPDATE user SET `Host` = '%' WHERE `User` = 'root' LIMIT 1;
```
#### 4、强制刷新权限 ：

```
flush privileges;
```
#### 5、远程连接授权：

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;
```
![](media/14993317558152/15311188284600.jpg)

# qzha项目安装与启动
```
1.进入项目目录（项目git拉取参考git操作）
cd ~/qzha/qzha-nodejs

2.安装依赖包
yarn install

3.全局安装pm2（进程守护，自动监控与重启）
yarn global add pm2

4.由pm2启动项目
pm2 start yarn --name="qzha" -- run prod
```
# qzha-nodejs项目日常维护更新操作
```
1.进入项目目录
cd ~/qzha/qzha-nodejs

2.拉取代码
git pull

3.pm2重载
pm2 reload all
```

# Nginx 安装及配置
```
0.如果没有已安装 c++则这步可以跳过
yum install gcc-c++

1.安装 pcre
yum install -y pcre pcre-devel　　

2.安装 zlib
yum install -y zlib zlib-devel

3.安装 openssl
yum install -y openssl openssl-devel

4.下载 Nginx （http://nginx.org/en/download.html）
wget http://nginx.org/download/nginx-1.11.6.tar.gz
tar -zxvf nginx-1.11.6.tar.gz
cd nginx-1.11.6

5.配置安装条件（如果需要 ssl 模块，则需要加上--with-http_ssl_module）
./configure --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-stream  --with-stream_ssl_module --with-pcre

6.编译
make

7.安装
make install

8.修改 nginx.conf 文件

user  root;
worker_processes  1;

error_log  logs/error.log;
error_log  logs/error.log  notice;
error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    upstream node-qzha {
        server 127.0.0.1:8080;
    }

    server {
        listen       80;
        server_name  test.qzha.org;
        location / {
            proxy_pass http://node-qzha/;
	    proxy_set_header Host $host;
	    proxy_redirect off;
            proxy_set_header X-real-IP $remote_addr;
	    proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
	    proxy_connect_timeout 60;
	    proxy_read_timeout 600;
	    proxy_send_timeout 600;
	}

#HTTPS server
    
    server {
        listen       443 ssl;
        server_name  api.qzha.org;

        ssl_certificate      /usr/local/nginx/cert/cert.pem;
        ssl_certificate_key  /usr/local/nginx/cert/cert.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;


        location / {
            proxy_pass http://node-qzha/;
            proxy_set_header Host $host;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_connect_timeout 60;
            proxy_read_timeout 600;
            proxy_send_timeout 600;
        }
        
    }

}

PS:此处需要 /usr/local/nginx 目录下创建 cert 目录并将 ssl 证书保存在该目录下命名为 cert.pem  cert.key

9.测试 Nginx 配置
/usr/local/nginx/sbin/nginx -t

10.启动 Nginx （PS:此处没有将 nginx 加入到环境变量中，所以直接使用路径）
/usr/local/nginx/sbin/nginx 

11.其他 Nginx 命令
验证配置是否正确: nginx -t

查看Nginx的版本号：nginx -V

启动Nginx：start nginx

快速停止或关闭Nginx：nginx -s stop

正常停止或关闭Nginx：nginx -s quit

配置文件修改重装载命令：nginx -s reload


```

