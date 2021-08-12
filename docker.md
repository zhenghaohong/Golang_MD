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

