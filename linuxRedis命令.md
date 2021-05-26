

如果不知道redis-server文件位置输入如下命令查询位置

```shell
find / -name redis-server
```

linux 下redis启动命令

```shell
/usr/local/bin/redis-server  /home/data/redis-3.2.1/redis.conf  (/usr/local/redis/redis.conf)
```



查看是否启动成功：

```sh
解决 redis connection refused
redis 默认没加载配置文件 ，需要指定声明
redis-server /usr/local/etc/redis.conf
ps -ef | grep redis-server

netstat -nplt

使用ps 查看或 netstat 
ps aux | grep redis-server
netstat -tunple | grep 6379


```









