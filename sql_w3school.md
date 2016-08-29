[TOC]

## SQL 基础教程

### 什么是 SQL？

SQL 指结构化查询语言

SQL 使我们有能力访问数据库

SQL 是一种 ANSI 的标准计算机语言

### RDBMS

RDBMS 指的是关系型数据库管理系统。

RDBMS 是 SQL 的基础，同样也是所有现代数据库系统的基础，比如 MS SQL Server, IBM DB2, Oracle, MySQL 以及 Microsoft Access。

RDBMS 中的数据存储在被称为表（tables）的数据库对象中。

表是相关的数据项的集合，它由列和行组成。

### SQL 语句后面的分号

某些数据库系统要求在每条 SQL 命令的末端使用分号。在我们的教程中不使用分号。
分号是在数据库系统中分隔每条 SQL 

语句的标准方法，这样就可以在对服务器的相同请求中执行一条以上的语句。

如果您使用的是 MS Access 和 SQL Server 2000，则不必在每条 SQL 语句之后使用分号，不过某些数据库软件要求必须使用分号。

### SQL DML 和 DDL

可以把 SQL 分为两个部分：数据操作语言 (DML) 和 数据定义语言 (DDL)。

SQL (结构化查询语言)是用于执行查询的语法。但是 SQL 语言也包含用于更新、插入和删除记录的语法。

查询和更新指令构成了 SQL 的 DML 部分：

+ SELECT - 从数据库表中获取数据
+ UPDATE - 更新数据库表中的数据
+ DELETE - 从数据库表中删除数据
+ INSERT INTO - 向数据库表中插入数据

SQL 的数据定义语言 (DDL) 部分使我们有能力创建或删除表格。我们也可以定义索引（键），规定表之间的链接，以及施加表间的约束。

SQL 中最重要的 DDL 语句:

+ CREATE DATABASE - 创建新数据库
+ ALTER DATABASE - 修改数据库
+ CREATE TABLE - 创建新表
+ ALTER TABLE - 变更（改变）数据库表
+ DROP TABLE - 删除表
+ CREATE INDEX - 创建索引（搜索键）
+ DROP INDEX - 删除索引

### SQL SELECT

语法：

```sql
SELECT 列名称 FROM 表名称
```

例子：

"Persons" 表

| Id | LastName | FirstName | Address        | City     |
|:--:|----------|-----------|----------------|----------|
| 1  | Adams    | John      | Oxford Street  | London   |
| 2  | Bush     | George    | Fifth Avenue   | New York |
| 3  | Carter   | Thomas    | Changan Street | Beijing  |

```sql
select LastName, FirstName from Person
```

结果：

| LastName | FirstName |
|----------|-----------|
| Adams    | John      |
| Bush     | George    |
| Carter   | Thomas    |


由 SQL 查询程序获得的结果被存放在一个结果集中。大多数数据库软件系统都允许使用编程函数在结果集中进行导航，比如：Move-To-First-Record、Get-Record-Content、Move-To-Next-Record 等等。

### SQL SELECT DISTINCT

在表中，可能会包含重复值。这并不成问题，不过，有时您也许希望仅仅列出不同（distinct）的值。关键词 DISTINCT 用于返回唯一不同的值。

语法：
```sql
SELECT DISTINCT 列名称 FROM 表名称
```

例子：

"Orders"表：

| Company  | OrderNumber |
|----------|-------------|
| IBM      | 3532        |
| W3School | 2356        |
| Apple    | 4698        |
| W3School | 6953        |

```sql
select distinct Company from Orders 
```

输出：

| Company  |
|----------|
| IBM      |
| W3School |
| Apple    |

### SQL WHERE

如需有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句。

语法规则

```sql
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```

| 操作符  | 描述         |
|---------|--------------|
| =       | 等于         |
| <>(!=)  | 不等于       |
| >       | 大于         |
| <       | 小于         |
| >=      | 大于等于     |
| <=      | 小于等于     |
| BETWEEN | 在某个范围内 |
| LIKE    | 搜索某种模式 |

例子：

"Persons" 表

