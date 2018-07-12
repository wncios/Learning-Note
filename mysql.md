# 1、SQL语言主要分为三类：
- DDL：Data Defination Language，数据定义语言，用来维护存储数据的结构（数据库、表），例如create、drop和alter操作
- DML：Data  Manipulation Language，数据操作语言，用来对数据进行操作，例如delete、update、insert，又可细分为DQL（Data Query Language），数据查询语言，例如select
- DCL：Data Control Language，数据控制语言，用于负责用户权限管理，例如grant、revoke等
- - - - -
# 2、库操作
## 2.1 新增数据库

基本语法：
``` SQL
create database 数据库名称 数据库选项;
```
数据库选项：
- 字符集设定：charset + 字符集，用来表示数据存储的编码格式，常用的字符集包括GBK和UTF8等
- 校对集设定：collate + 具体校对集，表示数据比较的规则，其依赖字符集
实例：
``` SQL
create database mydatabase charset utf8;
```
注意：
- 数据库名称最好别不要使用中文
- 数据库名称不要用关键字和保留字
- 如果要使用关键字和保留字，用反引号括起来

- - - - -
## 2.2 查询数据库
基本语法：
``` SQL
show databases;
```
模糊查询语法：
``` SQL
show databases like 'pattern';
```
其中，pattern是匹配模式，有两种，分别是：
- %：表示匹配多个字符
- _：表示匹配单个字符

如果库名中有_时，要用\_进行转义。
- - - - -
## 2.3 更新数据库
基本语法：
``` SQL
alter dabase 数据库名称 数据选项
```
实例：
``` SQL
alter database mydatabase charset gdk;
```
注意：
- 数据库名称不可更改
- - - - -
## 2.4 删除数据库
基本语法：
``` SQL
drop database 数据库名称;
```
注意：
- 删除是不可逆操作，删除之前要先备份数据库
- - - - -
# 3、表操作
## 3.1 新增表

基本语法：
``` SQL
create table [if not exists] 表名(
    字段名称 数据类型;
    ...
    字段名称 数据类型
)[表选项];
```
其中，if not exists 表示：
- 如果表名不存在，就执行创建代码；如果表名存在，则不执行创建代码。

表选项则是用来控制表的表现形式的，共有三种，分别为：
- 字符集设定：charset 具体字符集，用来指定存储的数据的编码格式，常用的有GBK和UTF8
- 校对集设定：collate 具体校对集，表示数据比较的规则，其依赖字符集
- 存储引擎：engine 具体存储引擎，默认为InnoDB，常用的还有MyISAM

实例一：实现的指定表所属的数据库
``` SQL
create table if not exists test.Student(
    name varchar(10),
    age int,
    grade varchar(10)
) engine InnoDB charset utf8;
```
实例二：隐式的指定表所属的数据库
``` SQL
use test;
create table if not exists Student(
    name varchar(10),
    age int,
    grade varchar(10)
) engine InnoDB charset utf8;
```
- - - - -
## 3.2 查询表
基本语法：
``` SQL
show tables;
```
模糊查询语法：
``` SQL
show tables like 'pattern';
```
其中，pattern是匹配模式，有两种，分别是：
- %：表示匹配多个字符
- _：表示匹配单个字符

查看表中的字段信息：
``` c++
desc/describe/show columns from 表名;
```
- - - - -
## 3.3 更新表
表的修改，分为修改表本身和修改表中的字段。
第1类：修改表本身
- 修改表名，基本语法：
``` SQL
rename table 旧表名 to 新表名;
```
- 修改表选项，基本语法：
``` SQL
alter table 表名 表选项[=] 值;
```
第2类：修改表中的字段，新增、修改、重命名和删除
- 新增字段，基本语法：
``` SQL
alter table 表名 add [column] 字段名 数据类型 [列属性][位置];
```
其中，位置表示此字段存储的位置，分为first（第一个位置）和after + 字段名（指定的字段后，默认为最后一个位置）。
实例：
``` SQL
alter table Student add column id int first;
```
- 修改字段，基本语法：
``` SQL
alter table 表名 modify 字段名 数据类型 [列属性][位置];
```
实例：
``` SQL
alter table Student modify name char(10) after id;
```
- 重命名字段，基本语法：
``` SQL
alter table 表名 change 旧字段名 新字段名 数据类型 [列属性][位置];
```
实例：
``` SQL
alter table Student change grage class varchar(10);
```
- - - - -
- 删除字段，基本语法：
``` SQL
alter table 表名 drop 字段名;
```
实例：
``` SQL
alter table Student drop age;
```
注意：
- 如果表中已经存在数据，那么删除该字段会清空该字段所有的数据，且不可逆，慎用。
- - - - -
## 3.4 删除表
基本语法：
``` SQL
drop table 表1, 表2;
```
 注意：
