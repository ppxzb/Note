## 一、插入数据优化

### 1. insert 优化

```text
批量插入
```

```text
手动提交事务
```

```text
主键顺序插入
```

### 2. load 大批量插入数据


```bash
mysql --local-infile -u root -p
```

```sql
set global local_infile = 1;
```

```sql
load data local infile '文件.log'
into table table_name
fields terminated by ','
lines terminated by '\n';
```

## 二、主键优化

### 1. 页分裂

```text
页可以为空，也可以填充一半，也可以填充100%。每个页包含了2-N行数据（如果一行数据太大，会行溢出），根据主键排序
```

### 2. 页合并

```text
当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用
```

```text
当页中删除的记录达到 MERGE_THRESHOLD（默认为页的50%），InnoDB 会开始寻找最靠近的页（前或后）是否可以将两个页合并以优化空间使用
```

### 3. 主键设计原则

```text
满足业务需求的情况下，尽量降低主键的长度
```

```text
插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键
```

```text
尽量不要使用 UUID 做主键或者是其他自然主键，如身份证号
```

```text
业务操作时，避免对主键的修改
```

## 三、ORDER BY 优化

### 1. Using filesort

```text
通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区 sort buffer 中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序
```

### 2. Using index

```text
通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高
```

### 3. ORDER BY 设计原则

```text
根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
```

```text
尽量使用覆盖索引
```

```text
多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）
```

```text
如果不可避免的出现 filesort，大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size（默认 256k）
```

## 四、GROUP BY 优化

### 1. Using temporary（使用了临时表）

### 2. Using index

### 3. GROUP BY 设计原则

```text
在分组操作时，可以通过索引来提高效率
```

```text
分组操作时，索引的使用也是满足最左前缀法则的
```

## 五、LIMIT 优化

### 1. 优化思路

```text
通过创建覆盖索引能够比较好地提高性能，可以通过覆盖索引加子查询进行优化
```

## 六、COUNT 优化

- `count(主键)`：
```text
InnoDB 引擎会遍历整张表，把每一行的主键 id 值都取出来，返回给服务层。服务层拿到主键后，直接按行进行累加(主键不可能为 nul)
```

- `count(字段)`（无 `NOT NULL` 约束）：
```text
InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为 nul，不为 null，计数累加
```

- `count(字段)`（有 `NOT NULL` 约束）：
```text
InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加
```

- `count(1)`：
```text
InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字“1”进去，直接按行进行累加
```

- `count(*)`：
```text
InnoDB 引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加
```

- 效率排序：
```text
count(字段) < count(主键 id) < count(1) <≈ count(*)
```

## 七、UPDATE 设计原则

```text
InnoDB 的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁
```
