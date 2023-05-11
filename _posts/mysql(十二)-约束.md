---
title: mysql(十二)约束
tags: sql
categories: sql
date: 2023-5-7 12:46:02
author: sec
---
# 约束

## 分类

1. not null(非空约束)
2. unique(唯一性约束)
3. primary key(主键约束)
4. foreign key(外键约束)
5. check(检查约束)
6. default(默认值约束)

##  非空约束

### 作用

`限定某个字段/某列的值不为空`

### 关键字

not null

### 特点

- 默认，所有的类型的值都可以是NULL，包括int，float等数据类型
- 非空约束只能出现在表对象的列上，只能某个列单独限制，不能组合限制
- 一个表可以有很多列都限制非空
- 空字符串''不等于NULL，0也不等于NULL

### 添加非空约束

```sql
# 创建表
CREATE TABLE student(
    sid int,
    sname varchar(20) not null,
    tel char(11),
    cardid char(18) not null
);
# 创建表后
alter table 表名称 modify 字段名 数据类型 not null;
alter table student modify sname varchar(20) not null;
```

### 删除非空约束

```sql
alter table 表名称 modify 字段名 数据类型 NULL;#去掉not null，相当于修改某个非注解字段，该字段允许为空
ALTER TABLE emp
MODIFY sex VARCHAR(30) NULL;
或
alter table 表名称 modify 字段名 数据类型;#去掉not null，相当于修改某个非注解字段，该字段允许为空
```

## 唯一性约束

### 作用

`限制某个字段/某列不能为空`

### 关键词

unique

### 特点

- 同一个表可以有多个唯一约束。

- 唯一约束可以是某一个列的值唯一，也可以多个列组合的值唯一。

- 唯一性约束允许列值为空。

- 在创建唯一约束的时候，如果不给唯一约束命名，就默认和列名相同。

- MySQL会给唯一约束的列上默认创建一个唯一索引。

### 添加唯一性约束

```sql
# 语法
create table 表名称(
字段名 数据类型,
字段名 数据类型 unique,
字段名 数据类型 unique key,
字段名 数据类型
);
create table 表名称(
字段名 数据类型,
字段名 数据类型,
字段名 数据类型,
[constraint 约束名] unique key(字段名)
);

# 举例
CREATE TABLE t_course(
cid INT UNIQUE,
cname VARCHAR(100) UNIQUE,
description VARCHAR(200)
);
CREATE TABLE USER(
id INT NOT NULL,
NAME VARCHAR(25),
PASSWORD VARCHAR(16),
-- 使用表级约束语法
CONSTRAINT uk_name_pwd UNIQUE(NAME,PASSWORD)
);
#字段列表中如果是一个字段，表示该列的值唯一。如果是两个或更多个字段，那么复合唯一，即多个字段的组合是唯一的
#方式1：
alter table 表名称 add unique key(字段列表);
#方式2：
alter table 表名称 modify 字段名 字段类型 unique;

alter table student add unique key(tel);
```

### 复合唯一约束

```sql
#学生表(tel,cardid)不是复合唯一约束
create table student(
sid int, #学号
sname varchar(20), #姓名
tel char(11) unique key, #电话
cardid char(18) unique key #身份证号
);


#选课表
create table student_course(
id int,
sid int,
cid int,
score int,
unique key(sid,cid) #复合唯一
);
```

### 删除唯一约束

- 添加唯一性约束的列上也会自动创建唯一索引。

- 删除唯一约束只能通过删除唯一索引的方式删除。

- 删除时需要指定唯一索引名，唯一索引名就和唯一约束名一样。

- 如果创建唯一约束时未指定名称，如果是单列，就默认和列名相同；如果是组合列，那么默认和()中排在第一个的列名相同。也可以自定义唯一性约束名

```sql
SELECT * FROM information_schema.table_constraints WHERE table_name = '表名'; #查看都有哪些约束
ALTER TABLE USER DROP INDEX uk_name_pwd;
```

## 主键约束

### 作用

`用来唯一标识表中的一行记录`

### 关键字

primary key

### 特点

- 主键约束相当于唯一约束+非空约束的组合，主键约束列不允许重复，也不允许出现空值。

- 一个表最多只能有一个主键约束，建立主键约束可以在列级别创建，也可以在表级别上创建

- 主键约束对应着表中的一列或者多列（复合主键）

- 如果是多列组合的复合主键约束，那么这些列都不允许为空值，并且组合的值不允许重复。

- 当创建主键约束时，系统默认会在所在的列或列组合上建立对应的主键索引（能够根据主键查询的，就根据主键查询，效率更高）。如果删除主键约束了，主键约束对应的索引就自动删除了。

- 需要注意的一点是，不要修改主键字段的值。因为主键是数据记录的唯一标识，如果修改了主键的值，就有可能会破坏数据的完整性。

