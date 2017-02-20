title: Hexo+Github搭建个人博客
date: 
tags: [Hexo, Github, 博客]
categories: 博客
layout: post
updated: 
comments: 
permalink: 
---


<!--more-->


# 用git进行博客备份和还原

## 博客备份

## 博客还原（在新环境中继续撰写博客）

- 安装 git、nodejs、hexo环境（同上）
- 克隆博客仓库
> git clone [你的仓库URL]

- 切换到源码分支（如果源码分支是默认分支，那么克隆过后会默认处于该分支下，不需要再手动切换）
> git checkout [你的分支名字]

- 安装npm hexo 本地环境
> npm install hexo --save #安装hexo环境到本目录
> npm install #安装npm 环境到本目录,该步骤不做的话可能会出现hexo generate过后缺少文件（如index页面）
> npm install hexo-deployer-git #hexo通过git发布网页的插件

- 环境恢复成功，可以使用hexo g -d 发布博客了 




