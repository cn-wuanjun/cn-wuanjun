# CentOS 8部署配置测试环境

> 如何在一个新的CentOS 8系统中部署配置测试环境

## 安装nginx

直接用命令行安装

```shell
dnf install -y nginx
```

### **配置nginx**

首先用命令`whereis nginx`找到nginx相关的目录地址

一般nginx的配置文件都在`/etc/nginx/`目录下.

我们需要修改`nginx.conf`文件.

修改运行用户:

```shell
user nginx;
```

修改为:

```shell
user root;
```

不然nginx访问其他程序时会报错:`Access denied`

### **配置转发和证书**

去阿里云SSL控制台下载nginx版本的https证书,将文件上传到服务器上.

修改配置文件:

```nginx
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user root;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

# Settings for a TLS enabled server.

    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        ssl_certificate /home/dingzi/3645118__znzmo.com.pem;   #将domain name.pem替换成您证书的文件名。
        ssl_certificate_key /home/dingzi/3645118__znzmo.com.key;   #将domain name.key替换成您证书的密钥文件名。
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
            proxy_set_header Host $host;
            proxy_pass http://127.0.0.1:3000/;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

    server {
        listen       443 ssl http2;
        server_name  api.znzmo.com;
        root         /usr/share/nginx/html;

        ssl_certificate /home/dingzi/3645118__znzmo.com.pem;   #将domain name.pem替换成您证书的文件名。
        ssl_certificate_key /home/dingzi/3645118__znzmo.com.key;   #将domain name.key替换成您证书的密钥文件名。
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
            proxy_set_header Host $host;
            proxy_pass http://127.0.0.1:8080/;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

}
```

相关配置说明:

1. `proxy_set_header Host $host;`这个是用来携带域名请求头的.不配置的话,取不到具体的域名
2. `ssl*`开头的都是配置https证书相关的配置项
3. 如果要配置多域名转发,需要配置多个server,并且需指定`server_name`
4. `default_server`只能有一个

## 部署前端项目

1. 安装npm
2. 安装yarn
3. 安装pm2
4. clone项目,编译,再启动

## 部署后端项目

1. 安装配置jdk
2. 配置tomcat端口为8080,打包项目(jar包).
3. 上传服务器,以jar包形式启动`nohup java jar dingzi_code_web_server.jar &`

## 配置服务器hosts

修改服务器hosts文件

```shell
vim /etc/hosts
```

增加如下配置项:

> 127.0.0.1 api.znzmo.com
>
> 127.0.0.1 www.znzmo.com

## 配置本机hosts

> 52.130.94.74 www.znzmo.com
>
> 52.130.94.74 3d.znzmo.com
>
> 52.130.94.74 su.znzmo.com
>
> 52.130.94.74 tietu.znzmo.com
>
> 52.130.94.74 sgt.znzmo.com
>
> 52.130.94.74 api.znzmo.com

其中`52.130.94.74`是服务器ip.

## 测试环境是否可以访问

1. 直接打开浏览器输入网址访问即可
2. 如果无法访问.在服务器上执行尝试访问`curl https://www.znzmo.com/`
3. 如果服务器上可以访问.本机无法访问,需要去配置服务器的端口开放策略,开放443端口
