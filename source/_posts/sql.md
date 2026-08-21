title: SQL语句
date: 2021-01-04
categories:
  - SQL
tags:
  - 统计报表
---


### [SQL计算累计和 sum() over](https://www.cnblogs.com/mingdashu/p/12100734.html)
```sql
累计求和sum() over(order by 其他列名1,其他列名2...)

sum(计数的列名) over()			统计所有行数，计算求和

sum(计数的列名) over(order by 列名1)	根据列名1，order by排序后的总和
```
<!--more-->

### 一个SQL片段
with *别名* as (select * from table)

### 多个片段
with *别名1* as (
	xxxx
),
*别名2* as(
	xxxx
)

### 时间/日期
```
+ interval '1 day'			----加减1天

列名::timestamp				----转成timestamp格式
CAST(列名 as timestamp)			----转成timestamp格式

date_trunc('week',列名)			----列名类型timestamp，返回每年/每月的第一天，每周的星期一

SELECT date_trunc('week',create_time),count(列名) FROM 表名 GROUP BY date_trunc('week',create_time)		----统计每周的数据量


to_char(列名,'YYYY-MM-DD')

```

### 拼接字段

CONCAT('工号为:',列名,'的员工',count(xxx))