- 删除表为不可逆操作，慎用。
- - - - -
# 4、数据操作
SQL的基本操作CURD，即增删改查
## 4.1 新增数据
对于数据的新增操作，有两种方法。
第1种：给全表字段插入数据，不需要指定字段列表，但要求数据的值出现的顺序与表中的字段出现的顺序一致，而且凡是非数值数据，都需要用引号括起来。
基本语法：
``` SQL
insert into 表名 values(值列表[,(值列表)]);
```
实例：
``` SQL
insert into Student values(1, 'Harlon', 'class-1');
```
第2种：给部分字段插入数据，需要选定字段列表，字段列表中字段出现的顺序与表中字段的顺序无关，但值列表的顺序必须与字段列表的保持一致。
基本语法：
``` SQL
insert into 表名(字段列表) values(值列表[,(值列表)]);
```
实例：
``` SQL
insert into Student(name, class) values('Juice', 'class-2');
```
- - - - -
## 4.2 查询数据
基本语法：
``` SQL
select * from 表名 [where 条件];
```
查看部分，基本语法：
``` SQL
select 字段名称[,字段名称] from 表名 [where 条件];
```
实例：
``` SQL
select name, class from Student;
```
- - - - -
## 4.3 更新数据
基础语法：
``` SQL
update 表名 字段 = 值 [where 条件];
```
在这里，尽量加上where条件，否则的话，操作就是全表数据。
实例：
``` SQL
update Student set class = 'class-2' where id=1;
```
- - - - -
## 4.3 删除数据
基本语法：
``` SQL
delete from 表名 [where 条件];
```
实例：
``` SQL
delete from Strudent where name='Juice';
```
我们也可以用drop实现删除操作，不过与delete相比，drop威力更强，其在执行删除操作的时候，不仅会删除数据，还会删除定义并释放存储空间；而delete删除的时候，仅会删除数据，并不会删除定义和释放存储空间。
- - - - -
# 5、字符集问题
查看服务器识别的字符集
``` SQL
show character set;
```
查看服务器默认的对外处理的字符集
``` SQL
show variables like 'character_set%';
```
修改字符传输方式：
``` SQL
set names utf8;
```
- - - - -
# 6、校对集问题
校对集有三种：
- _bin：binary，二进制比较，区分大小写
- _cs：case sensitive，大小写敏感，区分大小写
- _ci：case insensitive，大小写不敏感，不区分大小写
查看全部校对集：
``` SQL
show collation;
```
注意：
- 如果在没有数据之前没有声明校对集，在有了数据之后，再对校对集进行修改，则修改无效

