---
title: mvn导入本地包
date: 2018-05-29 11:20:22
tags:
---
直接执行mvn即可

```
mvn install:install-file -Dfile=bitcoin-json-rpc-client-1.1.jar -DgroupId=com.azazar -DartifactId=bitcoin-json-rpc-client -Dversion=1.1 -Dpackaging=jar
```