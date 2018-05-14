---
title: java环境变量
date: 2018-04-30 20:55:02
tags: java
---
1. PATH里添加
`%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;`

2. 新建CLASSPTH,在里面添加
`.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`

3. 新建JAVA_HOME, 添加jdk路径
java安装位置 比如`C:\Program Files\Java\jdk1.8.0_172`

**使用系统变量，不要在用户变量里操作**