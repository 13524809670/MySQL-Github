常用的MySQL内置函数
===================
[TOC]

比较函数
--------
##### COALESCE():
返回参数中第一个不是NULL的值。  

```sql
root@localhost : wing 11:23:22> select coalesce(null,null,1,2,3,'test');
+----------------------------------+
| coalesce(null,null,1,2,3,'test') |
+----------------------------------+
| 1                                |
+----------------------------------+
1 row in set (0.00 sec)

root@localhost : wing 11:39:14> select coalesce(null,'test',null,1,2);
+--------------------------------+
| coalesce(null,'test',null,1,2) |
+--------------------------------+
| test                           |
+--------------------------------+
1 row in set (0.00 sec)

root@localhost : wing 11:42:54> select coalesce('test',null,1,2);
+---------------------------+
| coalesce('test',null,1,2) |
+---------------------------+
| test                      |
+---------------------------+
1 row in set (0.00 sec)
```
##### GREATEST():
返回最大的参数,若参数中存在NULL值,则返回NULL值。  

```sql
root@localhost : wing 11:45:24> select greatest(1,2,null,3,4);
+------------------------+
| greatest(1,2,null,3,4) |
+------------------------+
|                   NULL |
+------------------------+
1 row in set (0.00 sec)

root@localhost : wing 11:45:29> select greatest(1,2,3,4);
+-------------------+
| greatest(1,2,3,4) |
+-------------------+
|                 4 |
+-------------------+
1 row in set (0.00 sec)

root@localhost : wing 11:45:33> select greatest(1,2,3,4,'test');
+--------------------------+
| greatest(1,2,3,4,'test') |
+--------------------------+
| 4                        |
+--------------------------+
1 row in set, 1 warning (0.00 sec)

Warning (Code 1292): Truncated incorrect DOUBLE value: 'test'
```
##### IN():
检验某个值是否存在于IN的集合中。  
##### INTERVAL():
返回第一个比第一个参数小的索引值,其内部参数必须为数值型,如果第一个参数为NULL值,则返回-1。  

```sql
root@localhost : wing 11:47:18> select interval(null,1,2,3,4);
+------------------------+
| interval(null,1,2,3,4) |
+------------------------+
|                     -1 |
+------------------------+
1 row in set (0.00 sec)

root@localhost : wing 11:54:34> select interval(2,1,2,3,4);
+---------------------+
| interval(2,1,2,3,4) |
+---------------------+
|                   2 |
+---------------------+
1 row in set (0.00 sec)
```
##### ISNULL():
判断其参数是否为NULL值。  

```sql
root@localhost : wing 11:54:39> select isnull(null);
+--------------+
| isnull(null) |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)

root@localhost : wing 11:57:34> select isnull(0);
+-----------+
| isnull(0) |
+-----------+
|         0 |
+-----------+
1 row in set (0.00 sec)

root@localhost : wing 11:57:37> select isnull('test');
+----------------+
| isnull('test') |
+----------------+
|              0 |
+----------------+
1 row in set (0.00 sec)
```
##### LEAST():
返回参数中最小的值,如果参数中存在NULL值,则返回NULL。  

```sql
root@localhost : wing 11:57:44> select least(null,1,2,3);
+-------------------+
| least(null,1,2,3) |
+-------------------+
|              NULL |
+-------------------+
1 row in set (0.00 sec)

root@localhost : wing 12:00:05> select least(3,2,7);
+--------------+
| least(3,2,7) |
+--------------+
|            2 |
+--------------+
1 row in set (0.00 sec)

root@localhost : wing 12:00:13> select least(3,2,7,'test');
+---------------------+
| least(3,2,7,'test') |
+---------------------+
| 0                   |
+---------------------+
1 row in set, 1 warning (0.00 sec)

Warning (Code 1292): Truncated incorrect DOUBLE value: 'test'
root@localhost : wing 12:00:22> select least(3,2,7,null,'test');
+--------------------------+
| least(3,2,7,null,'test') |
+--------------------------+
| NULL                     |
+--------------------------+
1 row in set (0.00 sec)
```

##### STRCMP(expr1,expr2)
字符串比较函数  
expr1<expr2返回 -1  
expr1=expr2返回 0  
expr1>expr2返回 1  

