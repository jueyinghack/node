---
title: mysql(五)单行函数
tags: sql
categories: sql
date: 2023-05-05 21:01:02
author: sec
---

| 函数 | 说明 | 结果 |
| --------------------- | -------------- |-------------- |
|结果全部是小写 | LOWER('SQL Course')   | sql course |
|结果全部是大写 | UPPER('SQL Course')   | SQL COURSE |
|结果首字符大写 | INITCAP('SQL Course') | |
| 连接函数 | CONCAT('Hello', 'World') | HelloWorld |
| 字符截取函数 | SUBSTR('HelloWorld',1,5)截取从1-5个字符 | Hello |
| 字符串统计长度 | LENGTH('HelloWorld') | 10 |
| 查找字符位置函数 | INSTR('HelloWorld', 'W') | 6 |
| 前填充函数 | LPAD(salary,10,'*') | `*****24000` |
| 后填充函数 | RPAD(salary, 10, '*') | `24000*****` |
| 替换函数 | REPLACE('JACK and JUE','J','BL') | BLACK and BLUE |
| 字符剪切函数 | TRIM('H' FROM 'HelloWorld') | elloWorld |
| ROUND：四舍五入到到指定的十进制值 | ROUND(45.926, 2) | 45.93 |
| TRUNC：将数字截尾取整 | TRUNC(45.926, 2) | 45.92 |
| MOD：返回余数 | MOD(1600, 300) | 100 |
|两个日期相差的月数  | MONTHS_BETWEEN ('01-SEP-95','11-JAN-94') |  19.6774194 |
|向指定日期中加上若干月数 | ADD_MONTHS  ('06-MAR-17',1) | 06-APR-17 |
|指定日期的下一个日期 | NEXT_DAY ('01-SEP-95','FRIDAY') | 08-SEP-95 |
|本月的最后一天 | LAST_DAY ('01-SEP-95') | 30-SEP-95 |
|日期四舍五入 | ROUND (SYSDATE,'MONTH') | 01-MAR-17 |
|日期四舍五入 | ROUND (SYSDATE ,'YEAR') | 01-JAN-17 |
|日期截断 | TRUNC (SYSDATE ,'MONTH') | 01-MAR-17 |
|日期截断 | TRUNC (SYSDATE ,'YEAR') | 01-JAN-17 |

## 数字

| 函数 | 说明 | 结果 |
| --------------------- | -------------- |-------------- |
| 返回绝对值 | ABS(x) | 返回x的绝对值 |
| 返回x的符号，正数返回1，负数返回-1,0返回0 | SIGN(x) |  |
| 返回圆周率 | PI() |  |
| 返回大于或等于某个值的最小整数 | CEIL(x),CEILNG(x) |  |
| 返回大于或等于某个值的最大整数 | FLOOR(x) |  |
| 返回列表中的最小值 | LEAST(e1,e2,e3...) |  |
| 返回列表中的最大值 | GREATEST(e1,e2,e3...) |  |
| 返回x除以y后的余数 | MOD(x,y) |  |
| 返回0-1随机数 | RAND() |  |
| 返回0-1的随机值，其中x作为种子，相同的x会产生相同的随机数 | RAND(x) |  |
| 返回一个对x的值四舍五入后。最接近x的整数 | ROUND(x) |  |
| 返回一个对x的值进行四舍五入后最接近x的值并保留小数到后面Y位 | ROUND(x,y) |  |
| 返回数字x截断为y位小数的结果 | TRUNCATE(x,y) |  |
| 返回x的平方根。当x的值为负数时，返回NULL | SQRT(x) |  |

## 弧度

| 函数 | 说明 | 结果 |
| --------------------- | -------------- |-------------- |
| 将角度转化为弧度，其中，参数为角度值 | RADIANS(x) |  |
| 将弧度转化为角度，其中，参数x为弧度值 | DEGREES(x) | |

## 三角函数

| 函数 | 说明 | 结果 |
| --------------------- | -------------- |-------------- |
| 返回x的正弦值，其中，x为弧度值 | SIN(x) | |
| 返回x的反正弦值，即获取正弦为x的值。如果x的值不在-1到1之间则返回NULL | ASIN(x) | |
| 返回x的余弦 | COS(x) | |
| 返回反余弦 | ACOS(x) | |
| 返回正切值 | TAN(x) | |
| 反正切值 | ATAN(x) | |
| 返回两个参数的反正切值 | ATAN2(m,n) | |
| 返回余切值 | COT(x) | |
| 返回x的y次方 | POW(x,y),POWER(x,y) | |
| 返回e的x次方 | EXP(x) | |
| 返回以e为底的x的对数 | LN(x),LOG(x) | |
| 返回以10为底的x的对数 | LOG10(x) | |
| 返回以2为底的x的对数 | LOG2(x) | |
| 返回x的二进制 | BIN(x) | |
| 返回x的十六进制 | HEX(x) | |
| 返回x的八进制 | OCT(x) | |
| 返回f1进制数变为f2进制数 | CONV(x,f1,f2) | |


