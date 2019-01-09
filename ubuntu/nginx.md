# Nginx

## 1.安装nginx

```shell
sudo apt-get install nginx
```



## 2.文件安装目录查看

```shell
whereis nginx
```



## 3.ngxin的启动和停止

```mysql
service nginx start //启动nginx
service nginx stop //启动nginx
service nginx restart //启动nginx
service nginx status //启动nginx
```



## 4.nginx的配置

### 4.1 http反向代理配置

```shell
server{
    listen 80;
    server_name aaa.aliyun.com;
    location / {
        proxy_pass  http://localhost:8081;
    }
}

server{
    listen 80;
    server_name bbb.aliyun.com;
    location / {
        proxy_pass  http://localhost:8088/appName/;
    }
}
```



### 4.2 https反向代理配置

```shell
server {
    listen       443;
    server_name www.aliyun.xyz;

    ssl on;

    ssl_certificate   /usr/nginx/cert/20180529001002.pem;
    ssl_certificate_key  /usr/nginx/cert/20180529001002.key;

    ssl_session_timeout  5m;

    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://localhost:8080;

        proxy_set_header Host $host;
    }
}
```



### 4.3 静态文件配置

```shell
location /image/ {
            root   /usr/local/static/;
            autoindex on;
        }
```

