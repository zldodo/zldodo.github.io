---
title: 博客站点在多个终端下的同步实现
tags: hexo
categories: 探知
date: 2017-05-04 11:29:07
update: 2017-05-04 10:19:11
---

**最初建立blog的电脑**

1. 新建文件夹hexo，并复制相关文件
	- 必需：soure,themes,_config.yml
	- 可选：.gitignore,scsffolds, package.json,这三个文件后续也可通过 `hexo init` 得到
2. 建立新分支hexo, 并将hexo文件夹下的内容push至blog's repository的新分支hexo
```	
git branch -b "hexo"
git add .
git commit
git push origin 
```
**另一台新电脑**

1. 新建文件夹blog_child1,配置git,node_js,hexo 
2. remote 到github上的仓库
```
git remote add origin <serve>
```
	例如，serve=git@github.com/zldodo/zldodo.github.io.git
3. pull hexo 分支上的内容
```
git pull origin hexo
```
4. 修改blog内容，然后push到 hexo分支上，并同步到博客网页
```
git add .
git commit -m
git push origin hexo
hexo d -g
```
### 常用命令指南
```
hexo s -p 5000 // 本地查看显示效果localhost:5000
```
