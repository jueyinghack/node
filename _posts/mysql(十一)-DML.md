---
title: mysql(十一)DML
tags: sql
categories: sql
date: 2023-5-7 11:38:02
author: sec
---

## DML

### 准备工作

```sql
# 准备工作
create database table1 character set 'utf8';
use table1;
create table if not exists emp1(
	id int,
    name varchar(15),
    hire_data date,
    salary double(10,2)
);
desc emp1;
```



### 插入数据

```sql
# 添加数据
## 方式一(按照声明字段的先后顺序进行添加)
insert into emp1 values(1,'Tom','2000-12-21',3400);
# 可以不按照字段的先后顺序进行添加
insert into emp1(id,`name`,hire_data,salary)
values(2,'sec','2023-05-03',3500);
# 多行数据添加
insert into emp1(id,`name`,hire_data,salary)
values
(3,'jec','2023-05-04',3600),
(4,'mac','2023-05-04',3700);
## 方式二(将查询数据插入到表中)  --- 添加的时候要注意数据的长度大小问题
select * from emp1;
insert into emp1(id,`name`,hire_data,salary)
select employee_id,last_name,hire_data,salary
from employees
where department_id in (70,60);

```



### 更新数据(修改数据)

```sql
# 语法(可以实现批量数据更改)
update ... set ... where ...
# 修改一个字段
update emp1
set hire_data = curdate()
where id = 1;
# 修改一调数据的多个字段
update emp1
set hire_data = curdate()， salary = 6000
where id = 1;

```

### 删除数据

```sql
# 语法
delete from ... where ...
# 删除emp1所有数据
delete from emp1;
# 删除id为1的数据
delete from emp1 where id =1;
```

### mysql8计算列

```sql
create table test1(
a int,
b int,
c int generated always as ( a + b ) virtual # 计算列总是等于a+b
);
```