- - - - -
# 7、数值型
在SQL中，将数据类型分为三大类，分别为：数值型、字符串型和日期时间型
![](https://img-blog.csdn.net/20170505201016682)
## 7.1 整数型
在SQL中，由于要考虑节省磁盘空间的问题，因此系统又将整型分配五类，分别为：
- tinyint：迷你整型，使用1个字节存储数据（常用）
- smallint：小整型，使用2个字节存储数据
- mediumint：中整型，使用3个字节存储数据
- int：标准整型，使用4个字节存储数据（常用）
- bigint：大整型，使用8个字节存储数据

实例：
``` SQL
create table my_int( 
    int_1 tinyint, 
    int_2 smallint, 
    int_3 int, 
    int_4 bigint 
) engine InnoDB charset utf8;
```
``` SQL
mysql> desc my_int;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| int_1 | tinyint(4)  | YES  |     | NULL    |       |
| int_2 | smallint(6) | YES  |     | NULL    |       |
| int_3 | int(11)     | YES  |     | NULL    |       |
| int_4 | bigint(20)  | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```
每个字段的数据后面都会跟一堆括号，并且里面含有数字，这表示数据的显示宽度。
``` SQL
mysql> alter table my_int add int_5 tinyint zerofill;
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0
mysql> desc my_int;
+-------+------------------------------+------+-----+---------+-------+
| Field | Type                         | Null | Key | Default | Extra |
+-------+------------------------------+------+-----+---------+-------+
| int_1 | tinyint(4)                   | YES  |     | NULL    |       |
| int_2 | smallint(6)                  | YES  |     | NULL    |       |
| int_3 | int(11)                      | YES  |     | NULL    |       |
| int_4 | bigint(20)                   | YES  |     | NULL    |       |
| int_5 | tinyint(3) unsigned zerofill | YES  |     | NULL    |       |
+-------+------------------------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```
- - - - -
## 7.2 小数型
在SQL中，将小数型分为浮点型和顶点型两种，其中：
- 浮点型：小数点浮动，精度有限，容易丢失精度
- 定点型：小数点固定，精度固定，不会丢失精度

第1种：浮点型
浮点型数据是一种精度型数据，因为超出指定范围之后，其会丢失精度，自动进行四舍五入操作，分为两种精度：
- float：单精度，占用4个字节存储数据，精度范围大概7位左右
- double：双精度，占用8个字节存储数据，精度范围15位左右

浮点型使用方式：如果直接使用float，则表示没有小数部分；如果用float(M, D)，其中M代表总长度，D代表小数部分长度，M-D则为证书部分长度。
实例：
``` SQL
create table my_float(
    f1 float,
    f2 float(10, 2),
    f3 float(6, 2)
) engine InnoDB charset utf8;
```
插入浮点数时，可以直接插入小数，也可以插入科学计数法表示的数据，此外，插入浮点型数据时，整数部分不能超出长度范围，但小数部分可以超出长度范围的，系统会自动进行四舍五入操作。
``` SQL
mysql> insert into my_float values(2.15e4, 20.15, 9999.99);
Query OK, 1 row affected (0.00 sec)

mysql> insert into my_float values(20180710, 33.338888, 9999.99);
Query OK, 1 row affected (0.00 sec)

mysql> select * from my_float;
+----------+-------+---------+
| f1       | f2    | f3      |
+----------+-------+---------+
|    21500 | 20.15 | 9999.99 |
| 20180700 | 33.34 | 9999.99 |
+----------+-------+---------+
2 rows in set (0.00 sec)
```
第2种：定点型
定点型数据，绝对的保证整数部分不会被四舍五入，也就是说不会丢失精度，但小数部分有可能会丢失精度，虽然理论上小数部分也不会丢失精度。
实例：
``` SQL
create table my_decimal (
    f1 float(10, 2),
    d1 decimal(10, 2)
) engine InnoDB charset utf8;
```
``` SQL
mysql> insert into my_decimal values(99999999.99, 99999999.99);
Query OK, 1 row affected (0.01 sec)

mysql> insert into my_decimal values(123456.99, 2018.1364);
Query OK, 1 row affected, 1 warning (0.00 sec)

mysql> select * from my_decimal;
+--------------+-------------+
| f1           | d1          |
+--------------+-------------+
| 100000000.00 | 99999999.99 |
|    123456.99 |     2018.14 |
+--------------+-------------+
2 rows in set (0.00 sec)
```
- - - - -
# 8、日期时间类型
日期时间类型共有物种类型，分别为：
- datetime：日期时间，其格式为yyyy-MM-dd HH:mm:ss，表示的范围是1000年到9999年，有零值，即0000-00-00 00:00:00
- date：日期，就是datetime的date部分
- time：时间，或者说是时间段，为指定的某个时间区间之间，包含正负时间；
- timestamp：时间戳，但并不是真正意义上的时间戳，其是从1970年开始计算的，格式和datetime一致
- year：年份，有两种格式，分别为year(2)和year(4)

实例：
``` SQL
create table my_date(
    d1 datetime,
    d2 date,
    d3 time,
    d4 timestamp,
    d5 year
) engine InnoDB charset utf8;
```
日期时间型中的time，可以为负数，甚至可以是很大的负数；year，可以使用 2 位数据插入，也可以使用 4 位数据插入；timestamp，只要当前所在的记录被更新，该字段就会自动更新为当前时间，且时间戳类型默认为非空的。
``` SQL
mysql> insert into my_date values('2018-07-10 13:15:00', '2017-07-10', '18:49:00', '2018-07-10 13:15:00', 2018);
Query OK, 1 row affected (0.00 sec)

mysql> insert into my_date values('2018-07-10 13:15:00', '2017-07-10', '-135:49:00', '2018-07-10 13:15:00', 69);
Query OK, 1 row affected (0.01 sec)

mysql> insert into my_date values('2018-07-10 13:15:00', '2017-07-10', '-2 35:49:00', '2018-07-10 13:15:00', 70);
Query OK, 1 row affected (0.04 sec)

mysql> select * from my_date;
+---------------------+------------+------------+---------------------+------+
| d1                  | d2         | d3         | d4                  | d5   |
+---------------------+------------+------------+---------------------+------+
| 2018-07-10 13:15:00 | 2017-07-10 | 18:49:00   | 2018-07-10 13:15:00 | 2018 |
| 2018-07-10 13:15:00 | 2017-07-10 | -135:49:00 | 2018-07-10 13:15:00 | 2069 |
| 2018-07-10 13:15:00 | 2017-07-10 | -83:49:00  | 2018-07-10 13:15:00 | 1970 |
+---------------------+------------+------------+---------------------+------+
3 rows in set (0.00 sec)
```
``` SQL
mysql> update my_date set d1 = '2017-07-10 18:54:00' where d5 = 1970;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from my_date;
+---------------------+------------+------------+---------------------+------+
| d1                  | d2         | d3         | d4                  | d5   |
+---------------------+------------+------------+---------------------+------+
| 2018-07-10 13:15:00 | 2017-07-10 | 18:49:00   | 2018-07-10 13:15:00 | 2018 |
| 2018-07-10 13:15:00 | 2017-07-10 | -135:49:00 | 2018-07-10 13:15:00 | 2069 |
| 2017-07-10 18:54:00 | 2017-07-10 | -83:49:00  | 2018-07-10 18:54:50 | 1970 |
+---------------------+------------+------------+---------------------+------+
3 rows in set (0.00 sec)
```
- - - - -
# 9、字符类型
在SQL中，将字符串分为6类，分别为：char、varchar、text、blob、enum和set
第1类：定长字符串
定长字符串：char，即磁盘在定义结构的时候就确定了最终数据的长度。
- char(L)：L表示length，即存储的长度，最大长度为255
- char(4)：表示在utf8情况下，需要4*3 = 12字节

第2类：变长字符串
变长字符串：varchar，即在分配空间的时候，按照最大的空间进行分配，但是实际用了多少，则是根据具体的数据来确定。
- varchar(L)：L表示length，理论长度是65536，但是会多出来1到2个字节来确定存储的实际长度
- varchar(10)：例如存储10个汉字，在UTF8环境下，需要10*3+1 = 31个字节
实际上，如果超过255个字符，用text。
如何选择定长字符串或者变长字符串呢？
- 定长字符串对磁盘空间比较浪费，但是效率高；如果数据基本上确定长度都一样，就使用定长字符串，例如身份证、电话号码等
- 变长字符串对磁盘空间比较节省，但是效率低；如果数据不能确定长度（不同的数据有变化），就使用变长字符串，例如地址、姓名等

第3类：文本字符串
如果数据量非常大，通常说超过255个字符就会使用文本字符串。
文本字符串根据存储的格式分类，可以分为：
- text：存储文字
- blob：存储二进制数据（其实际上都是存储路径），通常不用

第4类：枚举字符串
枚举字符串：enum，需要事先将可能出现的结果都设计好，实际上存储的数据必须是规定好的数据中的一个。
枚举字符串的使用方式：
- 定义：enum('元素1', '元素2', '元素3', ...)
- 使用：存储的数据，只能是事先定义好的数据

实例：
``` SQL
create table my_enum(
    gender enum('男', '女', '保密')
) engine InnoDB charset utf8;
```
``` SQL
mysql> insert into my_enum values('男'), ('女'), ('保密');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> insert into my_enum values('male');
ERROR 1265 (01000): Data truncated for column 'gender' at row 1
mysql> select * from my_enum;
+--------+
| gender |
+--------+
| 男     |
| 女     |
| 保密   |
+--------+
3 rows in set (0.00 sec)
```
枚举除了规范数据格式外，还能节省空间，因为枚举存储的是一个数值，而不是字符串本身。枚举的效率不高。

第5类：集合字符串
集合字符串：set，跟枚举类似，实际存储的是数值而不是字符串。
集合字符串的使用方式：
- 定义：set，元素列表；
- 使用：可以使用元素列表中的多个元素，用逗号分隔

实例：
``` SQL
create table my_set(
    hobby set('音乐', '电影', '旅行', '美食', '摄影', '运动', '宠物')
) engine InnoDB charset utf8;
```
``` SQL
mysql> insert into my_set values('电影,旅行,摄影');
Query OK, 1 row affected (0.00 sec)

mysql> insert into my_set values(3);
Query OK, 1 row affected (0.01 sec)

mysql> select * from my_set;
+----------------------+
| hobby                |
+----------------------+
| 电影,旅行,摄影       |
| 音乐,电影            |
+----------------------+
2 rows in set (0.00 sec)
```
同样，使用集合的效率并不高，但能规范数据和节省空间。
- - - - -
# 10、记录长度
MySQL规定：任何一条记录最长不超过65535个字节，这意味着varchar永远达不到理论最大值。
- - - - -
# 11、列属性
列属性：实际上，真正约束字段的数据类型，但是数据类型的约束比较单一，因此就需要额外的一些约束来保证数据的有效性，这就是列属性。
列属性有很多：例如：null、not null、default、primary key、unique key、auto_increment和comment等。

## 11.1 空属性
空属性有两个值，分别为：null和not null
虽然默认数据库的字段基本都为空，但是实际上真正开发的时候，要尽可能的保证数据不为空，因为空数据没有意义，也没办法参与运算。

实例：
``` SQL
create table my_class (
    grade varchar(20) not null,
    room varchar(20) null -- 显示声明为空，实际上，默认就为空
) engine InnoDB charset utf8;
```
``` SQL
mysql> desc my_class;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| grade | varchar(20) | NO   |     | NULL    |       |
| room  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
- - - - -
## 11.2 列描述
列描述：comment，表示描述，没有实际含义，是专门用来存储描述字段的，其会随着创建语句自动保存，用来给程序员（数据库管理员）了解数据库使用。

实例：
``` SQL
create table my_friend(
    name varchar(20) not null comment '姓名',
    age tinyint not null comment '年龄'
) engine InnoDB charset utf8;
```

``` SQL
mysql> desc my_friend;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| name  | varchar(20) | NO   |     | NULL    |       |
| age   | tinyint(4)  | NO   |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> show create table my_friend;
+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table     | Create Table                                                                                                                                                 |
+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| my_friend | CREATE TABLE `my_friend` (
  `name` varchar(20) NOT NULL COMMENT '姓名',
  `age` tinyint(4) NOT NULL COMMENT '年龄'
) ENGINE=InnoDB DEFAULT CHARSET=utf8     |
+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```
- - - - -

## 11.3 默认值
默认值：default，某一数据会经常性出现某个具体的值，因此可以在开始的时候就制定好，而在需要真实数据的时候，用户可以选择性的使用默认值。

实例：
``` SQL
create table my_default(
    name varchar(20) not null,
    age tinyint unsigned default 0,
    gender enum('男', '女') default '男'
) engine InnoDB charset utf8;
```

``` SQL
mysql> insert into my_default(name) values('Harlon');
Query OK, 1 row affected (0.00 sec)

mysql> insert into my_default values('Juice', 18, default);
Query OK, 1 row affected (0.00 sec)

mysql> select * from my_default;
+--------+------+--------+
| name   | age  | gender |
+--------+------+--------+
| Harlon |    0 | 男     |
| Juice  |   18 | 男     |
+--------+------+--------+
2 rows in set (0.00 sec)
```
- - - - -
## 11.4 主键
主键：primary key，表中主要的键，每张表只能有一个字段（复合主键，可以是多个字段）使用此属性，用来唯一的约束该字段里面的数据，不能重复。
增加主键：
在SQL操作中，有3种方法可以给表增加主键，分别为：
第1种：在创建表的时候，直接在字段之后，添加primary key关键字。

实例：
``` SQL
 create table my_pri (
    name varchar(20) not null comment '姓名',
    number char(10) primary key comment '学号'
) engine InnoDB charset utf8;
```
这种方法清晰明了，缺点是只能使用一个字段作为主键。

第2种：在创建表的时候，在所有的字段之后，使用primary key（主键字段列表）来创建主键（如果有多个字段作为主键，则称之为符合主键）

实例：
``` SQL
create table my_pri2(
    number char(10) not null comment '学号',
    course char(10) not null comment '课程编号',
    score tinyint unsigned default 60,
    primary key(number, course)
) engine InnoDB charset utf8;
```

第3种：当表创建之后，额外追加主键，可以直接追加主键，也可以通过修改字段的属性追回主键。

实例：
``` SQL
create table my_pri3 (
    course char(10) not null comment '课程编号',
    name varchar(10) not null comment '课程名称'
) engine InnoDB charset utf8;
```
追加主键的方式有两种：
- alter table my_pri3 modify course char(10) primary key comment '课程' -- 不建议使用
- alter table my_pri3 add primary key(course) -- 推荐使用

使用此方法的前提是表中的数据是不重复的，即保证唯一性。

更新主键 & 删除主键
对于主键，没有办法直接更新，主键必须先删除，然后才能更新
基本语法：
``` SQL
alter table 表名 drop primary key;
```

主键分类：
根据主键的字段类型，我们可以将主键分类两类，分别为：
- 业务主键：即使用真实的业务数据作为主键，例如学号、课程编号等等，很少使用
- 逻辑主键：即使用逻辑性的字段作为主键，字段没有业务含义，值有没有都没有关系，经常使用
- - - - -
## 11.5 自动增长
自动增长：auto_increment，当对应的字段，不给值，或者是默认值，或者是null的时候，就会自动的被系统触发，系统会从当前字段中取得已有的最大值再进行+1操作，得到新的字段值。
自增长通过跟主键进行搭配使用，其特点是：
- 任何字段要做自增长，前提其本身必须是一个索引，即key栏有值
- 自增必须是数字（整型）
- 每张表最多有一个自增长类型

实例：
``` SQL
create table my_auto (
    id int primary key auto_increment,
    name varchar(20) not null
) engine InnoDB charset utf8;
```
``` SQL
mysql> insert into my_auto values('Harlon');
ERROR 1136 (21S01): Column count doesn't match value count at row 1
mysql> insert into my_auto(name) values('Harlon');      
Query OK, 1 row affected (0.00 sec)

mysql> insert into my_auto values(null, 'Juice');
Query OK, 1 row affected (0.30 sec)

mysql> insert into my_auto values(default, 'Ellie'); 
Query OK, 1 row affected (0.01 sec)

mysql> select * from my_auto;
+----+--------+
| id | name   |
+----+--------+
|  1 | Harlon |
|  2 | Juice  |
|  3 | Ellie  |
+----+--------+
3 rows in set (0.00 sec)

mysql> show create table my_auto;
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table   | Create Table                                                                                                                                                               |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| my_auto | CREATE TABLE `my_auto` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8 |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```
修改自增长
自增长如果涉及到字段变化，必须先删除自增长，然后再增加增长，因为一个表只有有一个自增长字段。
如果修改当前自增长已经存在的值，则只能修改比当前已有自增长字段中的最大值更大，不能更小，因为更小不生效。
基本语法：
``` SQL
alter table 表名 auto_increment = 值;
```
``` SQL
mysql> show variables like 'auto_increment%';
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| auto_increment_increment | 1     |
| auto_increment_offset    | 1     |
+--------------------------+-------+
2 rows in set (0.00 sec)
```
其中auto_increment_increment表示自增长步长，auto_increment_offset表示自增长的初值。

删除自增长：
基本语法：
``` SQL
alter table 表名 modify 字段 类型
```

实例：
``` SQL
alter table my_auto modify id int;
```
- - - - -
## 11.6、唯一键
唯一键：每张表往往有多个字段需要具有唯一性，数据不能重复，但是在每个表中，只能有一个主键，因此唯一键就是用来解决表中多个字段具有唯一性的问题。
唯一键的本质与主键差不多，唯一键默认的允许字段为空，而且可以多个字段为空，因此空字段不参与唯一性的比较。

增加唯一性：
增加唯一性的方法与主键类型，有三种方法，分别为：
第一种：在创建表的时候，字段后面之间添加unique或者unique key关键字

实例：
``` SQL
create table my_unique (
    number char(10) unique comment '学号',
    name varchar(20) not null
) engine InnoDB charset utf8;
```

第二种：在所有字段之后，添加unique key(字段列表)，可以设置复合唯一键。

实例：
``` SQL
create table my_unique2 (
    number char(10) not null,
    name varchar(10) not null,
    unique key(number)
) engine InnoDB charset utf8;
```
当唯一键又非空时，就和主键的性质一样了。

第3种：在创建表之后，添加唯一键。

实例：
``` SQL
create table my_unique3 (
    id int primary key auto_increment,
    number char(10) not null,
    name varchar(10) not null    
) engine InnoDB charset utf8;
```
``` SQL
alter table my_unique3 add unique key(number);
```

唯一性约束：唯一键与主键本质上相同，区别在于：唯一键允许字段值为空，并且允许多个字段空值存在。

更新唯一键 & 删除唯一键
在表中，更新唯一键的时候，可以不用先删除唯一键，因为表的唯一键允许有多个。
删除唯一键的基本语法：
``` SQL
alter table 表名 drop index 索引名字
```

实例：
``` SQL
alter table my_unique3 drop index number;
```
- - - - -
# 12、索引
索引：系统根据某种算法，单独建立一个文件，根据这个文件能够快速的匹配数据，加快数据搜索。
索引的意义：
- 提高查询数据的效率
- 约束数据的有效性

MySQL中提供了多种索引，包括：
- 主键索引（primary key）
- 唯一键索引（unique key）
- 全文索引（fulltext index）
- 普通索引（index）

主键索引和唯一键索引，顾名思义就是在主键和唯一键字段上建立的索引，普通索引没有什么特色，就是为了加快数据查找。
全文索引，即根据文章内部的关键字进行索引，其最大的难度就是在于如何确定关键字。对于英文来说，全文索引的建立相对容易，因为英文的两个单词之间有空格；但是对于中文来说，全文索引的建立就比较难啦，因为中文两个字之间不仅没有空格，而是还可以随意组合。
- - - - -
# 13、关系
在数据库中，将实体与实体的关系反应到表的设计上来，可以分为三种，分别为：一对一（1：1），一对多（1：N）（或多对一（N：1））和多对多（N：N）。
在此，所有的关系都是指表与表之间的关系。

## 13.1 一对一
一对一，即一张表的一条记录只能与另一张表的一条记录想对应，反之亦然。

实例：
设计一张「个人信息表」，其字段包括：姓名、性别、年龄、身高、体重、籍贯、居住地等。

| ID | 姓名 | 性别 | 年龄 | 身高 | 体重 | 籍贯 | 居住地 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Juice | 男 | 18 | 182 | 72 | 中国 | 北京 | 
| 2 | Alice | 女 | 18 | 170 | 52 | 美国 | 西雅图 | 

如上表所示，但是有些数据是我们经常使用的，有些不经常使用，每次查询的时候都要查询所有数据，影响效率，可以将常用的数据和不常用的数据分为两个表，例如：

表1：常用数据

| ID | 姓名 | 性别 | 年龄 | 
| --- | --- | --- | --- | 
| 1 | Juice | 男 | 18 | 
| 2 | Alice | 女 | 18 | 

表2：不常用数据

| ID | 身高 | 体重 | 籍贯 | 居住地 |
| --- | --- | --- | --- | --- |
| 1 | 182 | 72 | 中国 | 北京 | 
| 2 | 170 | 52 | 美国 | 西雅图 | 

如上面表2和表1所示，通过字段ID，表1中的一条记录只能匹配表2种的一条记录，反之亦然，这就是一对一的关系。
- - - - -
## 13.2  一对多
一对多，即一张表中的记录可以对用另一张表中的多条记录，但是反过来，另一张表中的一条记录只能对应第一张表中的一条记录。

实例：
例如，设计「国家城市表」，其中包含两个实体，即国家和城市。

表3：国家表

| COUNTRY_ID | 国家 | 位置 |
| --- | --- | --- |
| 1 | 中国 | 亚洲 |
| 2 | 美国 | 北美洲 |

表4：城市表
| CITY_ID | 城市 | 国家 |
| --- | --- | --- |
| 1 | 北京 | 中国 |
| 2 | 上海 | 中国 |
| 3 | 纽约 | 美国 |
| 4 | 华盛顿 | 美国 |

如上面表3和表4的关系，是一种典型的一对多的关系。

- - - - -
## 13.3 多对多
多对多，即一张表中的记录可以对应另一张表中的多条记录，反过来，另外一张表中的记录也可以对应第一张表中的多条记录。

实例：
例如，设计「教师学生表」，其中包含两个实体，即学生和老师。

表5：教师表

| TEA_ID | 姓名 | 性别 |
| --- | --- | --- |
| 1 | 刘涛 | 女 |
| 2 | 胡歌 | 男 |
| 3 | 周杰伦 | 男 |

表6：学生表

| STU_ID | 姓名 | 性别 |
| --- | --- | --- |
| 1 | 张三 | 男 |
| 2 | 刘丽 | 女 |

表7：中间表

| ID | TEA_ID | STU_ID |
| --- | --- | --- |
| 1 | 1 | 1 |
| 2 | 1 | 2 |
| 3 | 2 | 1 |
| 3 | 3 | 2 |

表5和表6之间是多对多的关系，他们的关系是通过表7来维护的。
- - - - -
# 14、范式
范式：Normal Farmat，是为了解决数据存储和优化的问题。
在数据存储之后，凡是能通过关系寻找出来的数据，坚决不再重复存储，范式的终极目标是减少数据冗余。

范式是一种分层结果的规范，共6层，分别为1NF、2NF、3NF、4NF、5NF和6NF，每一层都比上一层严格，若要满足下一层范式，其前提条件是满足上一层范式。其中，1NF是最底层的范式，6NF为最高层的范式，也最为严格。

MySQL数据库属于关系型数据库，其存储数据的时候有些浪费空间，但也致力于节省空间，这就与范式想解决的问题不谋而合，因此在设计数据库的时候，大多利用范式来指导设计。但是数据库不单要解决存储空间的问题，还要保证效率的问题，而范式只为解决存储空间的问题，所以数据库的设计又不能完全按照范式的要求来实现，因此在一般情况下，只需要满足前三种范式即可。

此外，咱们需要知道：范式在数据库的设计中是有指导意义的，但不是强制规范。

## 14.1 1NF
第一范式：在设计数据库的时候，如果表中设计的字段存储的数据，在取出来使用之前还需要额外的处理（拆分），那么表的设计就不满足第一范式，第一范式要求字段的数据具有原子性，不可再分。

实例：
例如，设计一个「学校假期表」，如下所示：
表1：学校假期时间表

| ID | 学校 | 起始时间，结束时间 |
| --- | --- | --- |
| 1 | 清华大学 | 20180711, 20180901 |
| 2 | 华中科技大学 | 20180804, 20180820 |

观察上表，咱们会发现表1的设计并没有什么问题，但是如果需求是查询各学校开始放假的日期呢？那显然上表的设计并不满足1NF，数据不具有原子性。对于此类问题，解决的方案就是将表1进行拆分：

表2：拆分后的表1

| ID | 学校 | 起始时间 | 结束时间 |
| --- | --- | --- | --- |
| 1 | 清华大学 | 20180711 | 20180901 |
| 2 | 华中科技大学 | 20180804 | 20180820 |

- - - - -
## 14.2 2NF
第二范式：在数据表的设计过程中，如果有复合主键（多字段主键），且表中字段并不是由主键来确定，而是依赖复合主键的某个字段（主键的部分），也就是说存在依赖主键的部分依赖，第二范式就是解决表设计中不存在出现部分依赖。

实例：
例如，设计一个「教室授课表」，如下所示：

表3：教室授课表

| 教师| 性别 | 课程 | 地点 |
| --- | --- | --- | --- |
| 王伟 | 男 | 《如何追到心爱的女孩》 | 西12 |
| 李白 | 男 | 《唐诗三百首》 | 东9 |

观察上表，我们发现：教室不能作为主键，需要与授课地点相结合才能成作为主键，其中性别依赖于具体的老师，而课程依赖于授课地点，这就出现了表的字段依赖于部分主键的问题，从而导致不能满足第二范式。
- 解决方案1：将教师和性别，课程和授课地点，分成两张单独的表
- 解决方案2：取消复合主键，使用逻辑主键

这里我们选择方案2。

| ID | 教师| 性别 | 课程 | 地点 |
| --- | --- | --- | --- | --- |
| 1 | 王伟 | 男 | 《如何追到心爱的女孩》 | 西12 |
| 2 | 李白 | 男 | 《唐诗三百首》 | 东9 |

- - - - -
## 14.3 3NF
第三范式：需要满足第一范式和第二范式，理论上讲，每张表中的所有字段都应该直接依赖主键（逻辑主键，代表是业务主键），如果表设计中存在一个字段，并不直接依赖主键，而是通过某个非主键字段依赖，最终实现主键依赖（把这种不是直接依赖主键，而是依赖非主键字段的依赖关系，称之为传递依赖），第三范式就是要解决表设计中出现传递依赖的问题。

以上的添加逻辑主键的表3为例：

| ID | 教师| 性别 | 课程 | 地点 |
| --- | --- | --- | --- | --- |
| 1 | 王伟 | 男 | 《如何追到心爱的女孩》 | 西12 |
| 2 | 李白 | 男 | 《唐诗三百首》 | 东9 |

在以上表的设计中，性别依赖教师，教师依赖主键；课程依赖授课地点，授课地点依赖主键，因此性别和课程都存在传递依赖的问题。
- 解决方案：将存在传递依赖的字段，以及依赖的字段本身单独取出来，形成一个单独的表，然后在需要使用对应的信息的时候，把对应的实体表的主键添加进来。

表4：教室表

| TEA_ID | 教师| 性别 |
| --- | --- | --- |
| 1 | 王伟 | 男 | 
| 2 | 李白 | 男 | 

表5：授课地点表

| ADDRESS_ID | 课程 | 地点 |
| --- | --- | --- |
| 1 | 《如何追到心爱的女孩》 | 西12 |
| 2 | 《唐诗三百首》 | 东9 |

表6：进行处理后的表

| ID | TEA_ID | ADDRESS_ID |
| --- | --- | --- |
| 1 | 1 | 1 |
| 2 | 2 | 2 |
| 3 | 3 | 3 |

- - - - -
## 14.4 逆规范化
在某些特定的环境中（例如淘宝数据库），在设计表的时候，如果一张表中有几个字段是需要从另外的表中去获取数据，理论上讲，的确可以获得想要的数据，但是相对来说，其效率低会一点。此时为了提高查询效率，咱们会刻意的在某些表中，不去保存另外一张表的主键（逻辑主键），而是直接保存想要存储的数据信息，这样的话，在查询数据的时候，这张表就可以直接提供咱们想要的数据，而不需要多表查询，但是这样做会导致数据冗余。

实际上，逆规范化是磁盘利用率和效率之间的对抗。
- - - - -
# 15、主键冲突
数据的操作，无外乎就是增删改查。
在插入数据的时候，假设主键对用的值已经存在，则插入失败，这就是主键冲突。

第一种情况：主键冲突，进行更新操作
基本语法：
``` SQL
insert into 表名[(字段列表：包含主键)]  values(值列表) on duplicate  key update 字段 = 新值;
```

实例：
``` SQL
insert into my_class values('PM', 'B313') on duplicate key update room = 'B313'
```

第二种情况：主键冲突，选择替换操作
基本语法：
``` SQL
replace insert into 表名[(字段列表：包含主键)]  values(值列表);
```
实例：
``` SQL
insert into my_class values('PM', 'B313');
replace into my_class values('PM', 'B313');
```
- - - - -
# 16、蠕虫复制
蠕虫复制：从已有的表中获取数据，然后将数据进行新增操作，数据成倍（以指数形式）的增加。
根据已有表创建表，即复制表结构，其基本语法为：
``` SQL
create table 表名 like [数据库名.]表名;
```

实例：
``` SQL

mysql> create table my_copy like my_auto;
Query OK, 0 rows affected (0.01 sec)

mysql> desc my_copy;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |     | NULL    |                |
+-------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```

蠕虫复制的步骤为：先查出数据，然后将查出的数据新增一遍。
基本语法：
``` SQL
insert into 表名 select 字段列表/* from 表名;
```

实例：
``` SQL
mysql> insert into my_copy select * from my_auto;
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> select * from my_copy;
+----+--------+
| id | name   |
+----+--------+
|  1 | Harlon |
|  2 | Juice  |
|  3 | Ellie  |
+----+--------+
3 rows in set (0.00 sec)
```

蠕虫复制的效果：
``` SQL
mysql> insert into my_copy(name) select name from my_copy;
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> insert into my_copy(name) select name from my_copy;
Query OK, 6 rows affected (0.00 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> insert into my_copy(name) select name from my_copy;
Query OK, 12 rows affected (0.00 sec)
Records: 12  Duplicates: 0  Warnings: 0

mysql> insert into my_copy(name) select name from my_copy;
Query OK, 24 rows affected (0.01 sec)
Records: 24  Duplicates: 0  Warnings: 0
```

蠕虫复制的意义：
- 从已有的数据表中拷贝数据到新的数据表
- 可以迅速的让表中的数据膨胀到一定的数量级，多用于测试表的压力及效率

- - - - -
# 17、数据库高级操作
## 17.1 更新数据
基本语法：
``` SQL
update 表名 set 字段 = 值 [where 条件]
```
高级语法：
``` SQL
update 表名 set 字段 = 值 [where 条件] [limit 更新数量]
```

实例：
``` SQL
mysql> update my_copy set name = 'Harlon2' where name = 'harlon' limit 3;
Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0

mysql> select * from my_copy;
+----+---------+
| id | name    |
+----+---------+
|  1 | Harlon2 |
|  2 | Juice   |
|  3 | Ellie   |
|  4 | Harlon2 |
|  5 | Juice   |
|  6 | Ellie   |
|  7 | Harlon2 |
...
```
- - - - -
## 17.2 删除数据
``` SQL
delete from 表名 [where 条件]
```
高级语法：
``` SQL
delete from 表名 [where 条件] [limit 更新数量]
```

实例：
``` SQL
mysql> delete from my_copy where name = 'Harlon2' limit 2;
Query OK, 2 rows affected (0.00 sec)

mysql> select * from my_copy;
+----+---------+
| id | name    |
+----+---------+
|  2 | Juice   |
|  3 | Ellie   |
|  5 | Juice   |
|  6 | Ellie   |
|  7 | Harlon2 |
|  8 | Juice   |
...
```

删除表的数据，自增长不会还原，如果想要还原自增长属性，思路是：先删除表，然后重新建表。
基本语法：
``` SQL
truncate 表名
```