| LastName | FirstName | Address        | City     | Year |
|----------|-----------|----------------|----------|------|
| Adams    | John      | Oxford Street  | London   | 1970 |
| Bush     | George    | Fifth Avenue   | New York | 1975 |
| Carter   | Thomas    | Changan Street | Beijing  | 1980 |
| Gates    | Bill      | Xuanwumen 10   | Beijing  | 1985 |

```sql
select * from Person where City = 'Beijing'
```

结果：

| LastName | FirstName | Address        | City    | Year |
|----------|-----------|----------------|---------|------|
| Carter   | Thomas    | Changan Street | Beijing | 1980 |
| Gates    | Bill      | Xuanwumen 10   | Beijing | 1985 |

注意：字符串需要用单引号环绕。

### SQL AND & OR

AND 和 OR 可在 WHERE 子语句中把两个或多个条件结合起来。如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。

语法规则：

```sql
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'
SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'
```

### SQL ORDER BY

ORDER BY 语句用于根据指定的列对结果集进行排序。

ORDER BY 语句默认按照升序对记录进行排序。

如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。

例子：

Orders 表:

| Company  | OrderNumber |
|----------|-------------|
| IBM      | 3532        |
| W3School | 2356        |
| Apple    | 4698        |
| W3School | 6953        |

```sql
-- 按 Company 升序排列
SELECT * FROM Orders ORDER BY Company 

-- 按 Company, OrderNumber 升序排列
SELECT * FROM Orders ORDER BY Company, OrderNumber 

-- 按 Company 降序排列
SELECT * FROM Orders ORDER BY Company DESC 

-- 按 Company 降序, OrderNumber 升序 排列
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC 
```

结果自行分析

### SQL INSERT INTO

INSERT INTO 语句用于向表格中插入新的行。

语法规则：

```sql
INSERT INTO 表名称 VALUES (值1, 值2,....)

INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

例子：

"Persons" 表：

| LastName | FirstName | Address        | City    |
|----------|-----------|----------------|---------|
| Carter   | Thomas    | Changan Street | Beijing |

```sql
INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing');
INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees');
```

结果：

| LastName | FirstName | Address        | City    |
|----------|-----------|----------------|---------|
| Carter   | Thomas    | Changan Street | Beijing |
| Gates    | Bill      | Xuanwumen 10   | Beijing |
| Wilson   |           | Champs-Elysees |         |

### SQL UPDATE 

Update 语句用于修改表中的数据。

语法规则

```sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

例子：

```sql
-- 更新一个值
UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' 

-- 更新多个值
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'
WHERE LastName = 'Wilson'
```

### SQL DELETE

DELETE 语句用于删除表中的行。

语法规则

```sql
DELETE FROM 表名称 WHERE 列名称 = 值

-- 删除所用数据（不删除表）
DELETE FROM 表名称;
DELETE * FROM 表名称;

```

## SQL 高级教程

### SQL TOP

TOP 子句用于规定要返回的记录的数目。对于拥有数千条记录的大型表来说，TOP 子句是非常有用的。注释：并非所有的数据库系统都支持 TOP 子句。类似功能的子句有 LIMIT

SQL Server 语法：
```sql
-- 选择 number 条记录
SELECT TOP number column_name(s) FROM 表名称

-- 选择总数的 number% 条记录
SELECT TOP number PERCENT column_name(s) FROM 表名称
```

Oracle 语法：
```sql
SELECT column_name(s) FROM table_name WHERE ROWNUM <= number
```

MySQL 语法：
```sql
SELECT column_name(s) FROM table_name LIMIT number
```

### SQL LIKE

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

语法规则：

```sql
SELECT column_name(s)
FROM 表名称
WHERE column_name LIKE pattern
```

在搜索数据库中的数据时，SQL 通配符可以替代一个或多个字符。SQL 通配符必须与 LIKE 运算符一起使用。

通配符：

