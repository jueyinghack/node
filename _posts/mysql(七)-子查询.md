---
title: mysql(七)子查询
tags: sql
categories: sql
date: 2023-5-6 10:15:00
author: sec
---
## 子查询

```sql
# 多表查询
select last_name ,salary from employees e1,employees e2
where e2.salary > e1.salary
and e1.last_name = 'Abel'
#
# 子查询
select last_name,salary 
from employees
where salary >  (select salary 
                 from employees 
                 where last_name = 'Abel'
                )
# 单行子查询 vs 多行子查询
## 单行子查询(查出来的数据是一行数据)
### 单行操作符 = != > >= < <=

## 多行子查询
### 多行子查询操作符 in any all some




# 相关子查询 vs 不相关子查询

## 不相关子查询步骤
#   先进行内查询，然后进行外查询。将外查询的值和内查询的值进行比较。如果符合则保留

## 相关子查询
#  1.从主查询中获取候选列   2.子查询使用主查询的数据  3.如果满足子查询条件则返回该行 -> 返回第一步
```

