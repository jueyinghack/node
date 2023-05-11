---
title: mysql(十四)存储过程
tags: sql
categories: sql
date: 2023-5-9 13:12:02
author: sec
---
## 存储过程

### 前言

> 在日常工作中，表和表都是关联的。当我们修改一张表的同时可能需要同时修改其他的表。比如当我们购买了一个商品，我们需要在库存表减去库存，在订单表生成信息等

### 什么是存储过程

`事先经过编译并存储在数据库中的一段SQL语句`

### 为什么使用

- 可以减少开发人员的工作
- 减少数据在数据库和应用服务器之间的传输
- 提高处理效率

### 特点

- 封装，复用， 可以把某一业务SQL封装在存储过程中，需要用到的时候直接调用即可；
- 可以接收参数，也可以返回数据， 在存储过程中，可以传递参数，也可以接收返回值；
- 减少网络交互，提升效率，如果一次操作涉及到多条SQL，每执行一次都是一次网络传输。 如果将这些sql操作封装在存储过程中，只需网络交互一次可能就可以了；

### 存储过程变量

1. 系统变量

   ```sql
   # 查看系统变量
   SHOW [ SESSION | GLOBAL ] VARIABLES ; -- 查看所有系统变量
   SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......'; -- 可以通过LIKE模糊匹配方 式查找变量
   SELECT @@[SESSION | GLOBAL] 系统变量名; -- 查看指定变量的值
   
   # 设置系统变量
   SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;
   SET @@[SESSION | GLOBAL]系统变量名 = 值 ;
   
   # 特点
   mysql服务重新启动之后，所设置的全局参数会失效，要想不失效，可以在 /etc/my.cnf 中配置；
   全局变量(GLOBAL): 全局变量针对于所有的会话；
   会话变量(SESSION): 会话变量针对于单个会话，在另外一个会话窗口就不生效了；
   ```

2. 用户自定义变量

   ```sql
   # 设置自定义变量
   SET @var_name = expr [, @var_name = expr] ... ;
   SET @var_name := expr [, @var_name := expr] ... ;
   SELECT @var_name := expr [, @var_name := expr] ... ;
   SELECT 字段名 INTO @var_name FROM 表名;
   ```

3. 局部变量

   ```sql
   # 根据需要定义的在局部生效的变量，访问前，需通过DECLARE声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的BEGIN … END块。
   # 语法
   DECLARE 变量名 变量类型 [DEFAULT ... ] ;
   # 案例
   create procedure p2()
   BEGIN
   	declare my_count int default 0;
   	select count(*) into my_count from account;
   	select my_count;
   END;
   ```

### 函数

#### 条件判断(if,case when)

```sql
# if 语法
IF 条件1 THEN
	.....
ELSEIF 条件2 THEN -- 可选
	.....
ELSE -- 可选
	.....
END IF;
# 案例
create procedure p3()
BEGIN
	declare score int default 59;
	declare result VARCHAR(12);
	if score >= 85 then
		set result := '优秀';
	elseif score >= 60 then
		set result := '及格';
	else
		set result := '不及格';
	end if;
	select result;
END;

# case - when 
# 语法1
CASE case_value
	WHEN when_value1 THEN statement_list1
	[ WHEN when_value2 THEN statement_list2] ...
	[ ELSE statement_list ]
END CASE;
# 语法2
CASE
	WHEN search_condition1 THEN statement_list1
	[WHEN search_condition2 THEN statement_list2] ...
	[ELSE statement_list]
END CASE;

# 案例
create procedure p5()
BEGIN
	declare score int default 77;
	declare result VARCHAR(12);
	case
		when score >= 85 then
				set result := '优秀';
		when score >= 60 then
				set result := '及格';
		else
				set result := '未知结果';
	end case;	
	select result;
END;
```

#### 循环(while,repeat,loop)

```sql
# When（while 循环是有条件的循环控制语句。满足条件时，再执行循环体中的SQL语句；）
# 语法
WHILE 条件 DO
	SQL逻辑...
END WHILE;
# 案例(从1加到n)
create procedure p6(in n int)
	begin
		declare total int default 0;
		while n>0 do
			set total := total + n;
			set n := n - 1 ;
		end while;
		select total;
	end ;
	
# REPAT（repeat是有条件的循环控制语句, 当满足 until 声明的条件的时候，则退出循环,语法结构为：）
# 语法
REPEAT
SQL逻辑...
UNTIL 条件
END REPEAT;
# 案例
create procedure p7(in n int)
	begin
		declare total int default 0;
		repeat 
			set total := total + n;
			set n := n - 1;
		until n <= 0
		end repeat;
		select total;
	end ;

# LOOP（LOOP 可以实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。LOOP可以配合一下两个语句使用：）
# 语法
[begin_label:] LOOP
	SQL逻辑...
END LOOP [end_label];
# 案例
create procedure p8(in n int)
	begin
		declare total int default 0;
		sum:loop
			if n<=0 then 
					leave sum;
			end if;
			set total := total + n;
			set n := n - 1;
		end loop sum;
		select total;
	end ;

```

