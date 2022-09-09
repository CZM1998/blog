---
title: MySQL知识概要
tags: [MySQL,笔记]
categories: 笔记
date: 2022/02/23 08:22:22
index_img: https://cdn.pixabay.com/photo/2016/03/04/12/20/server-1235959_960_720.jpg
---

作为世界上非常流行的关系型数据库，开发人员有必要掌握其基本知识和技能。这里简要总结了一些MySQL的知识和技巧。

<!--more-->

## 基本概念

* 数据库（database）：保存有组织的数据的容器。名字唯一。
* 表（table）：某种特定类型数据的结构化清单。同一数据库内名字唯一。
* 模式（schema）: 关于数据库和表的布局及特性的信息。
* 列（column）： 表中的一个字段。所有表都是由一个或多个列组成的。
* 数据类型（datatype）： 所容许的数据的类型。每个表列都有相应的数据类型，它限制（或容许）该列中存储的数据。
* 行（row）： 表中的一个记录。 数据按行存储，一行为一个记录。
* 主键（primary key）：一列（或一组列），其值能够唯一区分表中每个行。值（或值的组合）唯一且不为NULL，不能重复。
    - 不更新。
    - 不重用。
    - 不会变。
* SQL（Structured Query Language）：结构化查询语言。有统一标准，但每个厂商实现不同。部分语句通用。
* 关键字（key word）： 作为MySQL语言组成部分的一个保留字，不应该作为表、列的名称。如下：
  

|关|键|字|
|:----:|:----:|:----:|
|ACTION|CASE|DATABASE|
|ADD|CHANGE|DATABASES|
|ALL|CHAR|DATE|
|ALTER|CHARACTER|DAY_HOUR|
|ANALYZE|CHECK|DAY_MICROSECOND|
|AND|COLLATE|DAY_MINUTE|
|AS|COLUMN|DAY_SECOND|
|ASC|CONDITION|DEC|
|ASENSITIVE|CONNECTION|DECIMAL|
|BEFORE|CONSTRAINT|DECLARE|
|BETWEEN|CONTINUE|DEFAULT|
|BIGINT|CONVERT|DELAYED|
|BINARY|CREATE|DELETE|
|BIT|CROSS|DESC|
|BLOB|CURRENT_DATE|DESCRIBE|
|BOTH|CURRENT_TIME|DETERMINISTIC|
|BY|CURRENT_TIMESTAMP|DISTINCT|
|CALL|CURRENT_USER|DISTINCTROW|
|CASCADE|CURSOR|DIV|
|DOUBLE |HOUR_MINUTE |LINES|
|DROP |HOUR_SECOND |LOAD|
|DUAL |IF |LOCALTIME|
|EACH |IGNORE |LOCALTIMESTAMP|
|ELSE |IN |LOCK|
|ELSEIF |INDEX |LONG|
|ENCLOSED |INFILE |LONGBLOB|
|ENUM |INNER |LONGTEXT|
|ESCAPED |INOUT |LOOP|
|EXISTS |INSENSITIVE |LOW_PRIORITY|
|EXIT |INSERT |MATCH|
|EXPLAIN |INT |MEDIUMBLOB|
|FALSE |INTEGER |MEDIUMINT|
|FETCH |INTERVAL |MEDIUMTEXT|
|FLOAT |INTO |MIDDLEINT|
|FOR |IS |MINUTE_MICROSECOND|
|FORCE |ITERATE |MINUTE_SECOND|
|FOREIGN |JOIN |MOD|
|FORM |KEY |MODIFIES|
|FULLTEXT |KEYS |NATURAL|
|GOTO |KILL |NO|
|GRANT |LEADING |NO_WRITE_TO_BINLOG|
|GROUP |LEAVE |NOT|
|HAVING |LEFT |NULL|
|HIGH_PRIORITY |LIKE |NUMERIC|
|HOUR_MICROSECOND |LIMIT |ON|
|OPTIMIZE |RLIKE |THEN|
|OPTION |SCHEMA |TIME|
|OPTIONALLY |SCHEMAS |TIMESTAMP|
|OR |SECOND_MICROSECOND |TINYBLOB|
|ORDER |SELECT |TINYINT|
|OUT |SENSITIVE |TINYTEXT|
|OUTER |SEPARATOR |TO|
|OUTFILE |SET |TRAILING|
|PRECISION |SHOW |TRIGGER|
|PRIMARY |SMALLINT |TRUE|
|PROCEDURE |SONAME |UNDO|
|PURGE |SPATIAL |UNION|
|READ |SPECIFIC |UNIQUE|
|READS |SQL |UNLOCK|
|REAL |SQL_BIG_RESULT |UNSIGNED|
|REFERENCES |SQL_CALC_FOUND_ROWS |UPDATE|
|REGEXP |SQL_SMALL_RESULT |USAGE|
|RELEASE |SQLEXCEPTION |USE|
|RENAME |SQLSTATE |USING|
|REPEAT |SQLWARNING |UTC_DATE|
|REQUIRE |STARTING |UTC_TIMESTAMP|
|RESTRICT |STRAIGHT_JOIN |VALUES|
|RETURN |TABLE |VARBINARY|
|REVOKE |TERMINATED |VARCHAR|
|RIGHT |TEXT |VARCHARACTER|
|VARYING |WHILE |XOR|
|WHEN |WITH |YEAR_MONTH|
|WHERE |WRITE |ZEROFILL|

## 连接MySQL和查看信息