### 添加主键约束

```sql
# 建表时指定主键约束
# 语法
create table 表名称(
字段名 数据类型 primary key, #列级模式
字段名 数据类型,
字段名 数据类型
);
create table 表名称(
字段名 数据类型,
字段名 数据类型,
字段名 数据类型,
[constraint 约束名] primary key(字段名) #表级模式
);
# 举例
create table temp(
id int primary key,
name varchar(20)
);

# 建表后增加主键约束
ALTER TABLE 表名称 ADD PRIMARY KEY(字段列表); #字段列表可以是一个字段，也可以是多个字段，如果是多个字段的话，是复合主键

ALTER TABLE student ADD PRIMARY KEY (sid);
ALTER TABLE emp5 ADD PRIMARY KEY(NAME,pwd);
```

### 复合主键

```sql
# 语法
create table 表名称(
字段名 数据类型,
字段名 数据类型,
字段名 数据类型,
primary key(字段名1,字段名2) #表示字段1和字段2的组合是唯一的，也可以有更多个字段
);
# 举例
CREATE TABLE emp6(
id INT NOT NULL,
NAME VARCHAR(20),
pwd VARCHAR(15),
CONSTRAINT emp7_pk PRIMARY KEY(NAME,pwd)
);
```

### 删除主键约束

```sql
# 语法
alter table 表名称 drop primary key;
# 举例
ALTER TABLE student DROP PRIMARY KEY;
```

## 自增列

### 作用

`某个字段值自增`

### 关键字

auto_increment

### 特点和要求

- 一个表最多只能有一个自增长列

- 当需要产生唯一标识符或顺序值时，可设置自增长

- 自增长列约束的列必须是键列（主键列，唯一键列）

- 自增约束的列的数据类型必须是整数类型

- 如果自增列指定了 0 和 null，会在当前最大值的基础上自增；如果自增列手动指定了具体值，直接赋值为具体值。

### 增加自增列

```sql
# 建表时
# 语法
create table 表名称(
    字段名 数据类型 primary key auto_increment,
    字段名 数据类型 unique key not null,
    字段名 数据类型 unique key,
    字段名 数据类型 not null default 默认值,
);
create table 表名称(
    字段名 数据类型 default 默认值 ,
    字段名 数据类型 unique key auto_increment,
    字段名 数据类型 not null default 默认值,,
    primary key(字段名)
);
# 举例
create table employee(
    eid int primary key auto_increment,
    ename varchar(20)
);

# 建表后
alter table 表名称 modify 字段名 数据类型 auto_increment;
alter table employee modify eid int auto_increment;
```

### 删除自增列

```sql
# 语法
alter table 表名称 modify 字段名 数据类型; #去掉auto_increment相当于删除
# 举例
alter table employee modify eid int;
```

## 外键约束

### 作用

`限定某个表的某个字段的引用完整性`

### 关键字

foreign key

### 主表和从表/父表和子表

- 主表（父表）：被引用的表，被参考的表

- 从表（子表）：引用别人的表，参考别人的表

### 特点

- 从表的外键列，必须引用/参考主表的主键或唯一约束的列（被依赖/被参考的值必须是唯一的）

- 在创建外键约束时，如果不给外键约束命名，默认名不是列名，而是自动产生一个外键名（例如student_ibfk_1;），也可以指定外键约束名。

- 创建(CREATE)表时就指定外键约束的话，先创建主表，再创建从表

- 删表时，先删从表（或先删除外键约束），再删除主表

- 当主表的记录被从表参照时，主表的记录将不允许删除，如果要删除数据，需要先删除从表中依赖该记录的数据，然后才可以删除主表的数据

- 在“从表”中指定外键约束，并且一个表可以建立多个外键约束

- 从表的外键列与主表被参照的列名字可以不相同，但是数据类型必须一样，逻辑意义一致。如果类型不一样，创建子表时，就会出现错误

- 当创建外键约束时，系统默认会在所在的列上建立对应的普通索引。但是索引名是外键的约束名。（根据外键查询效率很高）

- 删除外键约束后，必须 手动 删除对应的索引

### 添加外键约束

```sql
# 创建表
create table 主表名称(
    字段1 数据类型 primary key,
    字段2 数据类型
);
create table 从表名称(
    字段1 数据类型 primary key,
    字段2 数据类型,
    [CONSTRAINT <外键约束名称>] FOREIGN KEY（从表的某个字段) references 主表名(被参考字段)
);
-- FOREIGN KEY: 在表级指定子表中的列
-- REFERENCES: 标示在父表中的列

create table dept( #主表
    did int primary key, #部门编号
    dname varchar(50) #部门名称
);
create table emp(#从表
    eid int primary key, #员工编号
    ename varchar(5), #员工姓名
    deptid int, #员工所在的部门
    foreign key (deptid) references dept(did) #在从表中指定外键约束
    #emp表的deptid和和dept表的did的数据类型一致，意义都是表示部门的编号
);

# 建表后
# 语法
ALTER TABLE 从表名 ADD [CONSTRAINT 约束名] FOREIGN KEY (从表的字段) REFERENCES 主表名(被引用字段) [on update xx][on delete xx];
# 举例
ALTER TABLE emp1
ADD [CONSTRAINT emp_dept_id_fk] FOREIGN KEY(dept_id) REFERENCES dept(dept_id);
```

