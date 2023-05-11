---
title: mysql(十)DCL
tags: sql
categories: sql
date: 2023-5-7 11:09:02
author: sec
---
## DCL

### 创建用户

```sql
# 创建 user1 用户，只能在 localhost 这个服务器登录 mysql 服务器，密码为 123
create user 'user1'@'localhost' identified by '123';
# 创建 user2 用户可以在任何电脑上登录 mysql 服务器，密码为 123
create user 'user2'@'%' identified by '123';
```

### 赋予权限

```sql
# 权限（CREATE、ALTER、SELECT、INSERT、UPDATE、ALL）
GRANT 权限 1, 权限 2... ON 数据库名.表名 TO '用户名'@'主机名';
# 给 user1 用户分配对 test 这个数据库操作的权限：创建表，修改表，插入记录，更新记录，查询
grant create,alter,insert,update,select on test.* to 'user1'@'localhost';
# 给 user2 用户分配所有权限，对所有数据库的所有表
grant all on *.* to 'user2'@'%';
```

### 撤销权限

```sql
# 语法
REVOKE 权限 1, 权限 2... ON 数据库.表名 revoke all on test.* from 'user1'@'localhost'; '用户名'@'主机名';
# 撤销 user1 用户对 test 数据库所有表的操作的权限
revoke all on test.* from 'user1'@'localhost';
```

### 查看权限

```sql
SHOW GRANTS FOR '用户名'@'主机名';
```

### 删除用户

```sql
# 语法
DROP USER '用户名'@'主机名';
# 删除用户2
drop user 'user2'@'%';
```

### 修改管理员密码

```sql
# 语法
mysqladmin -uroot -p password 新密码
```

### 修改普通用户密码

```sql
# 语法
set password for '用户名'@'主机名' = password('新密码');
```