## 字符串

| 函数 | 说明 | 结果 |
| --------------------- | -------------- |-------------- |
| 返回字符串s中的第一个字符的ASCII码 | ASCII(s) | |
| 返回字符串s的字符数。 | CHAR_LENGTH(s) | |
| 返回字符串s的字节数，和字符集有关 | LENGTH(s) | |
| 连接s1,s2...sn为一个字符串 | CONCAT(s1,s2...sn) | |
| 同concat(s1..sn)函数，但是每个字符串之间要加上x | concat_ws(x,s1,s2,...sn) | |
| 将字符串str从第idx位置开始，len个字符长的子串替换为字符串replacestr | insert(str,idx,len,replacestr) | |
| 用字符串b替换字符串str中所有出现的字符串a | replace(str,a,b) | |
| 将字符串s的所有字母转成大写字母 | upper(s),UCASE(s) | |
| 将字符串s的所有字母转为小写字母 | LOWER(s),LCASE(s) | |
| 返回字符串str最左边的n个字符 | LEFT(str,n) | |
| 返回字符串str最右边的n个字符 | RIGHT(str,n) | |
| 用字符串pad对str最左边进行填充，直到str的长度为len个字符 | LPAD(str,len,pad) | |
| 用字符串pad对str最右边进行填充，直到str的长度为len个字符 | RPAD(str,len,pad) | |
| 去掉字符串s左侧的空格 | LTRIM(s) | |
| 去掉字符串s右侧的空格 | RTRIM(s) | |
| 去掉字符串s开始与结尾的空格 | TRIM(s) | |
| 去掉字符串s开始与结尾的s1 | TRIM(s1 from s) | |
| 去掉字符串s开始的s1 | TRIM(LEADING s1 from s) | |
| 去掉字符串s结尾的s1 | TRIM(TRAILNG s1 from s) | |
| 返回str重复n次的结果 | REPEAT(str,n) | |
| 返回n个空格 | SPACE(n) | |
| 比较字符串s1,s2的ASCII码的大小 | STRCMP(s1,s2) | |
| 返回从字符串s的index位置其n个字符 | SUBSTR(s,index,len) | |
| 返回字符串substr在字符串str中首次出现的位置 | LOCATE(substr,str) | |
| 返回指定位置的字符串，如果m=1，返回s1,如果m=2返回s2. | ELT(m,s1,s2,...sn) | |
| 返回字符串s在字符串列表中第一次出现的位置 | FIELD（s,s1,s2...sn) | |
| 返回字符串s1在字符串s2出现的位置 | FIND_IN_SET(s1.s2) | |
| 返回s反转后的字符串 | REVERSE(s) | |
| 比较两个字符串，如果value1与value2相等，返回NULL，否则返回value1 | NULLIF(value1,value2) | |



## 日期和时间类型

| 函数                                                         | 用法                       |
| ------------------------------------------------------------ | -------------------------- |
| curdate(),current_date()                                     | 返回当前日期，质保函年月日 |
| curtime(), current_time()                                    | 返回当前时间，只包含时分秒 |
| new()/sysdate()/current_timestamp()/localtime()/localtimestamp() | 返回当前系统日期和时间     |
| UTC_date()                                                   | 返回utc（世界标准时间)     |
| utc_time()                                                   | 返回utc(世界标准时间)时间  |



## 日期与时间戳的转换

| 函数                     | 用法                                   |
| ------------------------ | -------------------------------------- |
| unix_timestamp()         | 以unix时间戳的形式返回当前时间         |
| unix_timestamp(date)     | 将时间date以unix时间戳的形式返回       |
| from_unixtime(timestamp) | 将unix时间戳的时间转换为普通格式的时间 |

## 获取月份、星期、星期数、天数等函数

| 函数                                 | 用法                         |
| ------------------------------------ | ---------------------------- |
| year(date)/month(date)/day(date)     | 返回具体的日期值             |
| hour(time)/minute(time)/second(time) | 返回具体的时间值             |
| monthname(date)                      | 返回月份：January            |
| dayname(date)                        | 返回星期几：MonDay，...      |
| weekday(date)                        | 返回周几，周一是0，周2是1    |
| quarter(date)                        | 返回日期对应的季度范围1~4    |
| week(date)，weekofyear(date)         | 返回一年中的第几周           |
| dayofyear(date)                      | 返回日期是一年中的第几天     |
| dayofmonth(date)                     | 返回日期位于所在月份的第几天 |
| dayofweek(date)                      | 返回周几周一是1              |

