常用命令：

```shell
重启服务： service nginx restart
快速停止或关闭Nginx：nginx -s stop
正常停止或关闭Nginx：nginx -s quit
配置文件修改重装载命令：nginx -s reload
改完配置之后，检查配置有没问题: nginx -t
配置完重启服务
```

配置文件: etc/nginx/nginx.conf

### 以下案例为独立配置文件：include /etc/nginx/default.d/*.conf;

Example:

Port

配置7000端口的网站

/etc/nginx/conf.d/test.conf

```shell
server {
     	listen       7000;

	server_name  _;
	root    /home/website/test;
	include /etc/nginx/default.d/*.conf;
	location / {
            root   /home/website/test;
            index  index.html index.htm index.php;
            autoindex  on;
        }
    }

```



服务端口用 反向代理

```shell
server {
    listen          80;
    server_name     ccmp.qidaochina.com;

    location / {
        proxy_pass    http://127.0.0.1:8080;
        index  index.html index.htm;
    }
}


```













