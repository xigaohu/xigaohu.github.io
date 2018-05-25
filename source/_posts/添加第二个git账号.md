---
title: 添加第二个git账号
date: 2018-05-25 13:13:39
tags:
---

1. 生成公钥私钥

    ```
    ssh-keygen -t rsa -C "gaohubin.163.com"  
    ```
        
    输入**自定义**的文件名 id_rsa_lab，然后会生成两个文件**id_rsa_lab.pub**与**id_rsa_lab**
    然后密码不输入直接回车两次，这样虽然有点不安全，但以后不用输密码了  

2. 将公钥放入gitlab的SSH key的设置里，点击add key

    ![](addkey.png)

3. 在.ssh文件夹下面添加无后缀的config文件
   
    第一行是域名地址的别名  
    第二行是IP 或者域名  
    第三行是 用户名可以改  
    第四行是 私钥位置  

    ```
    Host gitlab              
    Hostname 202.182.104.28
    User git
    IdentityFile C:\Users\gao\.ssh\id_rsa_lab
    ```
参考: [一台电脑，两个及多个git账号配置](https://www.cnblogs.com/fanbi/p/7825746.html)