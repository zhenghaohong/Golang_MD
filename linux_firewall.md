## Linux 无法访问端口问题

CentOS 7.0默认使用的是firewall作为防火墙

查看状态： systemctl status firewalld 

关闭： systemctl stop firewalld

启动： systemctl start firewalld

查看：firewall-cmd --zone=public --query-port=80/tcp

删除：firewall-cmd --zone=public --remove-port=80/tcp --permanent

查看所有打开的端口： firewall-cmd --zone=public --list-ports

查看当前所有tcp端口：netstat -ntlp

开放端口永久生效：firewall-cmd --zone=public --add-port=80/tcp --permanent

重新载入：firewall-cmd --reload

非CentOS 7.0 可能使用“iptables”按照“iptables”配置开放端口即可

### 如果开启端口后还不能访问、则去阿里云添加墙规则

具体看阿里云--安全--防火墙 

```

firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --reload



注意事项：配置完不能访问 ，注意是否端口没开放、https(443)和http(80)


git remote add origin https://gitdoc.calfkaka.com/eks/eks-parent-server-api.git
git push -u origin master


https://gitdoc.calfkaka.com/eks/eks-parent-server-api.git



calfkaka.com.conf





```

| 用途                  | 内网          | 外网           |                                          |
| --------------------- | ------------- | -------------- | ---------------------------------------- |
| /api/parents/         | 172.16.0.17   | 42.193.190.206 |                                          |
| /mqtt-admin/          | 106.55.250.61 |                |                                          |
| /eks-admin-api/       |               | 120.76.192.60  |                                          |
| /api/pre-parents/     | 172.16.0.17   | 42.193.190.206 | 备注                                     |
| /api/parents/         | 172.16.0.17   | 42.193.190.206 |                                          |
| /api/baby/            | 172.16.0.17   | 42.193.190.206 |                                          |
| /api/parents-control/ | 172.16.0.17   |                | proxy_pass http://172.16.0.17:8003/api/; |
| /api/baby-ws/         | 172.16.0.3    | 1.14.134.203   | proxy_pass http://1.14.134.203:8888/;    |
| /ws/baby/             | 172.16.0.3    | 1.14.134.203   |                                          |
| /ws/smart-hardware/   | 159.75.36.133 |                |                                          |
|                       |               |                |                                          |
|                       |               |                |                                          |
|                       |               |                |                                          |
|                       |               |                |                                          |

