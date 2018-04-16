---
layout: post
title: 学习sql Part 3
categories: [数据库]
description: 学习sql Part 3，总结sql的基础知识，来源《sql必知必会》
keywords: 数据库, sql
---


<h2 align = "center"> 学习sql Part 3 </h2>

<br/>

## 11、使用子查询

### 11.1 子查询

我们迄今为止看到的查询都是简单查询，即从单个数据库表中检索数据的单条语句。

SQL还允许创建子查询，即嵌套在其他查询中的查询。

### 11.2 利用子查询进行过滤

```sql
SELECT * FROM A WHERE field1 in (SELECT field1 FROM B where field2 = v);
```

后面嵌套的子句起到了过滤的作用。执行时是先执行子查询

### 11.3 作为计算字段使用子查询

```sql
SELECT CUST_NAME, CUST_STATE,
(SELECT COUNT(*) FROM ORDERS WHERE ORDERS.CUST_ID = CUSTOMERS.CUST_ID) AS ORDERS
FROM CUSTOMERS
ORDER BY CUST_NAME;
```

显然这里的子查询起到的是计算字段的作用，可以认为每次查询是都会执行一次子查询。

注意上面使用到了完全限定列名来防止歧义。

## 12、联结表