## 日期的操作函数

| 函数                    | 用法                                     |
| ----------------------- | ---------------------------------------- |
| extract(type from date) | 返回指定日期中特定的部分，type指特定的值 |



## 时间和秒钟的转换函数

| 函数                 | 用法                                                         |
| -------------------- | ------------------------------------------------------------ |
| time_to_sec(time)    | 将time转化为秒并返回结果值。转化公式为小时\*3600+分钟\*60+秒 |
| sec_to_time(seconds) | 将seconds描述转化为包含小时、分钟和秒的时间                  |

## 计算日期和时间的函数

| 函数                                                         | 用法                                            |
| ------------------------------------------------------------ | ----------------------------------------------- |
| date_add(datetime,interval expr type),adddate(date,interval expr type) | 返回与给定日期时间相差interval时间段的日期时间  |
| date_sub(date,interval expr type),subdate(date,interval expr type) | 返回与date相差interval时间间隔的日期            |
| addtime(time1,time2)                                         | 返回time1加上time2的时间，当time2为数字时作为秒 |
| subtime(time1,time2)                                         | 返回time1减去time2的时间，当time2为数字时作为秒 |
| datediff(date1,date2)                                        | 返回date1-date2的时间间隔                       |
| from_days(n)                                                 | 返回从0000年1月1日起N天后的日期                 |
| to_days(date)                                                | 返回日期date距离0000年1月1日的天数              |
| last_day(date)                                               | 返回date所在月份的最后一天的日期                |
| makedate(year,n)                                             | 针对给定年份与所在年份中的天数返回一个日期      |
| maketime(hour,minute,second)                                 | 将给定的小时，分钟和秒组合成时间并返回          |
| period_add(time,n)                                           | 返回time加上n后的时间                           |

## 日期的格式化与解析

| 函数                              | 用法                                   |
| --------------------------------- | -------------------------------------- |
| date_format(date,fmt)             | 按照字符串fmt格式化日期日期date值      |
| time_format(time,fmt)             | 按照字符串fmt格式化时间time值          |
| get_format(date_type,format_type) | 返回日期字符串的显示格式               |
| str_to_date(str,fmt)              | 按照字符串fmt对str解析，解析为一个日期 |



## 流程控制函数

| 函数                                                         | 用法                                           |
| ------------------------------------------------------------ | ---------------------------------------------- |
| IF(value,value1,value2)                                      | 如果value为true,返回value1,否则返回value2      |
| IFNULL(value1,value2)                                        | 如果value1不为NULL，返回value1，否则返回value2 |
| case when 条件1 then 结果1 when 条件2 then 结果2 ...[else resultn] END | 相当于java的if... else if ...else              |
| case expr when 常量值1 then 值1 when 常量值1 then 值1... [else 值 n] END | 相当于java的switch ... case ...                |

## 加密与解密函数

| 函数                        | 用法                                     |
| --------------------------- | ---------------------------------------- |
| password(str)               | 返回str加密版，41位长的字符串            |
| md5(str)                    | md5加密后的值                            |
| sha(str)                    | 返回加密后的值                           |
| encode(value,password_seed) | 返回用password_seed作为加密密码加密value |
| decode(value,password_seed) | 返回用password_seed作为解密密码加密value |



## mysql信息函数

| 函数                                               | 用法                                             |
| -------------------------------------------------- | ------------------------------------------------ |
| version()                                          | 返回mysql版本                                    |
| connection_id()                                    | 返回当前mysql服务器连接数                        |
| database(),schema()                                | 返回mysql命令行当前数据库                        |
| user()/current_user()/system_user()/session_user() | 返回当前连接mysql的用户名，返回结果为"名@用户名" |
| charset(value)                                     | 返回字符串value自变量的字符集                    |
| collation(value)                                   | 返回字符串value的比较规则                        |



## 其他函数

| 函数                           | 用法                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| format(value,n)                | 返回对数字value进行格式化后的结果数据，n表示四舍五入后保留到小数点后n位 |
| conv(value,from,to)            | 将value的值进行不同进制之间的转换                            |
| inet_aton(ipvalue)             | 将以点分隔的ip地址转化为一个数字                             |
| inet_ntoa(value)               | 将数字形式的ip地址转化为以点分隔的ip地址                     |
| benchmark(n,expr)              | 将表达式expr重复执行n次，用于测试mysql处理expr表达式所耗费的时间 |
| convert(value using char_code) | 将value所使用的字符编码修改为char_code                       |
|                                |                                                              |