```sql
root@localhost : wing 12:08:48> select strcmp('tast','test');
+-----------------------+
| strcmp('tast','test') |
+-----------------------+
|                    -1 |
+-----------------------+
1 row in set (0.00 sec)

root@localhost : wing 12:08:52> select strcmp('test','test');
+-----------------------+
| strcmp('test','test') |
+-----------------------+
|                     0 |
+-----------------------+
1 row in set (0.00 sec)

root@localhost : wing 12:08:54> select strcmp('test','tast');
+-----------------------+
| strcmp('test','tast') |
+-----------------------+
|                     1 |
+-----------------------+
1 row in set (0.00 sec)
```

赋值操作
--------
1. ':='不同于=之处在于,':='操作从来不解释为一个比较操作符,所以可以使用':='操作符将任何一个有效的SQL语句赋值到变量中。  
2. '='只能用于两个场景下：  
SET语句中'='相当于':='  
EXAMPLE

```sql
# 未赋值成功,=相当于比较操作 
root@localhost : wing 01:10:02> select @a=1;    
+------+
| @a=1 |
+------+
| NULL |
+------+
1 row in set (0.00 sec)

# 赋值成功
root@localhost : wing 01:25:05> select @a:=1;
+-------+
| @a:=1 |
+-------+
|     1 |
+-------+
1 row in set (0.00 sec)
```

控制流函数
----------
##### CASE
**语法**  
```sql
CASE value WHEN [compare_value] THEN result [WHEN [compare_value] THEN result ...] [ELSE result] END
```
```sql
CASE WHEN [condition] THEN result [WHEN [condition] THEN result ...] [ELSE result] END
```
**实战演练**

```sql
root@localhost : wing 02:07:15> select * from fun_data;
+------+--------+
| id   | name   |
+------+--------+
|    1 | MySQL  |
|    2 | Oracle |
|    3 | Python |
| NULL | SHELL  |
|    5 | NULL   |
+------+--------+
5 rows in set (0.00 sec)

# CASE WHEN ...语句实战演练
root@localhost : wing 02:07:14> select case when id is null then 0 else id end from fun_data;
+-----------------------------------------+
| case when id is null then 0 else id end |
+-----------------------------------------+
|                                       1 |
|                                       2 |
|                                       3 |
|                                       0 |
|                                       5 |
+-----------------------------------------+
5 rows in set (0.00 sec)

# CASE value WHEN ...语句实战演练
root@localhost : wing 02:08:46> select case name when 'SHELL' then 'LINUX' else name end from fun_data;
+---------------------------------------------------+
| case name when 'SHELL' then 'LINUX' else name end |
+---------------------------------------------------+
| MySQL                                             |
| Oracle                                            |
| Python                                            |
| LINUX                                             |
| NULL                                              |
+---------------------------------------------------+
5 rows in set (0.00 sec)
```

##### IF(expr1,expr2,expr3)
如果expr1为TRUE,即expr1<>0以及expr1<>NULL,此时返回expr2;  
如果expr1为FALSE,即exor1 is null或expr1=0,此时返回expr3。  

##### IFNULL(expr1,expr2)
如果expr1为NULL值,此时返回expr2;  
如果expr1不为NULL值,此时返回expr1。  

```sql
root@localhost : wing 04:15:44> select ifnull(null,0);
+----------------+
| ifnull(null,0) |
+----------------+
|              0 |
+----------------+
1 row in set (0.00 sec)

root@localhost : wing 04:34:18> select ifnull(3,0);
+-------------+
| ifnull(3,0) |
+-------------+
|           3 |
+-------------+
1 row in set (0.00 sec)

root@localhost : wing 04:34:23> select ifnull('test',0);
+------------------+
| ifnull('test',0) |
+------------------+
| test             |
+------------------+
1 row in set (0.00 sec)

root@localhost : wing 04:34:30> select ifnull(0,1);
+-------------+
| ifnull(0,1) |
+-------------+
|           0 |
+-------------+
1 row in set (0.00 sec)
```

##### NULLIF(expr1,expr2)
如果expr1=exor2,则返回NULL;  
如果expr1<>expr2,则返回expr1。  

```sql
root@localhost : wing 04:37:56> select nullif(null,null);
+-------------------+
| nullif(null,null) |
+-------------------+
| NULL              |
+-------------------+
1 row in set (0.00 sec)

root@localhost : wing 04:39:06> select nullif(1024,1024);
+-------------------+
| nullif(1024,1024) |
+-------------------+
|              NULL |
+-------------------+
1 row in set (0.00 sec)

root@localhost : wing 04:39:18> select nullif(1024,512);
+------------------+
| nullif(1024,512) |
+------------------+
|             1024 |
+------------------+
1 row in set (0.00 sec)

root@localhost : wing 04:39:24> select nullif(512,1024);
+------------------+
| nullif(512,1024) |
+------------------+
|              512 |
+------------------+
1 row in set (0.00 sec)
```

