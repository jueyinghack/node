---
title: mysql(九)DDL
tags: sql
categories: sql
date: 2023-5-6 16:00:00
author: sec
---
## DDL



>  DDL操作一旦执行，不可回滚

### 创建和管理数据库

```sql
# 创建数据库
## 方式一(使用的字符集是默认的字符集)
create database mytest1;
## 方式二(显示了指明了要创建的数据库的字符集)
create database mytast2 character set '';
## 方式三(如果数据库存在则不执行语句)
create database if not exists mytast3;

# 管理数据库
## 查看当前连接中的数据库都有哪些
show databases;
## 切换数据库
use mytast1;
## 查看当前数据库中保存的数据表
show tables;
## 查看当前使用的数据库
select database() from dual;
## 查看指定数据库下保存的数据表
show tables from mysql;
## 查看创建数据库的结构
show create database mytast1;

# 修改数据库
## 修改数据库字符集
alter database mytast2 character set 'utf8';

# 删除数据库
##方式一
drop database mytast1;
## 方式二
drop database if exists mytast1;
```

### 创建表

```sql
# 创建表
---如果创建表时没有指明字符集，默认使用数据库的字符集
## 方式一
create table if not exists myemp1(
	id int,
    emp_name varchar(15),
    hire_data date
);
## 方式二(基于现有的表创建新表，包括将数据迁移过来)
create table myemp2
as
select employee_id,last_name,salary
from employees

# 查看表结构
desc myemp1;
show create table myemp1;
```



### 修改表(alter)

```sql
# 添加一个字段
alter table myemp1
add salary double(10,2)

alter table myemp1
add phone varchar(30) first;

alter table myemp1
add phone varchar(30) after emp_name;

# 修改一个字段(数据类型，长度，默认值)
alter table myemp1
modify emp_name varchar(25)

alter table myemp1
modify emp_name varchar(25) default 'aaa';

# 重命名一个字段
alter table myemp1
change salary monthly_salary double(10,2);# 修改名的时候可以直接改变类型

# 删除一个字段
alter table myemp1
drop column my_email;

# 重命名表
## 方式一：
rename table myemp1
to myemp11;
## 方式二：
alter table myemp2
rename to myemp12;

# 删除表(删除表结构和数据)
drop table if exists myemp12;

# 清空表(删除表数据)
truncate table myemp12;
delete from （DML）[set autocommit = FALSE]

# 相同点：都可以实现数据删除
# 不同点：
## 		truncate table ： 一旦执行操作，表数据全部清除。同时，数据是不可以回滚的
## 		delete from  ： 一旦执行操作，表数据全部清除。数据是可以回滚，也不可回滚
```

