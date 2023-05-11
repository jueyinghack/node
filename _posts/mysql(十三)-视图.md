---
title: mysql(十三)视图
tags: sql
categories: sql
date: 2023-5-8 13:10:02
author: sec
---
# 视图

> 视图是个虚拟表，本身不具有数据，占用很少的空间
>
> 视图建立在原有的表上，视图赖以建立的表叫`基表`
>
> 针对视图做DML操作，会影响到基表的数据，反之亦然
>
> 视图本身的删除，不会导致基表中数据的删除

## 创建视图

- 在create view 语句中嵌入子查询

```sql
create [or replace]
[algorithm = [undefined | megre | temptable]]
view 视图名称 [(字段列表)]
as 查询语句
[with | cascaded | local | check option]
```

- 精简版

  ```sql
  create view 视图名 [(字段列表)]
  as 查询语句
  ```

- 举例

  ```sql
  create view empvu80
  as
  select employee_id,last_name,salary
  from employees
  where department_id = 80
  ```

  

## 删除视图

```sql
DROP VIEW<视图名>[CASCADE]# 使用CASCADE才能级联删除该视图和由它导出的视图。
# 删除基表时，由该基表导出的所有视图定义都必须显式删除（）。
```



## 查询视图

```sql
SELECT Sno,Sage FROM IS Student WHERE Sage<20;
转换为:
SELECT Sno,Sage
FROM Student
WHERE Sdept='IS’AND Sage<20
```

## 更新视图

```sql
/**
更新视图是指通过视图来插入(INSERT)、删除(DELETE)和修改(UPDATE)数据。对视图的更新最终要转换为对基本表的更新。
在关系数据库中，并不是所有的视图都是可更新的，因为有些视图的更新不能唯一地有意义地转换成对相应基本表的更新。一般地，行列子集视图是可更新的。不同的DBMS有不同的规定。
*/
```

## 查看视图
```sql
# 查看视图结构
describe 'view1';
# 查看视图属性信息
show table status like 'view1'
# 查看视图详细定义信息
show create view view1;
```


## 视图作用
```
1)视图能够简化用户的操作。
    视图机制使用户可以将注意力集中在所关心的数据上。简化用户的查询操作，特别是涉及多表的查询。
    
2)视图使用户能以多种角度看待同一数据。
    当不同种类的用户共享同一个数据库时，这种灵活性是非常重要的。也就是说不同种类的用户对应不同的视图来访问数据。
    
3)视图对重构数据库提供了一定程度的逻辑独立性。
	数据的逻辑独立性是指当数据库重构时，如增加新的关系或对原有关系的修改等，用户的应用程序不会受影响。当数据库重构时，可以通过定义视图实现原来的关系，使用户的外模式保持不变。
	
4)视图能够对机密数据提供安全保护。
	对系统中的不同用户定义不同的视图，使用户只能访问他权限范围内的数据。
	
5)适当的利用视图可以更清晰的表达查询。 
```