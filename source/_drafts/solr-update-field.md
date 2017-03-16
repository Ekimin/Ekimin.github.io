---
title: 修改solr中数据
date:
tags:
categories:
layout: 
updated:
comments:
permalink:
---


<!--more-->

```java
    public static SolrInputDocument getSolrInputDoc(String serialNo, String repStatus) {
		Map<String, String> repStatusMap = new HashMap<String, String>();
		repStatusMap.put("set", repStatus);
		SolrInputDocument solrInputDoc = new SolrInputDocument();
		solrInputDoc.addField("serialno", serialNo);
		solrInputDoc.addField("repstatus", repStatusMap);

		// ARE.getLog().info("更新solr,solrInputDoc ：serialno=" + serialNo + "
		// repstatus=" + repStatus);
		return solrInputDoc;
	}
```

```java
// 同步solr数据的程序，更新其标志位。
	public void updateSolrStatus(Map<String, TextModel> serialNoTextModelMap) {
		String solrHost = ARE.getProperty("SolrService");
		SolrServer solrServer = new HttpSolrServer(solrHost);
		int batchCount = 0;
		// 遍历重复结果Map
		for (Map.Entry<String, TextModel> entry : serialNoTextModelMap.entrySet()) {
			String serialNo = entry.getKey();
			TextModel textModel = entry.getValue();
			String repStatus = textModel.getStatus();
			if (repStatus == null) {
				continue;
			}
			SolrInputDocument solrInputDocument = getSolrInputDoc(serialNo, repStatus);

			try {
				solrServer.add(solrInputDocument);
				batchCount++;
				if (batchCount >= 1000) {
					solrServer.commit();
					batchCount = 0;
				}
			} catch (SolrServerException e) {
				ARE.getLog().error("同步solr重复状态出错（cSolr异常）", e);
				e.printStackTrace();
			} catch (IOException e) {
				ARE.getLog().error("同步solr重复状态出错(IO异常)", e);
				e.printStackTrace();
			}
		}
		try {
			solrServer.commit();
			batchCount = 0;
		} catch (SolrServerException e) {
			ARE.getLog().error("同步solr重复状态出错（Solr异常）", e);
			e.printStackTrace();
		} catch (IOException e) {
			ARE.getLog().error("同步solr重复状态出错(IO异常)", e);
			e.printStackTrace();
		}
	}
```
