## 一、数据库操作（DDL）

### 1. 查询

- 所有数据库
```sql
SHOW DATABASES;
```

- 当前数据库
```sql
SELECT DATABASE();
```

### 2. 创建

```sql
CREATE DATABASE [IF NOT EXISTS] 数据库名
  [DEFAULT CHARSET 字符集]
  [COLLATE 排序规则];
```

### 3. 删除

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```

### 4. 使用

```sql
USE 数据库名;
```

## 二、表操作（DDL）

### 1. 查询

```sql
SHOW TABLES;
DESC 表名;
SHOW CREATE TABLE 表名;
```

### 2. 创建表

```sql
CREATE TABLE 表名 (
  字段1 字段1类型 [COMMENT 字段1注释],
  ...
  字段n 字段n类型 [COMMENT 字段n注释]
) [COMMENT 表注释];
```

### 3. 修改表结构

- 添加字段
```sql
ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
```

- 修改字段类型
```sql
ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
```

- 修改字段名与字段类型
```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
```

- 删除字段
```sql
ALTER TABLE 表名 DROP 字段名;
```

- 修改表名
```sql
ALTER TABLE 表名 RENAME TO 新表名;
```

### 4. 删除/清空表

```sql
DROP TABLE [IF EXISTS] 表名;
TRUNCATE TABLE 表名;
```

## 三、DML（增删改）

### 1. INSERT

- 指定字段
```sql
INSERT INTO 表名(字段名1, 字段名2, ...)
VALUES (值1, 值2, ...);
```

- 全字段
```sql
INSERT INTO 表名
VALUES (值1, 值2, ...);
```

- 批量插入
```sql
INSERT INTO 表名(字段名1, 字段名2, ...)
VALUES (值1, 值2, ...),
       (值1, 值2, ...),
       ...;
```

### 2. UPDATE

```sql
UPDATE 表名
SET 字段名1 = 值1, 字段名2 = 值2, ...
[WHERE 条件];
```

### 3. DELETE

```sql
DELETE FROM 表名
[WHERE 条件];
```

## 四、DQL（查询）

### 1. DQL 编写顺序（语法书写顺序）

```sql
SELECT (字段列表)
FROM (表名列表)
WHERE (条件列表)
GROUP BY (分组字段列表)
HAVING (分组后条件列表)
ORDER BY (排序字段列表)
LIMIT (分页参数)
```

### 2. DQL 执行顺序

```text
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```

### 3. 基本查询

```sql
SELECT 字段1, 字段2, ... FROM 表名;
SELECT * FROM 表名;
SELECT 字段1 [AS] 别名1, 字段2 [AS] 别名2 FROM 表名;
SELECT DISTINCT 字段列表 FROM 表名;
```

### 4. 条件查询（WHERE）

- 比较条件：
```text
>, >=, <, <=, =, <> 或 !=
BETWEEN ... AND ...
IN (...)
LIKE（_ 单字符，% 任意长度）
IS NULL
```

- 逻辑条件：
```text
AND 或 &&
OR  或 ||
NOT 或 !
```

### 5. 聚合函数

```text
COUNT / MAX / MIN / AVG / SUM
```

### 6. 分组（GROUP BY / HAVING）

```sql
SELECT 字段列表
FROM 表名
[WHERE 条件]
GROUP BY 分组字段名
[HAVING 分组后过滤条件];
```

- WHERE vs HAVING
```text
执行时机：WHERE 在分组前；HAVING 在分组后
条件能力：WHERE 不能直接判断聚合函数；HAVING 可以
```

### 7. 排序（ORDER BY）

```sql
SELECT 字段列表
FROM 表名
ORDER BY 字段1 排序方式1, 字段2 排序方式2;
```

```text
ASC（默认升序）
DESC（降序）
```

### 8. 分页（LIMIT）

```sql
SELECT 字段列表
FROM 表名
LIMIT 起始索引, 查询记录数;
```

- 要点：
  - 起始索引从 0 开始  
  - 起始索引 = (页码 - 1) * 每页记录数  
  - 第一页可简写：`LIMIT 10`  
  - 不同数据库分页语法不同  

## 五、DCL（用户与权限）

### 1. 用户管理

```sql
USE mysql;
SELECT * FROM user;

CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';

ALTER USER '用户名'@'主机名'
IDENTIFIED WITH mysql_native_password BY '新密码';

DROP USER '用户名'@'主机名';
```

- 主机名可使用 `%` 通配。

### 2. 权限控制

```sql
SHOW GRANTS FOR '用户名'@'主机名';

GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';

REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

常用权限：
```text
ALL / ALL PRIVILEGES
SELECT / INSERT / UPDATE / DELETE
ALTER / DROP / CREATE
```

