---
layout: post
title: 学习sql Part 2
categories: [数据库]
description: 学习sql Part 2，总结sql的基础知识，来源《sql必知必会》
keywords: 数据库, sql
---


<h2 align = "center"> 学习sql Part 2 </h2>

<br/>

## 7、创建计算字段

### 7.1 计算字段

存储在数据库表中的数据一般不是应用程序所需要的格式。我们需要**直接从数据库中检索出转换、计算或者格式化过的数据**，而不是检索出数据，然后从客户端应用程序中重新格式化。

这就是计算字段可以排上用场的地方了。计算字段并实际存在于数据库中，而是运行在SELECT语句内创建的。

**字段(field)**：基本与列的意思相同，但是数据库的列一般称为列，而字段通常用来描述计算字段。

### 7.2 拼接字段

**拼接**：将值联结在一起构成单个值；

MYSQL中需要用到Concat函数来联结：

```sql
SELECT Concat(列1, '(', 列2, ')')
From 表
```   

**去除空格**: RTRIM()[去掉字符串左边的空格]、TRIM()[去掉字符串两边的空格]、LTRIM()[去掉字符串左边的空格];                      

用到某一列或者计算字段上都可以

**使用别名**：上面生成的数据，没有名称，导致客户端应用是无法引用他的，这就需要为它设置一个别名。

别名（alias）是一个字段或者值的替换名，别名用AS关键词赋予。

```sql
SELECT Concat(列1, '(', 列2, ')') as 新列名
From 表
```  

### 7.3 执行算术计算

可以对SQL的字段进行加减乘数（+、-、*、/）

```sql
SELECT 列1 * 列2 as 新列名
From 表;
```  

### 7.4 测试计算

SELECT除了可以用来从表中索引数据，还可以用来简单地访问和处理表达式。

例如：

```sql
--计算200+100
SELECT 200+100；

--计算当前日期和时间
SELECT NOW();
```  

## 8、使用函数处理数据

### 8.1 函数

和大多数其他计算机语言一样，SQL也可以用来处理数据。函数一般是在数据上执行的，为数据的转换和处理提供了方便。

前面提到的TRIM()就是函数。值得注意的是，不同的DBMS中，函数的定义和用法都有差异。

### 8.2 文本处理函数

常用的文本处理函数：

|函数|说明|
|--|--|
|SUBSTRING(field, m, n)|提取字符串子串|
|LEFT(field, n)|返回field左边n个字符|
|RIGHT(field, n)|取field右边n个字符|
|LOWER(field)|field转换成小写|
|UPPER(field)|field转换成小写|
|LTRIM(field)|去掉field左边的空格|
|RTRIM(field)|去掉field右边的空格|
|TRIM(field)|去掉field两端的空格|
|LENGTH(field)|返回field字段的长度|

### 8.3 日期时间处理函数

日期和时间采用相应的数据类型存储在表中，每种DBMS都有自己的特殊形式。应用程序一般不适用日期和时间的存储格式，因此日期和时间函数总是用来读取、统计、处理这些值。

|函数|描述|
|--|--|
|NOW()|返回当前的日期和时间|
|CURDATE()|返回当前的日期|
|CURTIME()|返回当前的时间|
|DATE()|提取日期或日期/时间表达式的日期部分|
|EXTRACT()|返回日期/时间按的单独部分|
|DATE_ADD()|给日期添加指定的时间间隔|
|DATE_SUB()|从日期减去指定的时间间隔|
|DATEDIFF()|返回两个日期之间的天数|
|DATE_FORMAT(t)|用不同的格式显示日期/时间|
|YEAR(time) |返回time的年份|
|MONTH(time) |返回time的月份(范围是1到12)|
|DAY(time) |返回time的日期(范围是1到31)|
|HOUR(time) |返回time的小时(范围是0到24)|
|MINUTE(time) |返回time的分钟数(范围是0到59)|
|SECOND(time) |返回time的秒数(范围是0到59)|

具体的函数参考：<a href = "https://blog.csdn.net/u012373815/article/details/70008023">这里</a>

### 8.4 数值处理函数

|函数|描述|
|--|--|
|ABS()|绝对值|
|COS()|COS|
|EXp()|指数|
|PI()|圆周率|
|SIN()|SIN|
|SQRT()|开方|
|TAN()|正切|
|CEIL()|CEIL|
|FLOOR()|FLOOR|
|MOD()|取余数|
|RAND()|0-1的随机数|
|TRUNCATE()|截断为某位长度的小数|


具体的函数参考：<a href = "https://www.cnblogs.com/duhuo/p/4672667.html">这里</a>


## 9、聚集函数

|函数|描述|
|--|--|
|AVG()|均值|
|MAX()|最大值|
|MIN()|最小值|
|SUM()|求和|
|COUNT()|返回行数|

## 10、分组数据

### 10.1 数据分组

使用分组可以将数据分为多个逻辑组，然后对每个组进行聚集运算。

### 10.2 创建分组

分组是使用SELECT语句的GROUP BY 子句创建的，例子如下：

```sql
SELECT COUNT(*) AS NUM_PRODS
FROM PRODUCTS
GROUP BY VENDER_ID;
```

上面的sql语句计算每个不同的vender_id的数据条数

**GROUP BY的规定**：

* GROUP BY 子句可以包含任意数目的列，因而可以对分组进行嵌套，从而得到更细致地进行数据分组；
* 如果GROUP BY 中嵌套了分组，数据将在最后指定的分组上进行汇总。
* GROUP BY子句中列出的每一列都必须是检索列或者有效的表达式，不能是聚集函数。如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式，**不能使用别名**。
* 大多数SQL实现不允许GROUP BY 列带有长度可变的数据类型；
* 如果分组列中包含具有NULL值的列，则NULL将作为一个分组返回，如果列中有多行NULL值，他们将作为一组。
* GROUP BY 语句出现在where语句之后，ORDER BY 语句 之前。

### 10.3 过滤分组

SQL允许进行过滤分组，即规定包括哪些分组，排除哪些分组。注意WHERE是不能用来过滤分组的。

SQL用来过滤分组的子句是HAVING， HAVING 类似于WHERE，基本上可以代替WHERE。WHERE用来过滤行，而HAVING用来过滤分组。

HAVING继承了WHERE的所有功能。

HAVING 放在GROUP BY 语句之后。

### 10.4 分组和排序

GROUP BY 和 ORDER BY 一起使用。

### 10.5 SELECT 子句的顺序

selcet   要返回的列或表达式

from   从中检索数据的表

where    行级过滤

group by 分组说明

having 组级过滤

order by  输出排列顺序   ASC正序排列  DESC 倒序排列
