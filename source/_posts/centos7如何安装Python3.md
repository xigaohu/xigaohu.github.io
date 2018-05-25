---
title: 如何安装Python3
date: 2018-05-18 07:20:30
tags: python
---

1. 编译环境准备
    ```
    yum groupinstall 'Development Tools'
    yum install zlib-devel bzip2-devel openssl-devel ncurese-devel
    ```

2. 下载python3.6.5代码包
    ```
    wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz
    ```

    可以在**https://www.python.org/ftp/python/** 里面查找需要用的版本  

3. 编译

    ```
    tar Jxvf Python-3.6.5.tar.xz
    cd Python-3.6.5
    ./configure --prefix=/usr/local/python3
    make && make install
    ```

4. 更换Python版本

    - 备份旧版本  
    `mv /usr/bin/python /usr/bin/python2.7`
    - 新建指向新版本 Python 以及 pip 的软连接
        ```
        ln -s /usr/local/python3/bin/python3.6 /usr/bin/python
        ln -s /usr/local/python3/bin/pip3 /usr/bin/pip  
        ```

5. 更新yum相关设置

    打开文件
    ```
    vi /usr/bin/yum
    ```
    将第一行修改为
    ```
    #!/usr/bin/python2.7
    ```
    若出现错误
    ```
    File "/usr/libexec/urlgrabber-ext-down", line 28
    ```
    将/usr/libexec/urlgrabber-ext-down的第一行修改

    [参考链接]("https://www.jianshu.com/p/8bd6e0695d7f")

#### 其他尝试

若第4步改为
```
ln -s /usr/local/python3/bin/python3.6 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```
python 将不用修改其他的内容，但是在使用python3 安装的库时候，例如使用gunicorn,必须使用 `python3 -m gunicorn ...` 不能直接用gunicorn,因为默认是使用python(当前是python2)进行执行的

还是要等centos进行支持呀