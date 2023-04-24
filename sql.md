## Count

### 简介

>count 函数返回匹配指定列的值的数目

### 用法

1. select count(*) from table_name  返回列表中的记录数
2. select count(column_name) form table_name 返回指定列的数目
3. select count(distinct column_name) from table_name   返回指定列的不同值的数目

### count(1)，count(*)，count(字段)区别

```
# count(1)
遍历全表，但是不取值，server 层对返回的每一行数据新增一个1，然后进行判断累加。包括了忽略所有列，用1代表代码行，在统计结果的时候，不会忽略列值为NULL。
```

```
count(*)
返回的行一定不是空。扫描全表，但是不取值，按行累加。包括了所有的列，相当于行数，在统计结果的时候，不会忽略列值为NULL
```

```
count(字段)
进行全表扫描，然后判断指定字段的值是不是为NULL，不为NULL则累加。只包括列名那一列，在统计结果的时候，会忽略列值为空（这里的空不是只空字符串或者0，而是表示null）的计数，即某个字段值为NULL时，不统计
```

### count 表达式

1. case-when

   ```
   case—when：当条件满足是表达式的结果为非空，条件不满足时无结果默认为NULL
   格式：count(case when(条件) then 列名 end)
   count(case when column='1' then column end)
   ```

2. or null 

   ```
   count(表达式 or null):当条件不满足时，函数变成了count(null)不会统计数量
   count(sex='男' or null)
   ```

3. if 判断

   ```
   count(IF(is_reply=1,1,NULL)):当条件满足时表达式的值为非空，条件不满足时表达式值为NULL
   IF(condition,expression_1,expression_2)：逻辑为：如果condition为true，返回expression_1，否则返回NULL。
   ```

   