+ `_` ：与任意单字符匹配
+ `%``：与包含一个或多个字符的字符串匹配
+ `[ ]`：与特定范围（例如，[a-f]）或特定集（例如，[abcdef]）中的任意单字符匹配。
+ `[^]` 或者 `[!]`：与特定范围（例如，[^a-f]）或特定集（例如，[^abcdef]）之外的任意单字符匹配。

例子（[出处](http://www.cnblogs.com/ugoer/archive/2006/09/15/505596.html)）：

```sql
WHERE FirstName LIKE '_im' -- 可以找到所有三个字母的、以 im 结尾的名字（例如，Jim、Tim）。 
 
WHERE LastName LIKE '%stein' -- 可以找到姓以 stein 结尾的所有员工。 
 
WHERE LastName LIKE '%stein%' -- 可以找到姓中任意位置包括 stein 的所有员工。 
 
WHERE FirstName LIKE '[JT]im' -- 可以找到三个字母的、以 im 结尾并以 J 或 T 开始的名字（即仅有 Jim 和 Tim） 
 
WHERE LastName LIKE 'm[^c]%' -- 可以找到以 m 开始的、后面的（第二个）字母不为 c 的所有姓。
```

### SQL IN 

IN 操作符允许我们在 WHERE 子句中规定多个值。

```sql
SELECT column_name(s)
FROM 表名称
WHERE column_name IN (value1,value2,...)
```

例子：

Persons 表:

| Id | LastName | FirstName | Address        | City     |
|----|----------|-----------|----------------|----------|
| 1  | Adams    | John      | Oxford Street  | London   |
| 2  | Bush     | George    | Fifth Avenue   | New York |
| 3  | Carter   | Thomas    | Changan Street | Beijing  |


```sql
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```

结果：

| Id | LastName | FirstName | Address        | City    |
|----|----------|-----------|----------------|---------|
| 1  | Adams    | John      | Oxford Street  | London  |
| 3  | Carter   | Thomas    | Changan Street | Beijing |

### SQL BETWEEN 

操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

语法规则：

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
```

选择范围之外的数据

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name
NOT BETWEEN value1 AND value2
```

### SQL GROUP BY 

GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组。

语法规则：

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
```

例子：

"Orders" 表：

| O_Id | OrderDate  | OrderPrice | Customer |
|------|------------|------------|----------|
| 1    | 2008/12/29 | 1000       | Bush     |
| 2    | 2008/11/23 | 1600       | Carter   |
| 3    | 2008/10/05 | 700        | Bush     |
| 4    | 2008/09/28 | 300        | Bush     |
| 5    | 2008/08/06 | 2000       | Adams    |
| 6    | 2008/07/21 | 100        | Carter   |

```sql
-- 统计顾客的总金额
SELECT Customer,SUM(OrderPrice) FROM Orders
GROUP BY Customer
```

结果：

| Customer | SUM(OrderPrice) |
|----------|-----------------|
| Bush     | 2000            |
| Carter   | 1700            |
| Adams    | 2000            |

### SQL HAVING

在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用。

语法规则：

```sql
SELECT column_name, aggregate_function(column_name)
    FROM table_name
    WHERE column_name operator value
    GROUP BY column_name
    HAVING aggregate_function(column_name) operator value
```

例子：

 "Orders" 表:

| O_Id | OrderDate  | OrderPrice | Customer |
|------|------------|------------|----------|
| 1    | 2008/12/29 | 1000       | Bush     |
| 2    | 2008/11/23 | 1600       | Carter   |
| 3    | 2008/10/05 | 700        | Bush     |
| 4    | 2008/09/28 | 300        | Bush     |
| 5    | 2008/08/06 | 2000       | Adams    |
| 6    | 2008/07/21 | 100        | Carter   |

查找订单总金额少于 2000 的客户
```sql
SELECT Customer, SUM(OrderPrice) FROM Orders 
    GROUP BY Customer
    HAVING SUM(OrderPrice)<2000
```

查找客户 "Bush" 或 "Adams" 拥有超过 1500 的订单总金额。
```sql
SELECT Customer,SUM(OrderPrice) FROM Orders
    WHERE Customer='Bush' OR Customer='Adams'
    GROUP BY Customer
    HAVING SUM(OrderPrice)>1500
```

### SQL Alias

