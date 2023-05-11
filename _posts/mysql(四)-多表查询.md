---
title: mysql(四)多表查询
tags: sql
categories: sql
date: 2023-4-28 18:02:02
author: sec
---
## 多表查询

> 笛卡尔积
>
> 两个集合进行组合的所有可能
>
> (a,b)×(c,d) = (ac,ad,bc,bd)
>
> 出现的原因
>
> 1. 省略多个表的连接条件(或关联条件)
> 2. 连接条件无效
> 3. 所有表中的所有行互相连接

---

```sql
# 当多表查询时缺少连接条件的时候，就会出现笛卡尔积问题
select id,name,employ from user,employees

# 多表查询的正确方式:有连接条件(当eid为null,没有相等的值的情况下，查不出来)
select id,name,employ from user,employees where user.eid = employees.eid

# 多表查询的时候，查询多个表同时出现的字段的时候要指明查询在哪个表中的字段
select user.eid,id,name,employ from user,employees where user.eid = employees.eid

# 从sql优化的角度来说，建议多表查询的时候每个字段都要指明其所在的表
select user.eid,user.id,user.name,employees.employ from user,employees where user.eid = employees.eid

# 可以给表起别名(一旦起了别名，则在查询的时候要用表的别名)
select u.eid,u.id,u.name,e.employ from user u,employees e where user.eid = employees.eid
```

### 等值连接与非等值连接

```sql
# 等值连接
select u.eid,u.id,u.name,e.employ from user u,employees e where user.eid = employees.eid


# 非等值连接
select u.eid,u.id,u.name,g.level from user u,grade_level g where salary between g.lowest_sal and g.hight_sal
```

### 自连接与非自连接

```sql
# 以上的都是非自连接

# 自连接就是自己引用自己的属性(leader_id 也是员工id属性，就是user表引用自己的id属性作为leader_id属性)
select id,name,leader_id from user 
```

### 内连接与外连接

```sql
# 以上都是内连接(把满足条件的查询出来，不满足的则舍弃)
select u.eid,u.id,u.name,e.employ from user u,employees e where user.eid = employees.eid

# 外连接，合并具有匹配项的数据外，还将不满足条件的数据查询出来，
# SQL92 左外连接（MYSQL不支持）[+]
select u.eid,u.id,u.name,e.employ from user u,employees e where user.eid = employees.eid(+)
# SQL99 内连接([inner]join ... on ...)
slect u.eid,u.id,u.name,e.employ from user u join employees e on user.eid = employees.eid join role r on u.role_id = r.id 

# SQL99 外连接(outer join ... on ...)
select last_name,department_name from employees e outer join departments d on e.department_iod = d.department_id

# 左外(left [outer] join ... on ...)
select last_name,department_name from employees e left outer join departments d on e.department_iod = d.department_id

# 右外(right [outer] join ... on ...)
select last_name,department_name from employees e right outer join departments d on e.department_iod = d.department_id

# 满外(full [outer] join ... on ...) [MYSQL不支持]
select last_name,department_name from employees e full outer join departments d on e.department_iod = d.department_id  
```

![image-20230428172635063](1/image-20230428172635063.png)

![image-20230428172736691](1/image-20230428172736691.png)

![image-20230428172846668](1/image-20230428172846668.png)

![image-20230428172954903](1/image-20230428172954903.png)

![image-20230428174636889](1/image-20230428174636889.png)

![image-20230428175520840](1/image-20230428175520840.png)

![image-20230428175406320](1/image-20230428175406320.png)

```sql
# union all 直接将两个集合进行拼接，没有去重
# union 将两个集合进行拼接并且去重
```

```sql
SQL99 新特性

# 自然连接(会自动将有相同字段进行相等处理)
select last_name,department_name from employees e join departments d on e.department_iod = d.department_id  and e.manager_id = d.manager_id
等值  ==
select last_name,department_name from employees e natural join departments d


# USING(对两个表中相同的字段进行自动查找，然后让它相等)
select last_name,department_name from employees e join departments d on e.department_iod = d.department_id

select last_name,department_name from employees e join departments d using(departmen_id)
```