字符串函数
----------
##### LENGTH()
返回字符串的字节数  

##### CHAR_LENGTH()
返回字符串的字符数  

##### BIT_LENGTH()
返回字符串的位数  

```sql
root@localhost : wing 04:48:06> select char_length('MySQL数据库');
+-------------------------------+
| char_length('MySQL数据库')    |
+-------------------------------+
|                             8 |
+-------------------------------+
1 row in set (0.00 sec)

root@localhost : wing 04:48:21> select length('MySQL数据库');
+--------------------------+
| length('MySQL数据库')    |
+--------------------------+
|                       14 |
+--------------------------+
1 row in set (0.00 sec)

root@localhost : wing 04:48:25> select bit_length('MySQL数据库');
+------------------------------+
| bit_length('MySQL数据库')    |
+------------------------------+
|                          112 |
+------------------------------+
1 row in set (0.00 sec)
```

##### CONCAT()
对字符串进行合并  

```sql
root@localhost : wing 04:50:15> select concat('MySQL','数据库');
+-----------------------------+
| concat('MySQL','数据库')    |
+-----------------------------+
| MySQL数据库                 |
+-----------------------------+
1 row in set (0.00 sec)
```

##### ELT(expr1,expr2....)
根据expr1选择expr2....对应的数据  

```sql
root@localhost : wing 05:23:19> select elt(1,'MySQL','Oracle','Python');
+----------------------------------+
| elt(1,'MySQL','Oracle','Python') |
+----------------------------------+
| MySQL                            |
+----------------------------------+
1 row in set (0.00 sec)

root@localhost : wing 05:23:41> select elt(2,'MySQL','Oracle','Python');
+----------------------------------+
| elt(2,'MySQL','Oracle','Python') |
+----------------------------------+
| Oracle                           |
+----------------------------------+
1 row in set (0.00 sec)

root@localhost : wing 05:23:45> select elt(0,'MySQL','Oracle','Python');
+----------------------------------+
| elt(0,'MySQL','Oracle','Python') |
+----------------------------------+
| NULL                             |
+----------------------------------+
1 row in set (0.00 sec)

root@localhost : wing 05:23:49> select elt(5,'MySQL','Oracle','Python');
+----------------------------------+
| elt(5,'MySQL','Oracle','Python') |
+----------------------------------+
| NULL                             |
+----------------------------------+
1 row in set (0.00 sec)

root@localhost : wing 05:23:54> select elt(,'MySQL','Oracle','Python');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''MySQL','Oracle','Python')' at line 1
root@localhost : wing 05:23:59> select elt(NULL,'MySQL','Oracle','Python');
+-------------------------------------+
| elt(NULL,'MySQL','Oracle','Python') |
+-------------------------------------+
| NULL                                |
+-------------------------------------+
1 row in set (0.00 sec)
```

##### FORMAT(X,D)
数值的输出结果格式,X类似于‘#,###,###.##’,D为小数点输出的精度。  

```sql
root@localhost : wing 05:37:31> select format(837485938.3,4);
+-----------------------+
| format(837485938.3,4) |
+-----------------------+
| 837,485,938.3000      |
+-----------------------+
1 row in set (0.00 sec)

root@localhost : wing 05:37:48> select format(837485938.3);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ')' at line 1
root@localhost : wing 05:37:54> select format(837485938.3,0);
+-----------------------+
| format(837485938.3,0) |
+-----------------------+
| 837,485,938           |
+-----------------------+
1 row in set (0.00 sec)
```

##### LCASE() && LOWER()
LCASE()与LOWER()功能相同,将字符串所有字母小写  

```sql
root@localhost : wing 05:37:58> select lower('MySQL');
+----------------+
| lower('MySQL') |
+----------------+
| mysql          |
+----------------+
1 row in set (0.00 sec)
```

##### LEFT(str,len) && RIGHT(str,len)
分别为返回从左边开始返回str第len个字母，从右边开始返回str第len个字母  

```sql
root@localhost : wing 05:41:08> select left('MySQL',1),RIGHT('MySQL',1);
+-----------------+------------------+
| left('MySQL',1) | RIGHT('MySQL',1) |
+-----------------+------------------+
| M               | L                |
+-----------------+------------------+
1 row in set (0.00 sec)
```

