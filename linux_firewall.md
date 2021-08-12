## Linux 无法访问端口问题

CentOS 7.0默认使用的是firewall作为防火墙

关闭： systemctl stop firewalld

启动： systemctl start firewalld

查看所有打开的端口： firewall-cmd --zone=public --list-ports

查看当前所有tcp端口：netstat -ntlp

开放端口永久生效：firewall-cmd --zone=public --add-port=80/tcp --permanent

重新载入：firewall-cmd --reload

非CentOS 7.0 可能使用“iptables”按照“iptables”配置开放端口即可

### 如果开启端口后还不能访问、则去阿里云添加墙规则

具体看阿里云--安全--防火墙 