* 登录MySQL：`mysql -h 主机ip -u 用户名 -p`
* 选择数据库：`USE 数据库名;`
* 列出信息类：
    - 数据库：`SHOW DATABASES;`
    - 表：`SHOW TABLES;`
    - 表列：`SHOW COLUMNS FROM 表名;` 或 `DESCRIBE 表名;`
    - 服务器：`SHOW STATUS;`
    - 创建数据库和表的SQL代码：`SHOW CREATE DATABASE 数据库名;` 或 `SHOW CREATE TABLE 表名;`
    - 用户权限：`SHOW GRANTS;`
    - 错误和警告：`SHOW ERRORS;` 和 `SHOW WARNINGS;`

## 基本检索数据

* 查询部分列：`SELECT 列名1, ... , 列名n FROM 表名;`
* 查询所有列：`SELECT * FROM 表名;`
* 去重：`SELECT DISTINCT 列名 FROM 表名;`，有多个列名，则作用于所有列。
* 有限行：`SELECT * FROM 表名 LIMIT 行数;` 和 `SELECT * FROM 表名 LIMIT 开始行,行数;` 和 `SELECT * FROM 表名 LIMIT 行数 OFFSET 开始行;`，行从0开始计数，查询结果包含开始行。
* 完全限定名称：`SELECT 表名.列名 FROM 数据库名.表名;`，解决重名问题。

## 排序检索数据

* 单个列排序：`SELECT * FROM 表名 ORDER BY 列名;`
* 多个列排序：`SELECT * FROM 表名 ORDER BY 列名1, ... , 列名n;`，按列名出现顺序决定排序效果。
* 升序和降序：`SELECT * FROM 表名 ORDER BY 列名 ASC;` 和 `SELECT * FROM 表名 ORDER BY 列名 DESC;`，ASC和DESC只作用于其前面的一个列名。
* 多个降序：`SELECT * FROM 表名 ORDER BY 列名1 DESC, ... , 列名n DESC;`，ASC为默认排序方式。

## 过滤检索数据

* 按条件过滤检索结果：`SELECT * FROM 表名 WHERE 条件表达式;`
* 基本条件表达式：
    - `列名 = 值`：等于
    - `列名 <> 值` 和 `列名 != 值`：不等于
    - `列名 < 值` 或 `列名 <= 值`：小于 或 小于等于
    - `列名 > 值` 或 `列名 >= 值`：大于 或 大于等于
    - `列名 BETWEEN 值1 ADN 值2`：介于二者之间，包含两端
    - `列名 IS NULL`：判空
* 复合条件查询：
    - `条件表达式 AND 条件表达式`：两条件同时成立
    - `条件表达式 OR 条件表达式`：两条件至少一个成立
    - `条件表达式 IN (值1, ... , 值n)`：满足任意一个
    - `NOT 条件表达式`：否定
* 各操作符优先级：
  
|优先级|操作符|
|:---:|:---:|
|1|=(赋值运算）、:=|
|2|II、OR|
|3|XOR|
|4|&&、AND|
|5|NOT|
|6|BETWEEN、CASE、WHEN、THEN、ELSE|
|7|=(比较运算）、<=>、>=、>、<=、<、<>、!=、 IS、LIKE、REGEXP、IN|
|8|\||
|9|&|
|10|<<、>>|
|11|-(减号）、+|
|12|*、/、%|
|13|^|
|14|-(负号）、〜（位反转）|
|15|!|

* 通配符查询：
    - 单个字符：`SELECT * FROM 表名 WHERE 列名 LIKE '字符串_字符串';`，`_`表示这个位置可被任意一个字符体替代。`_`可以出现在任意位置。
    - 多个字符：`SELECT * FROM 表名 WHERE 列名 LIKE '字符串%字符串';`，`%`表示这个位置可以被任意多个任意字符替代。`%`可以出现在任意位置。
    - 慎用通配符查询，这可能会带来比较大的开销和错误。

* 正则表达式查询：`SELECT * FROM 表名 WHERE 列名 REGEXP '正则表达式'`
    - 使用`\\`进行转义
    - 预定义的字符集：

|字符集|说明|
|:---:|:---:|
|`[:alnum:]`|任意英文字母数字，等价于`[a-zA-Z0-9]`|
|`[:alpha:]`|任意英文字母，等价于`[a-zA-Z]`|
|`[:lower:]`|任意小写字母，等价于`[a-z]`|
|`[:upper:]`|任意大写字母，等价于`[A-Z]`|
|`[:digit:]`|任意数字，等价于`[0-9]`|
|`[:xdigit:]`|任意十六进制数字，等价于`[a-fA-F0-9]`|
|`[:space:]`|任意空白字符，等价于`[\\f\\n\\r\\t\\v]`|
|`[:blank:]`|任意空格和制表符，等价于`[\\t]`|
|`[:cntrl:]`|任意ASCII控制字符，等价于ASCII 0到31和127|
|`[:print:]`|任意可打印字符|
|`[:graph:]`|等价于不含空格的`[:print:]`|
|`[:punct:]`|任意不属于`[:alnum:]`和`[:cntrl:]`的字符|

更多正则表达式写法见[《正则表达式必知必会》](https://zh.1lib.ma/book/3628947/355484)

## MySQL算数计算和函数

* 别名：`SELECT 列名 AS 别名 FROM 表名;`，用别名来指代该列。
* 算数计算：`SELECT 列名1 算术运算 列名2 FROM 别名;`，算数运算有：`+-*/`
* MySQL常用函数汇总：[MySQL函数](https://www.cnblogs.com/kissdodog/p/4168721.html)



