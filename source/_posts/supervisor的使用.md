---
title: supervisor的使用
date: 2018-06-25 22:59:44
tags:
---
supervisor是一个用 Python 写的进程管理工具，可以很方便的用来启动、重启、关闭进程（不仅仅是 Python 进程）。除了对单个进程的控制，还可以同时启动、关闭多个进程，比如很不幸的服务器出问题导致所有应用程序都被杀死，此时可以用 supervisor 同时启动所有应用程序而不是一个一个地敲命令启动。

#### 安装
可以使用

`pip install supervisor`  
`pip install git+https://github.com/Supervisor/supervisor` python3版本

或者用

`yum install supervisor`

#### 配置
获取配置文件

`echo_supervisord_conf > /etc/supervisord.conf`
如果出现`echo_supervisord_conf: command not **found**` 则可能需要输入全路径，使用
`find / -name supervisor*`找到路径

```ini
[unix_http_server]
file=/tmp/supervisor.sock   ; UNIX socket 文件，supervisorctl 会使用
;chmod=0700                 ; socket 文件的 mode，默认是 0700
;chown=nobody:nogroup       ; socket 文件的 owner，格式： uid:gid

;[inet_http_server]         ; HTTP 服务器，提供 web 管理界面
;port=127.0.0.1:9001        ; Web 管理后台运行的 IP 和端口，如果开放到公网，需要注意安全性
;username=user              ; 登录管理后台的用户名
;password=123               ; 登录管理后台的密码

[supervisord]
logfile=/tmp/supervisord.log ; 日志文件，默认是 $CWD/supervisord.log
logfile_maxbytes=50MB        ; 日志文件大小，超出会 rotate，默认 50MB
logfile_backups=10           ; 日志文件保留备份数量默认 10
loglevel=info                ; 日志级别，默认 info，其它: debug,warn,trace
pidfile=/tmp/supervisord.pid ; pid 文件
nodaemon=false               ; 是否在前台启动，默认是 false，即以 daemon 的方式启动
minfds=1024                  ; 可以打开的文件描述符的最小值，默认 1024
minprocs=200                 ; 可以打开的进程数的最小值，默认 200

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///tmp/supervisor.sock ; 通过 UNIX socket 连接 supervisord，路径与 unix_http_server 部分的 file 一致
;serverurl=http://127.0.0.1:9001 ; 通过 HTTP 的方式连接 supervisord

; 包含其他的配置文件
[include]
files = relative/directory/*.ini    ; 可以是 *.conf 或 *.ini 用绝对路径
```
#### program配置
将include修改为

```ini
[include]
files = /etc/supervisord.d/*.ini
```


在对应的目录下面创建文件
```ini
[supervisord]
environment=PYTHONIOENCODING=utf-8,,FLASK_CONFIG="TESTING"
[program:batt_allcoin]
directory = /home/app/batt_eth_allcoin ; 程序的启动目录
command = python -B -u batt_eth_allcoin.py  ; 启动命令
autostart = false    ; 在 supervisord 启动的时候也自动启动
startsecs = 5        ; 启动 5 秒后没有异常退出，就当作已经正常启动了
autorestart = false   ; 程序异常退出后自动重启
startretries = 3 
redirect_stderr = true  ; 把 stderr 重定向到 stdout，默认 false
stdout_logfile_maxbytes = 5MB  ; stdout 日志文件大小，默认 50MB
stdout_logfile_backups = 20     ; stdout 日志文件备份数
stdout_logfile = /home/app/batt_eth_allcoin/debug.log

```

另一个配置文件
```ini
[program:binance]
directory = /home/app/coins/multi_coin ; 程序的启动目录
command = python2 -B -u imitate_order.py  ; 启动命令
autostart = false    ; 在 supervisord 启动的时候也自动启动
startsecs = 5        ; 启动 5 秒后没有异常退出，就当作已经正常启动了
autorestart = false   ; 程序异常退出后自动重启
startretries = 3 
redirect_stderr = true  ; 把 stderr 重定向到 stdout，默认 false
stdout_logfile_maxbytes = 5MB  ; stdout 日志文件大小，默认 50MB
stdout_logfile_backups = 20     ; stdout 日志文件备份数
stdout_logfile = /home/app/coins/multi_coin/debug.log
```

#### 启动
启动supervisor

`supervisord -c /etc/supervisord.conf`
如果supervisorctl不存在，就找到路径使用 `ln -s /usr/local/python3/bin/supervisorctl` 添加软链接 `/usr/bin/supervisorctl`

启动进程

```
$ supervisorctl status
$ supervisorctl stop usercenter
$ supervisorctl start usercenter
$ supervisorctl restart usercenter
$ supervisorctl reread
$ supervisorctl update
```
*注意*
在使用supervisorctl 时，如果supervisorctl 无法查找到配置文件，

supervisorctl  无法获知与supervisord 该如何通讯，你可能会看到如下错误

```
root@xxx:/home/operation# supervisorctl status  
http://localhost:9001 refused connection
```
处理解决办法：[supervisorctl配置文件简介](http://blog.51cto.com/finalbattle/1868454)

```
root@xxx:/home/operation# supervisorctl -c /path/to/supervisord.conf status  
root@xxx:/home/operation# ln -s /path/to/supervisord.conf /etc/
```
[使用 supervisor 管理进程](http://liyangliang.me/posts/2015/06/using-supervisor/)

[Supervisor and Environment Variables
](https://stackoverflow.com/questions/12900402/supervisor-and-environment-variables)

[Supervisor的作用与配置](https://www.jianshu.com/p/0226b7c59ae2)

[[supervisor] 使用小记(入门教程)](https://blog.csdn.net/orangleliu/article/details/45057377)