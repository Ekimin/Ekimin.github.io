---
title: 文件编码批量转换为UTF-8
tags:
  - 编码
  - 乱码
  - UTF-8
  - Python
  - GBK
categories: Python
date: 2017-03-15 20:13:24
updated:
comments:
permalink:
---


用 Python 实现的批量转换文件编码为UTF-8

环境：Python 3

<!--more-->

```python
# -*- coding:utf-8 -*-

import os,sys
import chardet

def convert( filename, in_enc = "GBK", out_enc="UTF8" ):
    try:
        print ("convert " + filename,)
        content = open(filename,'rb').read()
        result = chardet.detect(content)#通过chardet.detect获取当前文件的编码格式串，返回类型为字典类型
        coding = result.get('encoding')#获取encoding的值[编码格式]
        if coding != 'utf-8':#文件格式如果不是utf-8的时候，才进行转码
            print ("to utf-8!")
            new_content = content.decode(in_enc).encode(out_enc)
            open(filename, 'wb').write(new_content)
            print (" done")
        else:
            print (coding)
    except IOError as e:
    # except:
        print (" error")


def explore(dir):
    for root, dirs, files in os.walk(dir):
        for file in files:
            path = os.path.join(root, file)
            convert(path)

def main():
    for path in sys.argv[1:]:
        if os.path.isfile(path):
            convert(path)
        elif os.path.isdir(path):
            explore(path)

if __name__ == "__main__":
    main()
```

执行该文件需要 chardet，安装chardet：

- 安装pip （3.4版本及以后版本都已经默认安装了pip）
https://pip.pypa.io/en/stable/installing/

Windows下需要添加pip环境变量

![](http://oduq3lfcc.bkt.clouddn.com/image/python/piphome.png)

![](http://oduq3lfcc.bkt.clouddn.com/image/python/pythonhone.png)

- 下载 chardet

https://pypi.python.org/pypi/chardet 下载chardet-2.3.0.tar.gz (md5)

解压，进入解压后的目录 python setup.py install 安装