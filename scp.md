# centos下scp命令安装和使用

如果未安装scp，则命令如下：yum -y install openssh-clients

1. 复制文件（本地>>远程）：scp admin.text root@192.168.0.82:/home/project
2. 复制文件（远程>>远程）：scp root@192.168.0.82:/home/Lee/test/test.txt /home/Lee/test/
3. 复制目录（本地>>远程）：scp -r test/  root@192.168.0.82:/home/
4. 复制文件（远程>>本地）：scp -r root@192.168.0.82:/home/ /home/Lee/