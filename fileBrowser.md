File Browser文件浏览器配置

安装：linux

```
1、curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash
2、filebrowser -r /path/to/your/files
```



```
iwr -useb https://raw.githubusercontent.com/filebrowser/get/master/get.ps1 | iex
filebrowser -r /path/to/your/files
```



```shell
创建配置数据库：filebrowser -d /etc/filebrowser.db config init
设置监听地址：filebrowser -d /etc/filebrowser.db config set --address 0.0.0.0
设置监听端口：filebrowser -d /etc/filebrowser.db config set --port 8088
设置语言环境(中文)：filebrowser -d /etc/filebrowser.db config set --locale zh-cn
设置日志位置：filebrowser -d /etc/filebrowser.db config set --log /var/log/filebrowser.log
添加一个用户：filebrowser -d /etc/filebrowser.db users add root password --perm.admin，其中的root和password分别是用户名和密码，根据自己的需求更改。


```



filebrowser users add test test 

  

