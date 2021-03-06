---
title: Atom 手册
tags:
  - Atom
  - 工具
categories: 工具
date: 2017-03-15 00:00:00
updated:
comments:
permalink:
---


Atom 是 GitHub 打造的现代编辑器，速度快，跨平台，支持各种插件以及可以异常方便地自定义扩展。

下载地址：https://atom.io/

<!--more-->

输入apm查看是否安装了 Atom Package Manager.

```bash
C:\Users\ymhe>apm

apm - Atom Package Manager powered by https://atom.io

Usage: apm <command>

where <command> is one of:
    clean, config, dedupe, deinstall, delete, dev, develop, disable, docs,
    enable, erase, featured, home, i, init, install, link, linked, links, list,
    ln, lns, login, ls, open, outdated, publish, rebuild, rebuild-module-cache,
    remove, rm, search, show, star, starred, stars, test, uninstall, unlink,
    unpublish, unstar, update, upgrade, view.

Run `apm help <command>` to see the more details about a specific command.

Options:
  --color        Enable colored output                                     [boolean] [default: true]
  -v, --version  Print the apm version
  -h, --help     Print this usage message

  Prefix an option with `no-` to set it to false such as --no-color to disable  colored output.
```

- 安装汉化插件

```bash
C:\Users\ymhe>apm install simplified-chinese-menu
Installing simplified-chinese-menu to C:\Users\ymhe\.atom\packages done
```

- 安装 minimap

```bash
C:\Users\ymhe>apm install minimap
Installing minimap to C:\Users\ymhe\.atom\packages done
```

- 开启 markdown 预览

菜单栏 扩展-Markdown Preview-Toggle Preview

----

有没有感觉很酷炫

![](http://oduq3lfcc.bkt.clouddn.com/image/atom%E7%95%8C%E9%9D%A2.png)

- terminal-plus

cmd+` 打开终端

插件设置中可以改变shell

![](http://oduq3lfcc.bkt.clouddn.com/image/atomterminal.png)
