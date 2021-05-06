## /etc/profile 和~/.bash_profile区别

#### /etc/profile

```she
为系统的每个用户设置环境信息和启动程序，当用户第一次登录时，该文件被执行，其配置对所有登录的用户都有效。当被修改时，重启或使用命令 source /etc/profile 才会生效。英文描述：”System wide environment and startup programs, for login setup.”
```

#### ~/.bash_profile

```go
为当前用户设置专属的环境信息和启动程序，当用户登录时该文件执行一次。默认情况下，它用于设置环境变量，并执行当前用户的 .bashrc 文件。理念类似于 /etc/profile，只不过只对当前用户有效，需要重启或使用命令 source ~/.bash_profile 才能生效。(注意：Centos7系统命名为.bash_profile，其他系统可能是.bash_login或.profile。
```