#### 存储函数

1. 创建

    ```sql
    
    CREATE FUNCTION func_name ([param_name type[,...]])
    RETURNS type
    [characteristic ...] 
    BEGIN
        routine_body
    END;
    （1）func_name ：存储函数的名称。
    （2）param_name type：可选项，指定存储函数的参数。type参数用于指定存储函数的参数类型，该类型可以是MySQL数据库中所有支持的类型。
    （3）RETURNS type：指定返回值的类型。
    （4）characteristic：可选项，指定存储函数的特性。
    （5）routine_body：SQL代码内容。
    ```

2. 调用

    ```sql
    SELECT func_name([parameter[,…]]);
    ```

3. 修改

    ```sql
    ALTER FUNCTION func_name [characteristic ...]
    characteristic:
        COMMENT 'string'
      | LANGUAGE SQL
      | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
      | SQL SECURITY { DEFINER | INVOKER }
    ```

4. 删除

    ```sql
    DROP FUNCTION IF EXISTS func_user;
    
    ```

    



### 创建存储过程

```sql
# 语法
CREATE PROCEDURE 存储过程名称 ([ 参数列表 ])
BEGIN
-- SQL语句
END ;
# 案例
CREATE PROCEDURE p1()
BEGIN
	SELECT count(*) FROM account;
END;
```

### 调用存储过程

```sql
# 语法
call 名称([参数])
# 案例
call p1();
```

### 查看存储过程

```sql
SHOW CREATE PROCEDURE 存储过程名称 ; -- 查询某个存储过程的定义
```

### 删除存储过程

```sql
DROP PROCEDURE [ IF EXISTS ] 存储过程名称 ;
```



#### 存储过程输入输出参数使用

> 存储过程中使用到的参数的类型，主要分为以下三种：IN、OUT、INOUT；

- IN
  该类参数作为输入，也就是需要调用时传入值

- OUT
  该类参数作为输出，也就是该参数可以作为返回值

- INOUT
  既可以作为输入参数，也可以作为输出参数

1. 创建

   ```sql
   # 语法
   CREATE PROCEDURE 存储过程名称 ([ IN/OUT/INOUT 参数名 参数类型 ])
   BEGIN
   	-- SQL
   END;
   # 案例
   create procedure p4(in score int,out result varchar(12))
   BEGIN
   	if score >= 85 then
   		set result := '优秀';
   	elseif score >= 60 then
   		set result := '及格';
   	else
   		set result := '不及格';
   	end if;
   END;
   
   ```

2. 调用

   ```sql
   call p4(90,@result);
   select @result;
   ```

   

### 游标

> 游标（CURSOR）是用来存储查询结果集的数据类型 , 在存储过程和函数中可以使用游标对结果集进行循环的处理；

1. 声明游标

   ```sql
   DECLARE 游标名称 CURSOR FOR 查询语句 ;
   ```

2. 打开游标

   ```sql
   OPEN 游标名称 ;
   ```

3. 获取游标记录

   ```sql
   FETCH 游标名称 INTO 变量 [, 变量 ] ;
   ```

4. 关闭游标

   ```sql
   CLOSE 游标名称 ;
   ```

5. 案例

   ```sql
   CREATE PROCEDURE get_count_by_limit_total_salary(IN limit_total_salary DOUBLE,OUT total_count INT)
   BEGIN
   	DECLARE sum_salary DOUBLE DEFAULT 0; #记录累加的总工资
   	DECLARE cursor_salary DOUBLE DEFAULT 0; #记录某一个工资值
   	DECLARE emp_count INT DEFAULT 0; #记录循环个数
   	#定义游标 
   	DECLARE emp_cursor CURSOR FOR SELECT salary FROM employees ORDER BY salary DESC;
   	#打开游标 
   	OPEN emp_cursor;
   	REPEAT
   		#使用游标（从游标中获取数据）
   		FETCH emp_cursor INTO cursor_salary;
   		SET sum_salary = sum_salary + cursor_salary;
   		SET emp_count = emp_count + 1;
   		UNTIL sum_salary >= limit_total_salary
   	END REPEAT;
   	SET total_count = emp_count;
   	#关闭游标
   	CLOSE emp_cursor;
   END ;
   ```

   
