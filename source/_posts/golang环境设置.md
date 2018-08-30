---
title: golang环境设置
date: 2018-08-30 17:50:45
tags:
---

- goalng下载地址 [https://dl.google.com/go/go1.11.linux-amd64.tar.gz](https://dl.google.com/go/go1.11.linux-amd64.tar.gz)  
- 解压 `tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz` **请使用root权限**
- 配置go环境变量 `export PATH=$PATH:/usr/local/go/bin`
- 测试go可用 `go version`
 
- 添加GOPATH `export GOPATH=/home/go`
- 添加go bin `export PATH=$PATH:/home/go/bin`
- 安装glide `curl https://glide.sh/get | sh`