---
title: Java实现Jaccard算法去重
tags:
  - Java
  - Jaccard
  - 算法
categories: Java
date: 2016-04-09 19:25:34
updated:
comments:
permalink:
---


最近项目需求，需要对大量大文本进行去重，研究了一下相关算法，最后确定在Jaccard算法上，这里记录下实现过程。

<!--more-->

# Jaccard 算法

算法相关简介网上已经有很多介绍了，这里推荐一篇个人认为写得还不错的：[传送门](http://blog.csdn.net/ygrx/article/details/12748857)

# 分词

关于分词也有很多选择，我这里选择的是HanLP的分词，因为对中文的支持做的比较好。

[HanLP官网](http://hanlp.linrunsoft.com/)

# 倒排索引

记得在研究生语义分析课的时候学习过倒排索引的概念,当时并没有重视，但是实际运用中才发现神奇，倒排过后查询效率提升一个台阶。

# 代码

```java
package com.amarsoft.proj.jaccard;

import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import java.util.HashMap;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;

import org.apache.solr.client.solrj.SolrServer;
import org.apache.solr.client.solrj.SolrServerException;
import org.apache.solr.client.solrj.impl.HttpSolrServer;
import org.apache.solr.common.SolrInputDocument;

import com.amarsoft.are.ARE;
import com.amarsoft.proj.struct.TextModel;

public class ClearText {
/**
	 * 去重
	 * 
	 * @param serialNoTextModelMap
	 *            待去重Map
	 */
	public static Map<String, TextModel> clearNewText(Map<String, TextModel> serialNoTextModelMap) {
		Map<String, TextModel> repTextModelMap = new HashMap<String, TextModel>();
		// 对待去重文章Map进行排序
		List<Map.Entry<String, TextModel>> sortedSerialNoTextMapEntry = new CommonMethod()
				.sortByWordsCount(serialNoTextModelMap, 2);
		Map<String, List<TextModel>> wordsTextListMap = new HashMap<String, List<TextModel>>();// 存储<分词,有该分词的所有文章模型列表>的Map(倒排列表)
		for (Map.Entry<String, TextModel> entry : sortedSerialNoTextMapEntry) { // 遍历排序后的Map
			String serialNo = entry.getKey();// 文章序列号
			String[] wordsSet = null;
			wordsSet = HanLPCutWords.getWords(entry.getValue().getText());// 从数据库获取分词

			Map<String, Integer> currentWordCountMap = new HashMap<String, Integer>();// 当前文章每个分词出现的次数
			Map<String, Integer> serialNoDupCountMap = new HashMap<String, Integer>();// 存储与该文章的分词重复的文章的serialno和分词重复次数的Map
			// 遍历分词数组			
			for (String word : wordsSet) {
				// 该文章当前该分词数量+1				
				if (currentWordCountMap.containsKey(word)) {
					int cCount = currentWordCountMap.get(word);
					cCount++;
					currentWordCountMap.put(word, cCount);
				} else {
					currentWordCountMap.put(word, 1);
				}

				if (wordsTextListMap.containsKey(word)) {// 分词已经存在于wordsTextListMap中了
					
					// 根据重复的分词所属的文章，将重复分词数记录到serialNoDupCountMap
					List<TextModel> textModelList = wordsTextListMap.get(word);// 取得该分词对应的文章模型列表
					for (TextModel textModel : textModelList) {
						String dupSerialNo = textModel.getSerialNo();// 与该分词有重复的文章的serialno
						
						if (entry.getKey().equals(dupSerialNo)) {
							continue;// 跳过自己
						}						
						// 跳过word数量小于当前遍历文章word数的文章。
//						if (textModel.getCurrentWordCount() < currentWordCountMap.get(word)) {
//							continue;
//						}
						// 将所有包含该分词的文章对应的重复次数+1
						if (serialNoDupCountMap.containsKey(dupSerialNo)) {
							int dupCount = serialNoDupCountMap.get(dupSerialNo);
							dupCount++;
							serialNoDupCountMap.put(dupSerialNo, dupCount);
							
						} else {
							serialNoDupCountMap.put(dupSerialNo, 1);
						}
					}
					// 将本次遍历的文章加入到wordsTextListMap
					List<TextModel> tModelList = wordsTextListMap.get(word);
					if (!tModelList.contains(entry.getValue())) {
						TextModel tModel = entry.getValue();
						tModel.setCurrentWordCount(tModel.getCurrentWordCount() + 1);
						tModelList.add(tModel);
						wordsTextListMap.put(word, tModelList);
					}

				} else {// 分词不存在,新增
					List<TextModel> tModelList = new LinkedList<TextModel>();
					TextModel tModel = entry.getValue();
					tModel.setCurrentWordCount(1);
					tModelList.add(tModel);
					wordsTextListMap.put(word, tModelList);
				}
			} // 遍历分词数组结束,此时已经处理完该文章对应的所有分词

			// 去重，serialNoDupCountMap此时存放了之前遍历过的文章与本次遍历的文章分词重复的个数,遍历它，计算重复分词数占较少分词数文章的比例，标记超过阈值的任务文章
			int currentWordsCount = entry.getValue().getWordCount();// 当前遍历的文章的分词数（因为已经倒序过了，所以是目前最少的）
			// 遍历
			for (Map.Entry<String, Integer> dealRepEntry : serialNoDupCountMap.entrySet()) {
				// 若比例超过阈值，判定为重复文章，此时将较短（即本次遍历的）文章标记为重复，并记录它与谁重复(RID)
				double rate = dealRepEntry.getValue() * 1.0 / currentWordsCount;// 重复比例
				if (rate > JaccardTextSimilarity.ACCURACY) {
					String RID = dealRepEntry.getKey();
					if (RID.equals(serialNo)) {
						continue;
					}
					TextModel repTextModel = serialNoTextModelMap.get(serialNo);
					String repStatus = "-2";// 算法重复标志 -2
					repTextModel.setStatus(repStatus);
					repTextModel.setRID(RID);
					//debug:
					ARE.getLog().info("发现重复文章：" + serialNo + "和" + RID);
					
					// 将重复的textModel存入repTextModelMap.
					repTextModelMap.put(serialNo, repTextModel);					
				}
			}
		} // 遍历排序后的Map结束
		return repTextModelMap;
	}
}
```