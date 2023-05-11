---
title: mysql(三)select,运算符
tags: sql
categories: sql
date: 2023-4-24 21:59:02
author: sec
---
### select语句

```sql
# select ...
select 1; # 用于检测是否数据库连接
select 9/2;
select 1 from dual # dual是个伪表
```

```sql
# select 字段1,字段2... from 表名
select * from employees;# 查询表中所有数据
```

```sql
# 列的别名
# as(alias) 全称,可以省略
select employee_id emp,last_name,department_id from employees # 表中就没有employee_id字段，会出现emp字段，(emp是employee_id的别名)
select employee_id emp,last_name as lname,department_id "部门id" from employees # 可以用中文
select employee_id emp,last_name as lname,salary * 12 "annual sal" from employees # 计算后别名
```

```sql
# 去重重复行
SELECT department_id from employees; # 没有去重
SELECT DISTINCT department_id from employees; # 去重了的情况
```

```sql
# 空值参与运算
# 空值与其他值进行运算也是空值
可以用IFNULL解决
select salary * (1 + IFNULL(commission_pct,0) * 12 "年工资" from employees;
```

```sql
# 着重号(以下表名为特殊字符,则用着重号来解决)
select * form `order`;
```

```sql
# 查询常数
select 1,123,employee_id form employees;
```

```sql
# 显示表结构
# 显示表中字段的详细信息
describe employees;
desc employees;
```

```sql
# 过滤信息(where)
SELECT * from employees where department_id = 90;
SELECT * from employees where last_name = 'king';
```



### 算数运算符

```sql
# 算数运算符 + - * / div % mod
+ 不能拼接字符串只能作为数字运算
- 
*
/ 除不尽保留小数
div 向下取整除法
mod 取余数
```

### 比较运算符

```sql
# 比较运算符
# =  <=>  <>  !=  <  <=  >  >=
 1 = '1' (true)
 0 = 'a' (true) # 字符串存在隐式转换，如果转换不成功，则看成0
# 两边都是字符串的话就是按字符串比较(按asicc码比较)
 'a' = 'a' (true)
 'b' = 'a' (false)
# 只要有NULL参与比较结果为NULL
 1 = NULL (NULL)
 NULL = NULL (NULL)
 select last_name,commission_pct from employees where commission_pct = NULL; # 此时执行，不会有任何结果

# <=> 安全等于(可以对NULL进行判断)
 select 1 <=> 2, 1 <=> 1, 1 <=> 'a', 0 <=> 'a'; (0,1,0,1) # 没有NULL结果一样
 select 1 <=> NULL, NULL <=> NULL; (0,1) # 结果不为NULL
# <> 不等于

# IS NULL,IS NOT NULL, ISNULL
 is NULL 为空的
 is not null 不为空的
 ISNULL() # 为空的
# LEAST(最小)， GREATEST(最大)
SELECT LEAST('g','b','t','m'),GREATEST('g','b','t','m');

# between 下边界 and 上边界
select employee_id, last_name.salary from employees where salary between 6000 and 8000;# 包含边界

# in (set) \ not in (set) 是否在集合set里
 select last_name.salary,department_id from employees where department_id 10 or department_id = 20 or department_id = 30
 select last_name.salary,department_id from employees where department_id in (10,20,30);

# like 模糊查询
# %不确定个数字符, _代表一个不确定字符， 转义字符: \, escape '$' 指定$ 为转义字符
 select last_name from employees where last_name like '_a%';# 查询第二个字符是a的
 
# REGEXP \ RLIKE 正则表达式
select 'shaksjad' regexp '^s', 'shaksjad' regexp 'p$', 'shaksjad' regexp 'hk'
```

### 逻辑运算符

```sql

# OR || AND && NOT ! XOR javascript:;
# XOR 满足前一个且不满足后一个     或者      满足后一个且不满足前一个
## ！！  AND的优先级比OR高
```

### 位运算符

```sql
# & 按位与
# | 按位或
# ^ 按位异或
# ~ 按位取反
# >> 右移
# << 左移
```