通过使用 SQL，可以为列名称和表名称指定别名（Alias）。使用别名可以有效的缩短命令长度，减少出错的可能。

```sql
-- 设定表的别名
SELECT column_name(s)
FROM table_name
AS alias_name

-- 设定列的别名
SELECT column_name AS alias_name
FROM table_name
```
例子：

```sql
-- 不使用别名的 SQL 语句
SELECT Product_Orders.OrderID, Persons.LastName, Persons.FirstName
    FROM Persons, Product_Orders
    WHERE Persons.LastName='Adams' AND Persons.FirstName='John'

-- 使用别名对应的 SQL 语句
SELECT po.OrderID, p.LastName, p.FirstName
    FROM Persons AS p, Product_Orders AS po
    WHERE p.LastName='Adams' AND p.FirstName='John'
```

### SQL JOIN 系列

SQL join 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。

数据库中的表可通过键将彼此联系起来。主键（Primary Key）是一个列，在这个列中的每一行的值都是唯一的。在表中，每个主键的值都是唯一的。这样做的目的是在不重复每个表中的所有数据的情况下，把表间的数据交叉捆绑在一起。

+ `(INNER) JOIN`: 如果表中有至少一个匹配，则返回行
+ `LEFT (OUTER) JOIN`: 即使右表中没有匹配，也从左表返回所有的行
+ `RIGHT (OUTER) JOIN`: 即使左表中没有匹配，也从右表返回所有的行
+ `FULL (OUTER) JOIN`: 只要其中一个表中存在匹配，就返回行

例子：

两个关系数据表如下："Persons" 表，"Orders" 表。表的第一列分别其主键。

"Persons" 表：

| Id_P | LastName | FirstName | Address        | City     |
|------|----------|-----------|----------------|----------|
| 1    | Adams    | John      | Oxford Street  | London   |
| 2    | Bush     | George    | Fifth Avenue   | New York |
| 3    | Carter   | Thomas    | Changan Street | Beijing  |

"Orders" 表：

| Id_O | OrderNo | Id_P |
|------|---------|------|
| 1    | 77895   | 3    |
| 2    | 44678   | 3    |
| 3    | 22456   | 1    |
| 4    | 24562   | 1    |
| 5    | 34764   | 65   |

分别采用不同的方法列出订单的客户名字。

**WHERE**

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
    FROM Persons, Orders
    WHERE Persons.Id_P = Orders.Id_P 
    ORDER BY Persons.LastName
```

结果：

| LastName | FirstName | OrderNo |
|----------|-----------|---------|
| Adams    | John      | 22456   |
| Adams    | John      | 24562   |
| Carter   | Thomas    | 77895   |
| Carter   | Thomas    | 44678   |

**INNER JOIN**

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
    FROM Persons
    INNER JOIN Orders ON Persons.Id_P = Orders.Id_P
    ORDER BY Persons.LastName
```

结果同上。

** LEFT JOIN **

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
    FROM Persons
    LEFT JOIN Orders
    ON Persons.Id_P=Orders.Id_P
    ORDER BY Persons.LastName
```

结果：

| LastName | FirstName | OrderNo |
|----------|-----------|---------|
| Adams    | John      | 22456   |
| Adams    | John      | 24562   |
| Carter   | Thomas    | 77895   |
| Carter   | Thomas    | 44678   |
| Bush     | George    |         |

LEFT JOIN 关键字会从左表 (Persons) 那里返回所有的行，即使在右表 (Orders) 中没有匹配的行。

**RIGHT JOIN**

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
    FROM Persons
    RIGHT JOIN Orders
    ON Persons.Id_P=Orders.Id_P
    ORDER BY Persons.LastName
```
结果：

| LastName | FirstName | OrderNo |
|----------|-----------|---------|
| Adams    | John      | 22456   |
| Adams    | John      | 24562   |
| Carter   | Thomas    | 77895   |
| Carter   | Thomas    | 44678   |
|          |           | 34764   |

RIGHT JOIN 关键字会从右表 (Orders) 那里返回所有的行，即使在左表 (Persons) 中没有匹配的行。