##### LOCATE(substr,str),LOCATE(substr,str,pos)
LOCATE(substr,str)返回substr在str中第一次出现的位置  
LOCATE(substr,str,pos)返回sunstr在str中pos位置后第一次出现的位置  
类似函数有POSITION()  

```sql
root@localhost : wing 05:55:13> select locate('sql','MySQLSQL');
+--------------------------+
| locate('sql','MySQLSQL') |
+--------------------------+
|                        3 |
+--------------------------+
1 row in set (0.00 sec)

root@localhost : wing 05:55:18> select locate('SQL','MySQLSQL');
+--------------------------+
| locate('SQL','MySQLSQL') |
+--------------------------+
|                        3 |
+--------------------------+
1 row in set (0.00 sec)

root@localhost : wing 05:55:26> select locate('SQL','MySQLSQL',4);
+----------------------------+
| locate('SQL','MySQLSQL',4) |
+----------------------------+
|                          6 |
+----------------------------+
1 row in set (0.00 sec)
```

##### LTRIM() && RTRIM() && TRIM()
LTRIM()返回删除左边空格的字符串  
RTRIM()返回删除右边空格的字符串  
TRIM()返回删除左右空格的字符串  

```sql
root@localhost : wing 06:12:24> select ltrim('   MySQL   ');
+----------------------+
| ltrim('   MySQL   ') |
+----------------------+
| MySQL                |
+----------------------+
1 row in set (0.00 sec)

root@localhost : wing 06:12:31> select rtrim('   MySQL   ');
+----------------------+
| rtrim('   MySQL   ') |
+----------------------+
|    MySQL             |
+----------------------+
1 row in set (0.00 sec)

root@localhost : wing 06:12:34> select trim('   MySQL   ');
+---------------------+
| trim('   MySQL   ') |
+---------------------+
| MySQL               |
+---------------------+
1 row in set (0.00 sec)
```

##### repeat(str,count)
重复count次str

```sql
root@localhost : wing 06:04:49> select repeat('MySQL',2);
+-------------------+
| repeat('MySQL',2) |
+-------------------+
| MySQLMySQL        |
+-------------------+
1 row in set (0.00 sec)


root@localhost : wing 06:04:56> select repeat(null,2);
+----------------+
| repeat(null,2) |
+----------------+
| NULL           |
+----------------+
1 row in set (0.00 sec
```

##### replace(str,from_str,to_str)
将str中的from_str替换为to_str

```sql
root@localhost : wing 06:07:13> Select replace('www.mysql.com','mysql','MySQL');
+------------------------------------------+
| replace('www.mysql.com','mysql','MySQL') |
+------------------------------------------+
| www.MySQL.com                            |
+------------------------------------------+
1 row in set (0.00 sec)
```

##### reverse(str)
将str逆序输出  

```sql
root@localhost : wing 06:08:26> select reverse('MySQL');
+------------------+
| reverse('MySQL') |
+------------------+
| LQSyM            |
+------------------+
1 row in set (0.00 sec)
```

##### SPACE(n)
返回n个空格  

```sql
root@localhost : wing 06:12:38> select space(6);
+----------+
| space(6) |
+----------+
|          |
+----------+
1 row in set (0.00 sec)

root@localhost : wing 06:15:52> select space(100);
+------------------------------------------------------------------------------------------------------+
| space(100)                                                                                           |
+------------------------------------------------------------------------------------------------------+
|                                                                                                      |
+------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

##### UCASE() && UPPER()
将str大写  

```sql
root@localhost : wing 06:16:11> select UCASE('python'),UPPER('python');
+-----------------+-----------------+
| UCASE('python') | UPPER('python') |
+-----------------+-----------------+
| PYTHON          | PYTHON          |
+-----------------+-----------------+
1 row in set (0.00 sec)
```

数值函数
--------
##### CEIL() & CEILING()
两者功能相同，返回比给出的参数大的最小的整数。  

```sql
root@localhost : wing 04:18:06> select ceil(9.99);
+------------+
| ceil(9.99) |
+------------+
|         10 |
+------------+
1 row in set (0.00 sec)

root@localhost : wing 04:18:20> select ceiling(9.99);
+---------------+
| ceiling(9.99) |
+---------------+
|            10 |
+---------------+
1 row in set (0.00 sec)

root@localhost : wing 04:18:24> select ceil(null);
+------------+
| ceil(null) |
+------------+
|       NULL |
+------------+
1 row in set (0.00 sec)

