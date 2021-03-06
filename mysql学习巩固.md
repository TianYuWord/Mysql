# Mysql学习巩固

本节课内容
搞清楚：
什么是存储过程？
什么是存储函数？
它们的作用是什么？
编码实现一个存储过程。

他们的区别是什么？
函数：有返回值的过程
过程：没有返回值的函数

使用存储过程和存储函数有什么优势？
使用存储过程可以减少应用程序和数据库之间的交互。
存储过程和函数是 实现经过编译并存储在数据库中的一段SQL语句的集合，调用存储过程和存储函数可以简化应用开人员的很多工作，
减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的，

在使用mysql过程中，会遇到在编写过程语句的时候 ";" 导致把sql语句结束了，结束之后就会导致语句不完整
-切换";"为"$"
```
delimiter $ //使用delimter 把 ";" 分隔符换为 $ 以保证在编写存储函数的时候不会报错 , 注意该语句不可以打;结尾
```

## 创建存储过程
```
CREATE PROCEDURE procedure_name ([proc_parameter[...]])
begizn
	sql 语句
end;
```
### 创建一个实例
```
CREATE PROCEDURE procedure_name()
begin
sleect "Hello Procedure"
end
```

### 如何调用呢？
```
call procedure_name();$ 就可以调用了
```
执行结果
```
+-----------------+
| Hello Procedure |
+-----------------+
| Hello Procedure |
+-----------------+
```

### 查询某个数据库的存储过程
```
select name from mysql.proc where db = "数据库名";
```
执行结果：查出了在该数据库创建的 存储过程 2 个 ， 因为我就创建了两个存储过程
```
+----------------+
| name           |
+----------------+
| procedure_name |
| pro_name       |
+----------------+
```
不过这种方式查出的存储过程并不是很详细
### 详细的查出创建过的PROCEDURE存储过程（可在为选中数据库的情况下执行）
```
show procedure status;
```
执行结果
```
+-------+----------------+-----------+----------------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| Db    | Name           | Type      | Definer        | Modified            | Created             | Security_type | Comment | character_set_client | collation_connection | Database Collation |
+-------+----------------+-----------+----------------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| user2 | procedure_name | PROCEDURE | root@127.0.0.1 | 2020-03-19 15:52:11 | 2020-03-19 15:52:11 | DEFINER       |         | gbk                  | gbk_chinese_ci       | latin1_swedish_ci  |
| user2 | pro_name       | PROCEDURE | root@127.0.0.1 | 2020-03-19 15:58:17 | 2020-03-19 15:58:17 | DEFINER       |         | gbk                  | gbk_chinese_ci       | latin1_swedish_ci  |
+-------+----------------+-----------+----------------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
```
### 查询指定PROCEDURE存储过程（在选中数据库的情况下执行）
```
shwo create procedure pro_name\G$
```
执行结果
```
MariaDB [user2]> show create procedure pro_name\G;
*************************** 1. row ***************************
           Procedure: pro_name
            sql_mode: NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
    Create Procedure: CREATE DEFINER=`root`@`127.0.0.1` PROCEDURE `pro_name`()
begin
select "Hello Procedure";
end
character_set_client: gbk
collation_connection: gbk_chinese_ci
  Database Collation: latin1_swedish_ci
1 row in set (0.00 sec)
```
## 删除存储过程
```
drop procedure pro_name$
```
执行结果
```
Query OK, 0 rows affected (0.14 sec)
```
再次查询
```
MariaDB [user2]> select name from mysql.proc where db="user2";
+----------------+
| name           |
+----------------+
| procedure_name |
+----------------+
```
发现此时就只有一个procedure存储过程了。