**FULL JOIN**

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
    FROM Persons
    FULL JOIN Orders
    ON Persons.Id_P=Orders.Id_P
    ORDER BY Persons.LastName
```

结果：

| LastName | FirstName | OrderNo |
|----------|-----------|---------|
| Adams    | John      | 22456   |
| Adams    | John      | 24562   |
| Carter   | Thomas    | 77895   |
| Carter   | Thomas    | 44678   |
| Bush     | George    |         |
|          |           | 34764   |


### SQL UNION

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。默认的 UNION 操作符选取不同的值，如果允许重复的值需要使用 UNION ALL。

语法结构：
```sql
-- 结果不重复
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2

-- 允许结果重复
SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
```

### SQL SELECT INTO

SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表中。SELECT INTO 语句常用于创建表的备份复件或者用于对记录进行存档。

语法规则：
```sql
SELECT column_name(s)
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```

### SQL CREATE DATABASE

CREATE DATABASE 用于创建数据库。

语法规则：
```sql
CREATE DATABASE database_name
```

### SQL CREATE TABLE

CREATE TABLE 语句用于创建数据库中的表。

语法规则：

```sql
CREATE TABLE 表名称
(
col_name_1 type_1 C,
col_name_2 type_2 C,
col_name_3 type_3 C,
)
```

数据类型（data_type）规定了列可容纳何种数据类型。下面的表格包含了SQL中最常用的数据类型：

| integer, int, smallint,tinyint(size) | 仅容纳整数。在括号内规定数字的最大位数。 |
|--------------------------------------------------------|----------------------------------------------------------------------------------------|
| decimal(size,d) | 容纳带有小数的数字。 |
| numeric(size,d) | size 规定数字的最大位数。"d" 规定小数点右侧的最大位数。 |
| char(size) | 容纳固定长度的字符串（可容纳字母、数字以及特殊字符）。在括号中规定字符串的长度。 |
| varchar(size) | 容纳可变长度的字符串（可容纳字母、数字以及特殊的字符）。在括号中规定字符串的最大长度。 |
| date(yyyymmdd) | 容纳日期。 |

### SQL 约束

约束用于限制加入表的数据的类型。

可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后也可以（通过 ALTER TABLE 语句）。

我们将主要探讨以下几种约束：

**NOT NULL**

NOT NULL 约束强制列不接受 NULL 值。NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

**UNIQUE**

NIQUE 约束唯一标识数据库表中的每条记录。
UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。
PRIMARY KEY 拥有自动定义的 UNIQUE 约束。
请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。

语法规则：

```sql
------- 创建表时声明 UNIQUE 域 -------

-- 单一的 UNIQUE 域
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
UNIQUE (Id_P)
)

-- 复合的 UNIQUE 域
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
)

------- 创建表后定义 UNIQUE 域 -------

ALTER TABLE Persons ADD UNIQUE (Id_P)

ALTER TABLE Persons ADD CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)

------- 删除 UNIQUE 域 -------

-- MySQL
ALTER TABLE Persons DROP INDEX uc_PersonID

-- SQL Server / Oracle / MS Access
ALTER TABLE Persons DROP CONSTRAINT uc_PersonID
```

**PRIMARY KEY**

PRIMARY KEY 约束唯一标识数据库表中的每条记录。
主键必须包含唯一的值。
主键列不能包含 NULL 值。
每个表都应该有一个主键（虽然不是强制的），并且每个表只能有一个主键。

```sql

-- MySQL
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
PRIMARY KEY (Id_P)
)

CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)
)
```

PRIMARY KEY 可以在在表生成后再定义，修改或删除主键约束。

**FOREIGN KEY**

一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY。
FOREIGN KEY 约束用于预防破坏表之间连接的动作。
FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

```sql
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)
)

CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
CONSTRAINT fk_PerOrders FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)
)

