---
title: zec的安装
date: 2018-07-19 20:34:39
tags:
---
### 问题1 GLIBCXX_3.4.20 
    /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found
运行下面的命令，发现时少了3.4.20
```
[root@localhost src]# strings /usr/lib64/libstdc++.so.6 | grep GLIBCXX
GLIBCXX_3.4
GLIBCXX_3.4.1
GLIBCXX_3.4.2
GLIBCXX_3.4.3
GLIBCXX_3.4.4
GLIBCXX_3.4.5
GLIBCXX_3.4.6
GLIBCXX_3.4.7
GLIBCXX_3.4.8
GLIBCXX_3.4.9
GLIBCXX_3.4.10
GLIBCXX_3.4.11
GLIBCXX_3.4.12
GLIBCXX_3.4.13
GLIBCXX_3.4.14
GLIBCXX_3.4.15
GLIBCXX_3.4.16
GLIBCXX_3.4.17
GLIBCXX_3.4.18
GLIBCXX_3.4.19
GLIBCXX_DEBUG_MESSAGE_LENGTH
```
* 先安装gcc
```
wget https://ftp.gnu.org/gnu/gcc/gcc-8.1.0/gcc-8.1.0.tar.gz
//解压
//required libraries:
yum install libmpc-devel mpfr-devel gmp-devel

yum install zlib-devel*

./configure --with-system-zlib --disable-multilib --enable-languages=c,c++

make -j 8 <== this may take around 70 minutes or less to finish with 8 threads
              (depending on your cpu speed)

make install
```
参考stackoverflow文章
[How to Install gcc 5.3 with yum on CentOS 7.2](https://stackoverflow.com/questions/36327805/how-to-install-gcc-5-3-with-yum-on-centos-7-2)
然后找到libstdc++.so.6.0.25

`find / -name libstdc++.so.6*`  
* 替换原来的libstdc++.so.6
```
cp /usr/local/lib64/libstdc++.so.6.0.21 /usr/lib64/
cd /usr/lib64/
rm -f libstdc++.so.6
ln -s libstdc++.so.6.0.21 libstdc++.so.6
```
### 问题2 没有glibc_2.18
```
/lib64/libc.so.6: version `GLIBC_2.18' not found (required by /lib64/libstdc++.so.6)
```
* 安装
```
curl -O http://ftp.gnu.org/gnu/glibc/glibc-2.18.tar.gz
tar zxf glibc-2.18.tar.gz 
cd glibc-2.18/
mkdir build
cd build/
../configure --prefix=/usr
make -j 4
make install
```
[参考链接](https://www.jianshu.com/p/92c7a042d8ba)