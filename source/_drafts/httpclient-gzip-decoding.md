---
title: HttpClient中gzip压缩导致post请求数据返回的数据乱码问题
date: 
tags: [爬虫, Java, 乱码, Gzip, Httpclient]
categories: Java
layout: post
updated: 
comments: 
permalink: 
---

解决了gzip压缩的网页返回数据引起的乱码问题。

<!--more-->

使用post请求网站数据，发现返回的数据都是乱码，各种调试测试改编码，无果。后来发现post的header设置如下：
```java
httpPost.setHeader("Connection","keep-alive");    
httpPost.setHeader("Cache-Control", "max-age=0");
httpPost.setHeader("Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8");
httpPost.setHeader("Accept-Encoding", "gzip, deflate, sdch");
httpPost.setHeader("Accept-Language", "en-US,en;q=0.8");
```

注意到Accept-Encoding中接受的有gzip，后来发现网站返回的数据是gzip压缩过的，于是加上gzip解压，问题就解决了。代码如下：

```java
if (entity != null) {
    if (entity.getContentEncoding() != null) {
        if ("gzip".equalsIgnoreCase(entity.getContentEncoding().getValue())) {
            entity = new GzipDecompressingEntity(entity);
        } else if ("deflate".equalsIgnoreCase(entity.getContentEncoding().getValue())) {
            entity = new DeflateDecompressingEntity(entity);
        }
    }
}
```


