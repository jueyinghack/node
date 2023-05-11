---
title: mysql(二)登录,编码,分类
tags: sql
categories: sql
date: 2023-4-24 21:59:01
author: sec
---
### 登录

```cmd
mysql -u root -h localhost -P 3306  -p
# -u 用户名
# -h 服务器地址
# -P 端口
# -p 密码登录
```

### 查看编码命令

```cmd
show variables like 'character_%';
show variables like 'collation_%';
```

### 修改编码

```cmd
# 在my.ini中修改
[mysql] 
...
default-character-set=utf8
[mysqld]
...
character-set-server=utf8
collation-server=utf8_general_ci
```

### SQL分类

1. DDL 数据定义语言

   ```sql
   create,
   drop,
   alter,
   rename,
   truncate
   ```

2. DML 数据操作语言

   ```sql
   insert,
   delete,
   update,
   select
   ```

3. DCL 数据控制语言

   ```sql
   grant,
   revoke,
   commit,
   rollback,
   savepoint
   ```
   
### 导入数据库

```sql
source sql文件
```