root@localhost : wing 04:18:29> select ceiling(null);
+---------------+
| ceiling(null) |
+---------------+
|          NULL |
+---------------+
1 row in set (0.00 sec)

root@localhost : wing 04:18:32> select ceil(0);
+---------+
| ceil(0) |
+---------+
|       0 |
+---------+
1 row in set (0.00 sec)

root@localhost : wing 04:18:36> select ceiling(-9.99);
+----------------+
| ceiling(-9.99) |
+----------------+
|             -9 |
+----------------+
1 row in set (0.00 sec)
```

##### CRC32()
用于循环冗余校验，返回一个32位无符号数值，是对一个传送的数据块进行校验，是一种高效的差错控制方法。  

```sql
root@localhost : wing 04:21:43> select crc32('MySQL');
+----------------+
| crc32('MySQL') |
+----------------+
|     3259397556 |
+----------------+
1 row in set (0.00 sec)

root@localhost : wing 04:23:41> select crc32('mysql');
+----------------+
| crc32('mysql') |
+----------------+
|     2501908538 |
+----------------+
1 row in set (0.00 sec)
```

##### FLOOR()
返回比参数小的最大的整数。  

```sql
root@localhost : wing 04:41:32> select floor(1.23);
+-------------+
| floor(1.23) |
+-------------+
|           1 |
+-------------+
1 row in set (0.00 sec)

root@localhost : wing 04:41:39> select floor(0);
+----------+
| floor(0) |
+----------+
|        0 |
+----------+
1 row in set (0.00 sec)

root@localhost : wing 04:41:42> select floor(null);
+-------------+
| floor(null) |
+-------------+
|        NULL |
+-------------+
1 row in set (0.00 sec)

root@localhost : wing 04:41:47> select floor(-1.23);
+--------------+
| floor(-1.23) |
+--------------+
|           -2 |
+--------------+
1 row in set (0.00 sec)
```

##### POW(x,y) && POWER(x,y)
两者功能相同，求x的y次方  

```sql
root@localhost : wing 04:49:54> select power(2,4);
+------------+
| power(2,4) |
+------------+
|         16 |
+------------+
1 row in set (0.00 sec)
```

##### RAND() && RAND(N)
1. 不论是同一个事务中还是非同一个事务中,包含RAND()函数的SQL语句之前都会记录两个会话级的参数RAND_SEED1和RAND_SEED2，由这两个参数根据RAND()产生随机数的算法便可得到一个确定的数值，所以即使在binlog_format=STATEMENT模式下，主从复制之间使用RAND()函数也可以确保数据一致;  
http://puppetfans.vicp.cc:8090/pages/viewpage.action?pageId=4718827  
2. RAND()函数为产生随机数在[0,1)之间;  
3. 对于RAND(N),参数N用作种子值,种子值即binlog日志中RAND_SEED1和RAND_SEED2,产生相同的随机数。  
4. 

```sql
root@localhost : wing 05:08:51> select * from rand_data;
+------+
| id   |
+------+
|    1 |
|    2 |
|    3 |
+------+
3 rows in set (0.00 sec)

# 在如下示例中可观察到，rand()的函数产生的随机数随机改变，rand(n)的函数产生的随机数是确定值
root@localhost : wing 05:08:58> select id , rand() from rand_data;
+------+---------------------+
| id   | rand()              |
+------+---------------------+
|    1 | 0.07249362959758716 |
|    2 |  0.3962498189846518 |
|    3 |  0.7637682368641423 |
+------+---------------------+
3 rows in set (0.00 sec)

root@localhost : wing 05:09:08> select id , rand(1) from rand_data;
+------+---------------------+
| id   | rand(1)             |
+------+---------------------+
|    1 | 0.40540353712197724 |
|    2 |  0.8716141803857071 |
|    3 |  0.1418603212962489 |
+------+---------------------+
3 rows in set (0.00 sec)

root@localhost : wing 05:09:14> select id , rand() from rand_data;
+------+--------------------+
| id   | rand()             |
+------+--------------------+
|    1 | 0.6300917581004014 |
|    2 | 0.8591541106432249 |
|    3 | 0.4054953096485653 |
+------+--------------------+
3 rows in set (0.00 sec)

root@localhost : wing 05:09:21> select id , rand(1) from rand_data;
+------+---------------------+
| id   | rand(1)             |
+------+---------------------+
|    1 | 0.40540353712197724 |
|    2 |  0.8716141803857071 |
|    3 |  0.1418603212962489 |
+------+---------------------+
3 rows in set (0.00 sec)
```