### 约束等级

- Cascade方式 ：在父表上update/delete记录时，同步update/delete掉子表的匹配记录

- Set null方式 ：在父表上update/delete记录时，将子表上匹配记录的列设为null，但是要注意子表的外键列不能为not null

- No action方式 ：如果子表中有匹配的记录，则不允许对父表对应候选键进行update/delete操作

- Restrict方式 ：同no action， 都是立即检查外键约束

- Set default方式 （在可视化工具SQLyog中可能显示空白）：父表有变更时，子表将外键列设置成一个默认的值，但Innodb不能识别

`对于外键约束，最好是采用: ON UPDATE CASCADE ON DELETE RESTRICT 的方式。`

```sql
on update cascade on delete set null
on update set null on delete cascade
on update cascade on delete cascade

create table emp(
    eid int primary key, #员工编号
    ename varchar(5), #员工姓名
    deptid int, #员工所在的部门
    foreign key (deptid) references dept(did) on update cascade on delete set null
    #把修改操作设置为级联修改等级，把删除操作设置为set null等级
);
```

### 删除外键约束

```sql
# 语法
# (1)第一步先查看约束名和删除外键约束
SELECT * FROM information_schema.table_constraints WHERE table_name = '表名称';#查看某个表的约束名
ALTER TABLE 从表名 DROP FOREIGN KEY 外键约束名;
# （2）第二步查看索引名和删除索引。（注意，只能手动删除）
SHOW INDEX FROM 表名称; #查看某个表的索引名
ALTER TABLE 从表名 DROP INDEX 索引名;

# 举例
SELECT * FROM information_schema.table_constraints WHERE table_name = 'emp';
alter table emp drop foreign key emp_ibfk_1;
```

## check约束

### 作用

`检查某个字段的值是否符号xx要求，一般指的是值的范围`

### 关键词

ckeck

### 支持

`5.7`不支持

### 添加

```sql
create table employee(
    eid int primary key,
    ename varchar(5),
    gender char check ('男' or '女')
);

CREATE TABLE temp(
    id INT AUTO_INCREMENT,
    NAME VARCHAR(20),
    age INT CHECK(age > 20),
    PRIMARY KEY(id)
);
age tinyint check(age >20) 或 sex char(2) check(sex in(‘男’,’女’))
CHECK(height>=0 AND height<3)
```

### 默认值约束

### 作用

`给某个字段/某列指定默认值，一旦设置默认值，在插入数据时，如果此字段没有显式赋值，则赋值为默认值。`

### 关键词

default

### 添加

```sql
# 建表时
# 语法
create table 表名称(
    字段名 数据类型 primary key,
    字段名 数据类型 unique key not null,
    字段名 数据类型 unique key,
    字段名 数据类型 not null default 默认值,
);
create table 表名称(
    字段名 数据类型 default 默认值 ,
    字段名 数据类型 not null default 默认值,
    字段名 数据类型 not null default 默认值,
    primary key(字段名),
    unique key(字段名)
);
# 举例
create table employee(
    eid int primary key,
    ename varchar(20) not null,
    gender char default '男',
    tel char(11) not null default '' #默认是空字符串
);

# 建表后
# 语法
alter table 表名称 modify 字段名 数据类型 default 默认值;
#如果这个字段原来有非空约束，你还保留非空约束，那么在加默认值约束时，还得保留非空约束，否则非空约束就被删除了
#同理，在给某个字段加非空约束也一样，如果这个字段原来有默认值约束，你想保留，也要在modify语句中保留默认值约束，否则就删除了
alter table 表名称 modify 字段名 数据类型 default 默认值 not null;
# 举例
alter table employee modify gender char default '男'; #给gender字段增加默认值约束
alter table employee modify tel char(11) default ''; #给tel字段增加默认值约束
```

### 删除

```sql
# 语法
alter table 表名称 modify 字段名 数据类型 ;#删除默认值约束，也不保留非空约束
alter table 表名称 modify 字段名 数据类型 not null; #删除默认值约束，保留非空约束
# 举例
alter table employee modify gender char; #删除gender字段默认值约束，如果有非空约束，也一并删除
alter table employee modify tel char(11) not null;#删除tel字段默认值约束，保留非空约束
```