```
**CHECK**

CHECK 约束用于限制列中的值的范围。
如果对单个列定义 CHECK 约束，那么该列只允许特定的值。
如果对一个表定义 CHECK 约束，那么此约束会在特定的列中对值进行限制。

```sql
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
PRIMARY KEY (Id_P),
CHECK (Id_P>0)
)
```
**DEFAULT**

DEFAULT 约束用于向列中插入默认值。

```sql
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
City varchar(255) DEFAULT 'Sandnes'
)
```


### SQL CREATE INDEX

在表中创建索引，以便更加快速高效地查询数据。
用户无法看到索引，它们只能被用来加速搜索/查询。
更新一个包含索引的表需要比更新一个没有索引的表更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

```sql
CREATE INDEX "INDEX_NAME" ON "TABLE_NAME" (COLUMN_NAME);
```

### SQL DROP

通过使用 DROP 语句，可以轻松地删除索引、约束、表、表中数据和数据库。

```sql
-- 以下均是基于 MySQL 语法

-- 删除索引
ALTER TABLE table_name DROP INDEX index_name

-- 删除表
DROP TABLE 表名称

-- 删除表数据
TRUNCATE TABLE 表名称

-- 删除数据库
DROP DATABASE 数据库名称
```


### SQL ALTER

ALTER TABLE 语句用于在已有的表中添加、修改或删除列或者列的属性。

```sql
-- 添加列
ALTER TABLE table_name
ADD column_name datatype

-- 删除列
ALTER TABLE table_name 
DROP COLUMN column_name

-- 修改列的数据类型
ALTER TABLE table_name
MODIFY COLUMN column_name datatype
```

### SQL AUTO INCREMENT

我们通常希望在每次插入新记录时，自动地创建主键字段的值。
我们可以在表中创建一个 auto-increment 字段。

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
PRIMARY KEY (P_Id)
)
```

### SQL VIEW（有待考究）

在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。
视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。我们可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，我们也可以提交数据，就像这些来自于某个单一的表。
数据库的设计和结构不会受到视图中的函数、where 或 join 语句的影响。


语法规则：

```sql
CREATE VIEW view_name AS
    SELECT column_name(s)
        FROM table_name
        WHERE condition
```

### SQL Date

当我们处理日期时，最难的任务恐怕是确保所插入的日期的格式，与数据库中日期列的格式相匹配。
只要数据包含的只是日期部分，运行查询就不会出问题。但是，如果涉及时间，情况就有点复杂了。
在讨论日期查询的复杂性之前，我们先来看看最重要的内建日期处理函数。

| 函数          | 作用                                |
|---------------|-------------------------------------|
| NOW()         | 返回当前的日期和时间                |
| CURDATE()     | 返回当前的日期                      |
| CURTIME()     | 返回当前的时间                      |
| DATE()        | 提取日期或日期/时间表达式的日期部分 |
| EXTRACT()     | 返回日期/时间按的单独部分           |
| DATE_ADD()    | 给日期添加指定的时间间隔            |
| DATE_SUB()    | 从日期减去指定的时间间隔            |
| DATEDIFF()    | 返回两个日期之间的天数              |
| DATE_FORMAT() | 用不同的格式显示日期/时间           |


SQL Date 数据类型

MySQL 使用下列数据类型在数据库中存储日期或日期/时间值：

+ DATE - 格式 YYYY-MM-DD
+ DATETIME - 格式: YYYY-MM-DD HH:MM:SS
+ TIMESTAMP - 格式: YYYY-MM-DD HH:MM:SS
+ YEAR - 格式 YYYY 或 YY


### SQL NULL

如果表中的某个列是可选的，那么我们可以在不向该列添加值的情况下插入新记录或更新已有的记录。这意味着该字段将以 NULL 值保存。
NULL 值的处理方式与其他值不同。
NULL 用作未知的或不适用的值的占位符。
无法比较 NULL 和 0；它们是不等价的。
无法使用比较运算符来测试 NULL 值，比如 =, <, 或者 <>。
必须使用 IS NULL 和 IS NOT NULL 操作符。

"Persons" 表：

| Id | LastName | FirstName | Address      | City     |
|----|----------|-----------|--------------|----------|
| 1  | Adams    | John      |              | London   |
| 2  | Bush     | George    | Fifth Avenue | New York |
| 3  | Carter   | Thomas    |              | Beijing  |


