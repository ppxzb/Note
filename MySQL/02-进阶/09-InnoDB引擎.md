## 一、逻辑存储结构

### 1. Tablespace（表空间，`.ibd` 文件）

```text
一个 MySQL 实例可以对应多个表空间，用于存储记录、索引等数据
```

### 2. Segment（段）

```text
分为数据段（Leaf node segment）、索引段（Non_leaf node segment）、回滚段（Rollback segment）
```

```text
InnoDB 是索引组织表，数据段就是 B+ 树的叶子节点，索引段即为 B+ 树的非叶子节点
```

### 3. Extent（区）

```text
表空间的单元结构，每个区的大小为 1M
```

```text
默认情况下，InnoDB 存储引擎页大小为 16K，即一个区中有 64 个连续的页
```

### 4. Page（页）

```text
InnoDB 存储引擎磁盘管理的最小单元，每个页的大小默认为 16KB
```

```text
为了保证页的连续性，InnoDB 存储引擎每次从磁盘申请 4-5 个区
```

### 5. Row（行）

```text
InnoDB 存储引擎数据是按行进行存放的
```

- `Trx_id`：每次对某条记录进行改动时，都会把对应的事务 id 赋值给 `trx_id` 隐藏列  
- `Roll_pointer`：每次对某条记录进行改动时，都会把旧的版本写入到 undo 日志中；该隐藏列相当于指针，可通过它找到记录修改前的位置  

## 二、架构

## 三、事务原理

## 四、MVCC
