---
title: '@MySQL语法之 Indexes'
comments: true
categories: 数据库
abbrlink: b1ae4170
date: 2017-08-06 19:21:59
updated: 2018-01-18 03:09:31
tags: mysql
keywords:
description:
---


用来加快查询的技术很多，其中最重要的是索引。通常索引能够快速提高查询速度。如果不使用索引，MYSQL必须从第一条记录开始然后读完整个表直到找出相关的行。表越大，花费的时间越多。但也不全是这样。本文讨论在 MySQL 中如何创建、删除、重命名索引。


## 创建索引

在执行 `CREATE TABLE` 语句时可以创建索引，也可以单独用 `CREATE INDEX` 或 `ALTER TABLE` 来为表增加索引。

> 例1 CREATE TABLE: 在创建表时就创建索引

```mysql
CREATE TABLE contacts
( 
    contact_id INT(11) NOT NULL AUTO_INCREMENT,
    last_name VARCHAR(30) NOT NULL,
    first_name VARCHAR(25),
    birthday DATE,
    CONSTRAINT contacts_pk PRIMARY KEY (contact_id),
    INDEX contacts_idx (last_name, first_name)
);
```

> 例2 CREATE INDEX: 创建完表后，单独创建索引

```mysql
CREATE TABLE contacts
( 
    contact_id INT(11) NOT NULL AUTO_INCREMENT,
    last_name VARCHAR(30) NOT NULL,
    first_name VARCHAR(25),
    birthday DATE,
    CONSTRAINT contacts_pk PRIMARY KEY (contact_id)
);

CREATE INDEX contacts_idx ON contacts (last_name, first_name);
```

> 例3 ALTER TABLE: 创建完表后，添加索引

```mysql
CREATE TABLE contacts
( 
    contact_id INT(11) NOT NULL AUTO_INCREMENT,
    last_name VARCHAR(30) NOT NULL,
    first_name VARCHAR(25),
    birthday DATE,
    CONSTRAINT contacts_pk PRIMARY KEY (contact_id)
);

ALTER TABLE contacts ADD INDEX contacts_idx (last_name, first_name)

```

## 删除索引

可利用 `ALTER TABLE` 或 `DROP INDEX` 语句来删除索引。类似于 `CREATE INDEX` 语句，`DROP INDEX` 可以在 `ALTER TABLE` 内部作为一条语句处理。

> 例1 DROP INDEX

```mysql
DROP INDEX contacts_idx ON contacts;
```

> 例2 ALTER TABLE 中使用 DROP INDEX 语句

```mysql
ALTER TABLE contacts DROP INDEX contacts_idx 
```

## 重命名索引

> MySQL5.6及其之前的版本，重命名索引，需要先删除之前的索引

```mysql
ALTER TABLE contacts
    DROP INDEX contacts_idx,
    ADD INDEX contacts_new_index (last_name, first_name);
```

> MySQL5.7及其之后的版本，使用关键字 `RENAME` 关键字即可重命名索引

```mysql
ALTER TABLE contacts
    RENAME INDEX contacts_idx TO contacts_new_index;
```

## 参考
1. [MySQL: indexes](https://www.techonthenet.com/mysql/indexes.php)

