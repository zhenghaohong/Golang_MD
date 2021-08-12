## 一切皆文件



```shell
查询pid 相关信息： 					lsof -p 8001  #常用
列出谁在使用某个端口: 				lsof -i :3306
列出所有tcp 网络连接信息:			lsof -i tcp
根据文件描述列出对应的文件信息: lsof -d 3 | grep PARSER1(关键词)




```

