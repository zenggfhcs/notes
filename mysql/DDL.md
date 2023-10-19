# 数据定义语言

## 数据库

### 查询

查询所有数据库

```mysql
show databases; 
```
查询当前数据库

```mysql
select database();
```

### 使用

使用数据库

```mysql
use 数据库名;
```

### 创建

创建数据库

```mysql
create database [if not exists] 数据库名;
```

### 删除

删除数据库

```mysql
drop database [if exists] 数据库名;
```

## 表

### 创建

```mysql
create table  表名(
 	字段1  字段类型  [ 约束 ]  [ comment  字段1注释 ] ,
 	...... 
 	字段n  字段类型  [ 约束 ]  [ comment  字段n注释 ] 
) [ comment  表注释 ] ;
```

#### 约束

|   约束   |                       描述                       |   关键字    |
| :------: | :----------------------------------------------: | :---------: |
| 非空约束 |              限制该字段值不能为null              |  not null   |
|   唯一   |       保证字段的所有数据都是唯一、不重复的       |   unique    |
|   主键   |     主键是一行数据的唯一标识，要求非空且唯一     | primary key |
| 默认约束 |   保存数据时，如果未指定该字段值，则采用默认值   |   default   |
| 外键约束 | 让两张表的数据建立连接，保证数据的一致性和完整性 | foreign key |

### 查询

查询当前数据库所有表：
```mysql
show tables; 
```
查询表结构：
```sql
desc 表名; 
```
查询建表语句：
```mysql
show create table 表名;
```
### 修改

添加字段：
```sql
alter table 表名 add 字段名 类型(长度) [comment 注释] [约束]; 
```
修改字段类型：
```sql
alter table 表名 modify 字段名 新数据类型(长度);
```
修改字段名和字段类型：
```sql
alter table 表名 change 旧字段名 新字段名 类型 (长度) [comment 注释] [约束]; 
```
删除字段：
```mysql
alter table 表名 drop column 字段名;
```
 修改表名： 
```sql
rename table 表名 to 新表名;
```
### 删除

删除表：
```sql
drop table [ if exists ] 表名;
```