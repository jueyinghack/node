---
title: mysql(六)聚合查询
tags: sql
categories: sql
date: 2023-5-5 22:19:02
author: sec
---
## 常见的几个聚合函数

## AVG/SUM

```sql
# avg平均数
select avg(salary) from employees
# sum总和
select sum(salary) from employees 
```

## MAX/MIN

```sql
# 适用于数值类型，字符串类型，日期时间类型的字段（或变量）
# max最大值
select max(salary) from employees
# min最小值
select min(salary) from employees
```

## COUNT

```sql
# 1.作用：计算指定字段在查询结构中出现的个数（不包含Null值）
select count(employee_id) from employees
# 统计表中有几条数据
# MyISAM存储引擎的话，效率一样
# InnoDB效率不一样 count(*) = count(1) > count(字段)
select count(*)
select count(1)
select count(具体字段) # 不一定对，Null值不算数据
```

## group by

```sql
# 查询各个部门的平均工资
select department_id 
from employees
group by department_id
```

## having

```sql
# 过滤数据
# 查询各个部门中最高工资比10000高的部门信息
select department_id,max(salary)
from employees
group by department_id
# 如果过滤条件中使用了聚合函数，则必须使用having来替换where。否则报错
select department_id,max(salary)
from employees
group by department_id
having max(salary) > 10000
# having和group by 一起使用
```



## SQL底层执行原理
```sql
from -> on -> left join -> where -> group by -> having -> select -> distinct -> limit ->
```