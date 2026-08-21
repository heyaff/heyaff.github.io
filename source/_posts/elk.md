---
title: ELK查询语句
date: 2024-05-17 18:50:41
categories:
  - ELK
tags:
  - ELK
---


在kibana管理Web页面中，Dev Tools工具，写SQL语句查看统计结果，格式如下

```
POST /_sql?format=txt
{
"query": "SELECT 日志字段1,日志字段2 FROM index_索引表名 WHERE 日志字段3 = 'XXX' AND 时间字段 >= '2024-05-16' AND 时间字段 <= '2024-05-17' "
}

```
<!--more-->


例如，查询特定时间内访问blog.csdn.net的源IP次数统计
```
POST /_sql?format=txt
{
"query": "SELECT network_source_ip, COUNT(*) AS Num FROM net_flow_log WHERE log_product = 'nta_wb_cn' AND log_time >= '2024-05-16T10:00:00.000Z' AND log_time <= '2024-05-17T10:01:00.000Z' AND data = 'blog.csdn.net' GROUP BY network_source_ip ORDER BY Num DESC  LIMIT 100"
}
```





