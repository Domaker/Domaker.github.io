---
layout: post
title:  "ElasticSearch入门"
date:   2017-10-14 13:25:35 +0200
tags: [php,es]
category: [Elastic Search]
---

> 我们建立一个网站或应用程序，并要添加搜索功能，但是想要完成搜索工作的创建是非常困难的。我们希望搜索解决方案要运行速度快，我们希望能有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP来索引数据，我们希望我们的搜索服务器始终可用，我们希望能够从一台开始并扩展到数百台，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。因此我们利用Elasticsearch来解决所有这些问题及可能出现的更多其它问题。

# 基本概念
## Node & Cluster
> Elastic 本质上是一个分布式数据库，允许多台服务器协同工作，每台服务器可以运行多个Elastic实例。

> 单个Elastic实例称为一个Node（节点）。

> 一组Node（节点）构成Cluster（集群）。

## Index

> Elastic会索引所有字段，经过处理后写入一个反向索引（Inverted Index）。

## Query DSL
> 分类：叶查询子句、复合查询子句

### 匹配查询
#### match
> 将文本或短语与一个或多个字段的值匹配

```
{
   "query":{
      "match" : {
         "city":"pune"
      }
   }
}
```

#### multi_match
> 将文本或短语与多个字段匹配

```
{
   "query":{
      "multi_match" : {
         "query": "hyderabad",
         "fields": [ "city", "state" ]
      }
   }
}
```

### 查询字符串查询
> 此查询使用查询解析器和```query_string```关键字

```
{
   "query":{
      "query_string":{
         "query":"good faculty"
      }
   }
}
```

```
{
   "took":16, "timed_out":false, "_shards":{"total":10, "successful":10, "failed":0}, 
   "hits":{
      "total":1, "max_score":0.09492774, "hits":[{
         "_index":"schools", "_type":"school", "_id":"2", "_score":0.09492774, 
         "_source":{
            "name":"Saint Paul School", "description":"ICSE Affiliation",
            "street":"Dawarka", "city":"Delhi", "state":"Delhi",
            "zip":"110075", "location":[28.5733056, 77.0122136],
            "fees":5000, "tags":["Good Faculty", "Great Sports"],
            "rating":"4.5" 
         }
      }]
   }
}
```

### 期限等级查询
> 这些查询主要处理结构化数据，如数字、日期和枚举

```
{
   "query":{
      "term":{"zip":"176115"}
   }
}
```

```
{
   "took":1, "timed_out":false, "_shards":{"total":10, "successful":10, "failed":0},
   "hits":{
      "total":1, "max_score":0.30685282, "hits":[{
         "_index":"schools", "_type":"school", "_id":"1", "_score":0.30685282,
         "_source":{
            "name":"Central School", "description":"CBSE Affiliation",
            "street":"Nagan", "city":"paprola", "state":"HP", "zip":"176115",
            "location":[31.8955385, 76.8380405], "fees":2200, 
            "tags":["Senior Secondary", "beautiful campus"], "rating":"3.3"
         }
      }]
   }
}
```

### 范围查询
> 此查询用于查找值的范围之间的值的对象，需要
- gte - 大于等于
- gt  - 大于
- lte - 小于等于
- lt  - 小于

```
{
   "query":{
      "range":{
         "rating":{
            "gte":3.5
         }
      }
   }
}
```

```
{
   "took":31, "timed_out":false, "_shards":{"total":10, "successful":10, "failed":0},
   "hits":{
      "total":3, "max_score":1.0, "hits":[
         {
            "_index":"schools", "_type":"school", "_id":"2", "_score":1.0,
            "_source":{
               "name":"Saint Paul School", "description":"ICSE Affiliation",
               "street":"Dawarka", "city":"Delhi", "state":"Delhi", 
               "zip":"110075", "location":[28.5733056, 77.0122136], "fees":5000, 
               "tags":["Good Faculty", "Great Sports"], "rating":"4.5"
            }
         }, 

         {
            "_index":"schools_gov", "_type":"school", "_id":"2", "_score":1.0, 
            "_source":{
               "name":"Government School", "description":"State Board Affiliation",
               "street":"Hinjewadi", "city":"Pune", "state":"MH", "zip":"411057",
               "location":[18.599752, 73.6821995] "fees":500, 
               "tags":["Great Sports"], "rating":"4"
            }
         },

         {
            "_index":"schools", "_type":"school", "_id":"3", "_score":1.0,
            "_source":{
               "name":"Crescent School", "description":"State Board Affiliation",
               "street":"Tonk Road", "city":"Jaipur", "state":"RJ", "zip":"176114", 
               "location":[26.8535922, 75.7923988], "fees":2500,
               "tags":["Well equipped labs"], "rating":"4.5"
            }
         }
      ]
   }
}
```
> 其他类型的期限级查询
 - 存在的查询 - 如果某一个字段有非空值
 - 缺失的查询 - 与存在的查询相反
 
### 复合查询
> 这些查询是通过使用如和，或，非和或等，用于不同索引或具有函数调用等的布尔运算符彼此合并的不同查询的集合。


