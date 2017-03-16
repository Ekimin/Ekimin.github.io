---
title: Solr 6.4.1 搭建和使用教程
date: 2016-08-22 19:22:54
tags: [Solr, 大数据]
categories: 大数据
layout: post
updated:
comments:
permalink: 
---


<!--more-->

# Solr 搭建

## 环境

Solr：solr-6.4.1
JDK：1.8.0_111
tomcat：8.5.1 （8版本都支持）
OS： CentOs 7


## 单例模式下部署

**Solr目录结构**

![](http://oduq3lfcc.bkt.clouddn.com/solr%E7%9B%AE%E5%BD%95.png)

solr6.4内嵌了jetty，不再需要tomcat作为容器启动，所以安装solr6.4.1非常简单：
解压下载solr6.4.1到任意目录，然后cd 到 bin 目录下运行命令

```bash
[ymhe@iZj6c2vq0s1w6wkap2geanZ bin]$ ./solr start
Archiving 1 old GC log files to /home/ymhe/software/solr-6.4.1/server/logs/archived
Archiving 1 console log files to /home/ymhe/software/solr-6.4.1/server/logs/archived
Waiting up to 180 seconds to see Solr running on port 8983 [\]  
Started Solr server on port 8983 (pid=28125). Happy searching!
```

默认端口8983，现在访问浏览器地址 http://localhost:8983/solr 就可以访问。

![](http://oduq3lfcc.bkt.clouddn.com/image/solr/webui.png)

## solr 命令

solr option [参数]

- solr start -p 8984  --使用指定的端口号启动solr服务
-  solr start -help


## 新建 Core

1. 在solr-6.4.1/server/solr下，创建一个文件夹（名字自定义），这个文件夹就是一个core
```bash
[ymhe@iZj6c2vq0s1w6wkap2geanZ solr]$ pwd
/home/ymhe/software/solr-6.4.1/server/solr
[ymhe@iZj6c2vq0s1w6wkap2geanZ solr]$ mkdir wtt_core
```

2.  solr-6.4.1/server/solr/configsets/basic_configs下的conf文件夹复制进来
```bash
[ymhe@iZj6c2vq0s1w6wkap2geanZ basic_configs]$ pwd
/home/ymhe/software/solr-6.4.1/server/solr/configsets/basic_configs
[ymhe@iZj6c2vq0s1w6wkap2geanZ basic_configs]$ cp -r conf/ /home/ymhe/software/solr-6.4.1/server/solr/wtt_core/
```

3. 在sol界面上新建core，填入name和instancDir（第一步中建立的文件夹的名字，即核心的名字）

![](http://oduq3lfcc.bkt.clouddn.com/image/solr/add%20core.png)

4. 点击 Add Core, 添加完成后如图所示。

![](http://oduq3lfcc.bkt.clouddn.com/image/solr/addedcore.png)


## solrconfig.xml 配置
```xml
<luceneMatchVersion> 声明使用的lucene 的版本。
<lib> 配置solr 用到的 jar包，具体语法示例中基本都有了。
<dataDir>solrhome下core下的data 目录，可指定其它目录来存放所有索引数据。
<directoryFactory> 索引文件的类型，默认solr.NRTCachingDirectoryFactory，这个文件类型包装了 solr.StandardDirectoryFactory 和小文件内存缓存的类型，来提供 NRT（near-real-time近实时）搜索性能。。
<indexConfig> 主要索引相关配置：
  <lockType>文件锁的类型，默认 native，使用 NativeFSLockFactory 。
  <infoStream>为了调试， Lucene提供了这个参数，如果是 true的话， IndexWriter 会像设置的文件中写入 debug信息。
<jmx> 一般不需要设置具体可以查看 wiki文档http://wiki.apache.org/solr/SolrJmx
<updateHandler> 更新的Handler ，默认DirectUpdateHandler2，主要配置如下：
  <updateLog>
<strname="dir"> 配置更新日志的存放位置
   <autoCommit> 硬自动提交，可以配置 maxDocs即从上次提交后达到多少文档后会触发自动提交； maxTime时间限制； openSearcher ，如果设为false ，导致索引变化的最新提交，不需要重新打开 searcher就能看到这些变化，默认 false。
  <autoSoftCommit>软自动提交，与前面的 <autuCommit> 相似，但是它只是让这些变化能够看到，并不保证这些变化会同步到磁盘上。这种方法比硬提交要快，而且更接近实时更友好。
<query> 配置检索词相关参数以及缓存配置参数。
  <maxBooleanClauses>每个 BooleanQuery 中最大BooleanClauses 的数目，默认 1024。
  <filterCache> filterCache 存储了无序的lucenedocumentid 集合。为 IndexSearcher 使用，当一个IndexSearcher 打开时，可以被重新赋于原来的值，或者使用旧的 IndexSearcher 的值，例如使用 LRUCache时，最近被访问的 Items将被赋予 IndexSearcher 。solr 默认是 FastLRUCache 。
  <queryResultCache> 缓存查询的结果集的 docs的 id。
  <documentCache> 缓存 document对象，因为 document中的内部 id是 transient, 所以autowarmed 为0 ，不能被 autowarmed。
  <enableLazyFieldLoading> 保存的字段，如果不需要的话就懒加载，默认true。
  <queryResultWindowSize>queryResultCache 的一个参数。
  <queryResultMaxDocsCached> queryResultCache 的一个参数。
  <listener event"newSearcher"class="solr.QuerySenderListener">query 的事件监听器。
  <useColdSearcher>当一个检索请求到达时，如果现在没有注册的searcher，那么直接注册正在预热的 searcher并使用它。如果设为 false则所有请求都要 block，直到有 searcher完成预热。
  <maxWarmingSearchers>后台同步预热的 searchers数量。
<requestDispatcherhandleSelect="false"> solr接受请求后如何处理，推荐新手使用false
  <requestParsersenableRemoteStreaming="true" multipartUploadLimitInKB="2048000"formdataUploadLimitInKB="2048" /> 使系统能够接收远程流
  <httpCachingnever304="true">http cache 参数，solr 不输出任何 HTTPCaching相关的头信息。
<requestHandler> 接收请求，根据name值不同分发到不同的handler。
 "/select" 检索 SearchHandler
 "/query" 检索 SearchHandler
 "/get" RealTimeGetHandler
 "/browse" SearcherHandler
 "/update" UpdateRequestHandler
 "/update/json" JsonUpdateRequestHandler
 "/update/csv" CSVRequestHandler
 "/update/extract"ExtractingRequestHandler
 "/analysis/field" FieldAnalysisRequestHandler
 "/analysis/document"DocumentAnalysisRequestHandler
 "/admin/" AdminHandlers
 "/replication" 复制，要有主，有从
  <searchComponent> 注册searchComponent
  <queryResponseWriter> 返回数据
```

# 数据导入

从数据库导入数据至solr。在wtt_core/conf/下新建hexo-data-config.xml(名字可自定义)


```xml
<dataConfig>
    <dataSource type="JdbcDataSource" driver="com.mysql.jdbc.Driver" url="jdbc:mysql://wentuotuo.com:3306/test?useUnicode=true&amp;characterEncoding=UTF-8" user="hexo" password="iju_,sfe"/>
        <document name="documents">
        <entity name="documents" pk="id" query="SELECT * FROM Hexo" deltaQuery="select * from Hexo where date > '${dataimporter.last_index_time}'">

         <field column="ID" name="id" />
         <field column="title" name="title" />
         <field column="date" name="date" />
         <field column="slug" name="slug" />
         <field column="content" name="content" />

        </entity>
    </document>
</dataConfig>
```

在 solrconfig.xml 中，添加/修改 下面内容：

```xml
<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
     <lst name="defaults">
         <str name="config">hexo-data-config.xml</str>
     </lst>
</requestHandler>
```

在界面上wtt_core核心中点击Dataimport，execute导入后：

![](http://oduq3lfcc.bkt.clouddn.com/image/solr/dataimport.png)

# Error
## SolrCore Initialization Failures
- org.apache.solr.common.SolrException:org.apache.solr.common.SolrException: Error loading class 'org.apache.solr.handler.dataimport.DataImportHandler'

- 原因: 缺少包solr-dataimporthandler-X.X.X.jar这个jar包，默认在下载包的dist目录下
- 解决方法：在solrconfig.xml中增加下面一行
```xml
<lib dir="/home/ymhe/software/solr-6.4.1/dist" regex="solr-dataimporthandler-\d.*\.jar" />
```

# 参考资料

- [Solr Data Import 快速入门](http://blog.chenlb.com/2010/03/solr-data-import-quick-start.html)