```sql
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NULL
```

结果：

| LastName | FirstName | Address |
|----------|-----------|---------|
| Adams    | John      |         |
| Carter   | Thomas    |         |

```sql
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NOT NULL
```

结果：

| LastName | FirstName | Address      |
|----------|-----------|--------------|
| Bush     | George    | Fifth Avenue |

与 NULL 相关的集合函数：

IFNULL()

### MySQL 数据类型

在 MySQL 中，有三种主要的类型：文本、数字和日期/时间类型。

** 主要 Text 类型 **：

| 数据类型      | 描述                                                                                                                                                    |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| CHAR(size)    | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。                                                       |
| VARCHAR(size) | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。注释：如果值的长度大于 255，则被转换为 TEXT 类型。 |
| TINYTEXT      | 存放最大长度为 255 个字符的字符串。                                                                                                                     |
| TEXT          | 存放最大长度为 65,535 个字符的字符串。                                                                                                                  |
| BLOB          | 用于 BLOBs (Binary Large OBjects)。存放最多 65,535 字节的数据。                                                                                         |

** 数字类型 **：

| 数据类型        | 描述                                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------|
| TINYINT(size)   | -128 到 127 常规。0 到 255 无符号*。在括号中规定最大位数。                                                  |
| SMALLINT(size)  | -32768 到 32767 常规。0 到 65535 无符号*。在括号中规定最大位数。                                            |
| MEDIUMINT(size) | -8388608 到 8388607 普通。0 to 16777215 无符号*。在括号中规定最大位数。                                     |
| INT(size)       | -2147483648 到 2147483647 常规。0 到 4294967295 无符号*。在括号中规定最大位数。                             |
| BIGINT(size)    | -9223372036854775808 到 9223372036854775807 常规。0 到 18446744073709551615 无符号*。在括号中规定最大位数。 |
| FLOAT(size,d)   | 带有浮动小数点的小数字。在括号中规定最大位数。在 d 参数中规定小数点右侧的最大位数。                         |
| DOUBLE(size,d)  | 带有浮动小数点的大数字。在括号中规定最大位数。在 d 参数中规定小数点右侧的最大位数。                         |
| DECIMAL(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。                                                            |

 这些整数类型拥有额外的选项 UNSIGNED。通常，整数可以是负数或正数。如果添加 UNSIGNED 属性，那么范围将从 0 开始，而不是某个负数。

** Date 类型 **

| 数据类型    | 描述                                                                                                                                                  |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| DATE()      | 日期。格式：YYYY-MM-DD。注释：支持的范围是从 '1000-01-01' 到 '9999-12-31'                                                                             |
| DATETIME()  | 日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS。注释：支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'                                      |
| TIMESTAMP() | 时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的描述来存储。支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS 注释：支持的范围是从 '-838:59:59' 到 '838:59:59'                                                                                 |
| YEAR()      | 2 位或 4 位格式的年。注释：4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。                                      |

即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。

### SQL 服务器

##### DBMS - 数据库管理系统（Database Management System）
数据库管理系统是一种可以访问数据库中数据的计算机程序。

DBMS 使我们有能力在数据库中提取、修改或者存贮信息。

不同的 DBMS 提供不同的函数供查询、提交以及修改数据。

##### RDBMS - 关系数据库管理系统（Relational Database Management System）

关系数据库管理系统 (RDBMS) 也是一种数据库管理系统，其数据库是根据数据间的关系来组织和访问数据的。

20 世纪 70 年代初，IBM 公司发明了 RDBMS。

RDBMS 是 SQL 的基础，也是所有现代数据库系统诸如 Oracle、SQL Server、IBM DB2、Sybase、MySQL 以及 Microsoft Access 的基础。

## SQL 函数

在 SQL 中，基本的函数类型和种类有若干种。函数的基本类型是：

+ Aggregate 函数：面向一系列的值，并返回一个单一的值。
+ Scalar 函数：面向某个单一的值，并返回基于输入值的一个单一的值。

[函数大全](http://blog.csdn.net/sugang_ximi/article/details/6664748 "点我点我")
