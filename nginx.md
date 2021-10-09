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













```shell
   
   server{
   listen 80;
    listen [::]:80;
    server_name devparents.calfkaka.com;
    root /usr/share/nginx/html;
    
    
 		location / {
        proxy_pass http://localhost:8501;
        index index.html index.htm index.jsp;
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}

```



### 配置https例子



```json
 server {
    listen 443 ssl http2 ;
    listen [::]:443 ssl http2 ;
    server_name apiserver.calfkaka.com;
    root /www;

    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout 10m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
		ssl_certificate "/usr/local/nginx/ssl/apiserver.calfkaka.com.pem";
    ssl_certificate_key "/usr/local/nginx/ssl/apiserver.calfkaka.com.key";

     location / {
        proxy_pass http://localhost:8101;
     }
  }
}


```

