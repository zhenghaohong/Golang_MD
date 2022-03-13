## Docker

Centos：

删除旧版本：

```sh
yum remove docker  docker-common docker-selinux docker-engine
```

## 安装需要的软件包

yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

```sh
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

## 设置Docker yum源

```sh
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

## 查看所有仓库中所有docker版本

```sh
yum list docker-ce --showduplicates | sort -r
```

## 安装docker

```sh
// 由于repo中默认只开启stable仓库，故这里安装的是最新稳
sudo yum install docker-ce

// 如果需要安装指定的docker版本
sudo yum install docker-ce-18.06.1.ce  

```



## 启动

设为开机启动

```sh
systemctl enable docker
```

启动

```sh
systemctl start docker
```

查看启动状态

```sh
systemctl status docker
```

查看版本

```sh
docker version
```





容器

```shell
删除所有停止的容器：docker container prune
列出容器列表：docker container ls -a

docker run -d --privileged=true --restart=always --name=gitea -p 10022:22 -p 8000:3000 -v /var/lib/gitea:/data gitea/gitea:latest


docker run -d --name=gitea -p 8000:8000 -v /var/lib/gitea:/data zhengteshuai/gitea-backup:zts

docker run -p 8000:8000 -v /var/lib/gitea:/data -d zhengteshuai/gitea-backup:zts

docker run -d --name=gitea -p 10022:22 -p 8000:3000 -v /var/lib/gitea:/data gitea/gitea:latest




mv data/conf/app.ini /var/lib/gitea/gitea/conf/app.ini
mv data/* /var/lib/gitea/gitea/data/
mv log/* /var/lib/gitea/gitea/log/

mv repos/* /var/lib/gitea/repositories/

scp dist.zip


docker run -d \
  --name mongodb \
  --restart always \
  --net=yapi \
  -p 27017:27017 \
  -v mongo-data:/data/db \
  -e MONGO_INITDB_DATABASE=yapi \
  -e MONGO_INITDB_ROOT_USERNAME=yapipro \
  -e MONGO_INITDB_ROOT_PASSWORD=yapipro1024 \
  mongo
  


  
  27017 docker-yapi
  
  
 db.auth("yapipro", "yapipro1024");
 
db.createUser({user: 'yapi',pwd: 'yapi123456',roles:[{role:"dbAdmin",db:"yapi"},{role:"readWrite",db:"yapi"}]});



{
   "port": "3000",
   "adminAccount": "zhengteshuai@gmail.com",
   "timeout":120000,
   "db": {
     "servername": "mongo",
     "DATABASE": "yapi",
     "port": 27017,
     "user": "yapi",
     "pass": "yapi123456",
     "authSource": ""
   },
   "mail": {
     "enable": true,
     "host": "smtp.gmail.com",
     "port": 465,
     "from": "*",
     "auth": {
       "user": "zhengteshuai@gmail.com",
       "pass": "xxx"
     }
   }
 }
 
 
 
 "closeRegister":true
 
 docker run -d --rm \
  --name yapi-init \
  --link mongodb:mongo \
  --net=yapi \
  -v $PWD/config.json:/yapi/config.json \
   yapipro/yapi \
  server/install.js



docker run -d \
   --name yapi \
   --link mongodb:mongo \
   --restart always \
   --net=yapi \
   -p 3000:3000 \
   -v $PWD/config.json:/yapi/config.json \
   yapipro/yapi \
   server/app.js


docker exec a5ee6948a22d mongorestore -d db --drop --dir /data/db




docker run -it --rm --link mongodb:mongo --entrypoint npm --workdir /api/vendors registry.cn-hangzhou.aliyuncs.com/anoy/yapi run install-server



sudo scp yapi.calfkaka.com.key root@120.25.205.7:/etc/nginx/ssl/




Eks_test_28990800


mv app.ini /var/lib/gitea/gitea/conf/app.ini

mv data/* /var/lib/gitea/gitea/
mv log/* /var/lib/gitea/gitea/log/
mv repos/* /var/lib/gitea/git/repositories/


chown -R gitea:gitea /etc/gitea/conf/app.ini /var/lib/gitea





```



## 常用命令

```json
列出所有镜像：docker images
运行容器   : docker run -d --name=gitea -p 10022:22 -p 8000:3000 -v /var/lib/gitea:/data gitea/gitea:latest (例如：--name(启动的容器名称) -p 宿主端口:容器端口 -v 宿主目录:容器目录（挂载） 最后面的是镜像名称:标签)
重启容器: docker restart 容器id/容器名称
进入容器：docker exec -it 容器id  /bin/sh
停止容器: docker stop 容器id/容器名称
删除所有停止的容器：docker container prune(一般容器停止完需要用这个命令清除缓冲的数据)
列出容器列表：docker container ls -a
强制删除镜像: docker rmi -- force 镜像id
删除镜像: docker rmi 镜像id
查找镜像: docker search 镜像名称
拉取镜像：docker pull 镜像名称
登录帐号: docker login -u 用户名称
复制宿主文件到容器：docker cp <宿主文件地址>  <容器id或者容器名称>:<容器内部地址>
复制容器文件到宿主: docker cp <容器id或者容器名称>:<容器内部地址>  <宿主文件地址>
```

运行：

拉取镜像---启动容器
