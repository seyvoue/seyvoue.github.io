---
title: '@MySQL语法之JOIN'
comments: true
categories: languages
tags: mysql
abbrlink: bd53954e
date: 2017-07-30 23:36:09
updated: 2018-01-18 03:08:17
keywords:
description:
---

如果数据存储在多个表中，如何用一条 `SELECT` 语句就检索出数据呢？—— 使用联结(join)  

通过一个简单的例子理解 mysql 中的：  

- INNER JOIN  
- LETF OUTTER JOIN (or sometimes called LEFT JOIN)  
- RIGHT OUTTER JOIN (or sometimes called RIGHT JOIN)  


---


__案例：__  现有两张表 `suppliers`、`orders`，两表通过字段 `supplier_id` 关联。

- suppliers table

| supplier_id |  supplier_name  |
|-------------|-----------------|
|       10000 | IBM             |
|       10001 | Hewlett Packard |
|       10002 | Microsoft       |
|       10003 | NVIDIA          |

- orders table

| order_id | supplier_id | order_date |
|----------|-------------|------------|
|   500125 |       10000 | 2017/05/12 |
|   500126 |       10001 | 2017/05/13 |
|   500127 |       10004 | 2017/05/14 |



## INNER JOIN

使用 `INNER JOIN` 对于两张关联的表 table1 和 table2 进行查询时，返回的是两张表的交集，即 $table1 \bigcap table2$，如下图：

![img1-c300](https://lh3.googleusercontent.com/-Q3dlPsKE4vA/WXI_QBQxJNI/AAAAAAAAB2s/JZ828EK1M1YIn9H9dYbDM8tkjOQMruw8ACHMYCw/I/Screen%2BShot%2B2017-07-22%2Bat%2B01.05.47.png)

> 对于案例中的 table 执行下面的 INNER JOIN 语句

```sql
SELECT suppliers.supplier_id, suppliers.supplier_name, orders.order_date
FROM suppliers
INNER JOIN orders
ON suppliers.supplier_id = orders.supplier_id;
```

> 执行结果：

| supplier_id |  supplier_name  | order_date |
|-------------|-----------------|------------|
|       10000 | IBM             | 2017/05/12 |
|       10001 | Hewlett Packard | 2017/05/13 |


## LEFT OUTTER JOIN

使用 `LEFT OUTTER JOIN` 对于两张关联的表 table1 和 table2 进行查询时，返回的是 $table1 \bigcap table2$ 以及 $table1 - table2$，如下图：

![img2-c300](https://lh3.googleusercontent.com/-vh4OIdNl0y4/WXI_QVWlIPI/AAAAAAAAB2w/3Grls5PgRgcZoOfKznG46P4KMD7TtPQpgCHMYCw/I/15006583395939.jpg)


> 对于案例中的 table 执行下面的 LEFT OUTTER JOIN 语句

```sql
SELECT suppliers.supplier_id, suppliers.supplier_name, orders.order_date
FROM suppliers
LEFT JOIN orders
ON suppliers.supplier_id = orders.supplier_id;
```

> 执行结果：

| supplier_id |  supplier_name  | order_date |
|-------------|-----------------|------------|
|       10000 | IBM             | 2017/05/12 |
|       10001 | Hewlett Packard | 2017/05/13 |
|       10002 | MICROSOFT       | <null>     |
|       10003 | NVIDIA          | <null>     |


## RIGHT OUTTER JOIN

使用 `LEFT OUTTER JOIN` 对于两张关联的表 table1 和 table2 进行查询时，返回的是 $table1 \bigcap table2$ 以及 $table2 - table1$，如下图：

![img3-c300](https://lh3.googleusercontent.com/-9AAm-QfV2lU/WXI_Qg7_ELI/AAAAAAAAB20/qNUBCk3cMeQ_notW-EmYA9ukR7ZeQfNFgCHMYCw/I/15006584390571.jpg)


> 对于案例中的 table 执行下面的 LEFT OUTTER JOIN 语句

```sql
SELECT orders.order_id, orders.order_date, suppliers.supplier_name, orders.supplier_id
FROM suppliers
RIGHT JOIN orders
ON suppliers.supplier_id = orders.supplier_id;
```

> 执行结果：

| order_id | order_date |  supplier_name  | supplier_id |
|----------|------------|-----------------|-------------|
|   500125 | 2017/05/12 | IBM             |       10000 |
|   500126 | 2017/05/13 | Hewlett Packard |       10001 |
|   500127 | 2017/05/14 | <null>          |       10004 |

结果表明，对于 `supplier_id=10004`，虽然不满足 `suppliers.supplier_id = orders.supplier_id`，但由于语句中用的是 `RIGHT JOIN` 且该条记录是在 `orders` 表中，所以还是会被返回。

---

## 参考
1. [MySQL: JOIN](https://www.techonthenet.com/mysql/joins.php)
2. [Mysql Join语法解析与性能分析](http://www.cnblogs.com/BeginMan/p/3754322.html)

