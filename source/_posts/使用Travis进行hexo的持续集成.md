---
title: 使用Travis进行hexo的持续集成
date: 2018-04-30 20:56:33
tags:
---

1. 由于hexo会将项目重写，所以要在github.io上面创建一个dev分支来放代码，而master用来放默认的静态页面
2. 在github的设置页面的Developer settings里面选择Personal access tokens,然后新建一个token，权限选择repo的所有权限
3. 在travis里面开启项目的集成，在设置里面勾选Build only if .travis.yml is present，然后创建一个Environment Variables，把上一步的token放进去
4. 编写 **.travis.yml**
```
language: node_js  #设置语言

node_js: 
- 8.11  #设置相应的版本

cache:
    apt: true
    directories:
        - node_modules # 缓存不经常更改的内容

before_install:
- npm install -g hexo-cli

install:
- npm install

# 这一步是执行,其他的都是环境
script:
- hexo clean
- hexo generate

branches:
  only:
    - hexo  #只监测hexo分支，hexo是我的分支的名称，可根据自己情况设置

after_script:
  - cd ./public
  - git init
  - git config --global user.name "xigaohu"
  - git config --global user.email "xigaohu@163.com"
  - git add .
  - git commit -m "update by travis"
  - git push --force --quiet "https://${REP_TOKEN}@github.com/xigaohu/xigaohu.github.io.git" master:master
```

**注意看travis里面的job日志，有三角形符号的说明可以展开**

最后点击页面的 build unknown 把链接加入readme里面获取徽章

> todo:目前静态文件的版本只能保存一个  
> todo: 没有使用hexo d进行发布