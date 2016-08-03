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

