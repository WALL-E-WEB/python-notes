```
tsSzgGh_G7e0
WangYANG520
```

查看时区

```
查看时区:
show variables like'%time_zone';
修改时区:
set global time_zone = '+8:00'; 
```



# 数据类型和约束

## 1. 数据类型

数据类型是指在创建表的时候为表中字段指定数据类型，只有数据符合类型要求才能存储起来，使用数据类型的原则是:够用就行，尽量使用取值范围小的，而不用大的，这样可以更多的节省存储空间。

**常用数据类型如下:**

- 整数：int，bit
- 小数：decimal
- 字符串：varchar,char
- 日期时间: date, time, datetime
- 枚举类型(enum)

**数据类型说明:**

- decimal表示浮点数，如 decimal(5, 2) 表示共存5位数，小数占 2 位.
- char表示固定长度的字符串，如char(3)，如果填充'ab'时会补一个空格为'ab '，3表示字符数
- varchar表示可变长度的字符串，如varchar(3)，填充'ab'时就会存储'ab'，3表示字符数
- 对于图片、音频、视频等文件，不存储在数据库中，而是上传到某个服务器上，然后在表中存储这个文件的保存路径.
- 字符串 text 表示存储大文本，当字符大于 4000 时推荐使用, 比如技术博客.

## 2. 数据约束

约束是指数据在数据类型限定的基础上额外增加的要求.

**常见的约束如下:**

- 主键 primary key: 物理上存储的顺序. MySQL 建议所有表的主键字段都叫 id, 类型为 int unsigned.
- 非空 not null: 此字段不允许填写空值.
- 惟一 unique: 此字段的值不允许重复.
- 默认 default: 当不填写字段对应的值会使用默认值，如果填写时以填写为准.
- 外键 foreign key: 对关系字段进行约束, 当为关系字段填写值时, 会到关联的表中查询此值是否存在, 如果存在则填写成功, 如果不存在则填写失败并抛出异常.

## 3. 数据类型附录表

##### 1. 整数类型

| 类型        | 字节大小 | 有符号范围(Signed)                         | 无符号范围(Unsigned)     |
| :---------- | :------- | :----------------------------------------- | :----------------------- |
| TINYINT     | 1        | -128 ~ 127                                 | 0 ~ 255                  |
| SMALLINT    | 2        | -32768 ~ 32767                             | 0 ~ 65535                |
| MEDIUMINT   | 3        | -8388608 ~ 8388607                         | 0 ~ 16777215             |
| INT/INTEGER | 4        | -2147483648 ~2147483647                    | 0 ~ 4294967295           |
| BIGINT      | 8        | -9223372036854775808 ~ 9223372036854775807 | 0 ~ 18446744073709551615 |

##### 2. 字符串

| 类型     | 说明                        | 使用场景                     |
| :------- | :-------------------------- | :--------------------------- |
| CHAR     | 固定长度，小型数据          | 身份证号、手机号、电话、密码 |
| VARCHAR  | 可变长度，小型数据          | 姓名、地址、品牌、型号       |
| TEXT     | 可变长度，字符个数大于 4000 | 存储小型文章或者新闻         |
| LONGTEXT | 可变长度， 极大型文本数据   | 存储极大型文本数据           |

##### 3. 时间类型

| 类型      | 字节大小 | 示例                                                  |
| :-------- | :------- | :---------------------------------------------------- |
| DATE      | 4        | '2020-01-01'                                          |
| TIME      | 3        | '12:29:59'                                            |
| DATETIME  | 8        | '2020-01-01 12:29:59'                                 |
| YEAR      | 1        | '2017'                                                |
| TIMESTAMP | 4        | '1970-01-01 00:00:01' UTC ~ '2038-01-01 00:00:01' UTC |



 

# mysql安装登陆

```mysql
安装后:
进入 bin  管理员cmd

//保留密码
mysqld --initialize --console

mysqld -install

启动:  net start mysql　

mysql -u root -p 初始密码

设置密码: ALTER USER 'root'@'localhost' IDENTIFIED BY  '新密码';
//注意带";"
```



```
.window
启动:  net start mysql　　
关闭:	 net stop mysql　

.Linux
启动服务:service mysql start　　　

关闭服务:service mysql stop　　

重启服务:service restart stop
```

pymysql 安装

 运行pymysql报错解决

```python
RuntimeError: cryptography is required for sha256_password or caching_sha2_password

解决:pip install cryptography  
如安装失败,多次安装即可
```

# SQL语句

1.登陆

```sql
mysql -u root -p
后输入密码
```

退出：

```
quit 或 exit 或 ctrl + d
```

## 2.数据库操作的SQL语句

```mysql
1、查看所有数据库：show databases;

2、创建数据库：create database 数据库名 charset=utf8;

3、使用数据库：use 数据库名;

4、查看当前使用的数据库：select database();

5、删除数据库-慎重：drop database 数据库名;
```

##  3.表结构操作的SQL语句

```mysql
1、查看当前数据库中所有表：show tables;

2、创建表：
	create table students(
     id int unsigned primary key auto_increment not null,
     name varchar(20) not null,
     age tinyint unsigned default 0,
     height decimal(5,2),
     gender enum('男','女','人妖','保密')
    );
    
    说明:
        create table 表名(
        字段名称 数据类型  可选的约束条件,
        column1 datatype contrai,
        ...
        );
        
3、修改表-添加字段：
	alter table 表名 add 列名 类型 约束;
	例：
	alter table students add birthday datetime;
	
3、修改表-修改字段名和字段类型：
	alter table 表名 modify 列名 类型 约束;
	例：
	alter table students modify birthday date not null;
	

4、修改表-修改字段名和字段类型：
	alter table 表名 change 原名 新名 类型及约束;
	例：
	alter table students change birthday birth datetime not null;
	
5、修改表-删除字段：
	alter table 表名 drop 列名;
	例：
	alter table students drop birthday;

6、查看创表SQL语句：
	show create database 数据库名;
	例：
	show create database mytest;
	
7、查看表结构：desc 表名;

8、删除表：
    drop table 表名;
    例：
    drop table students;


```

## 4.表数据操作

查询数据

```mysql
-- 1. 查询所有列
    select * from 表名;
    例：
    select * from students;

-- 2. 查询指定列
    select 列1,列2,... from 表名;
    例：
    select id,name from students;
```

添加数据

```mysql
-- 1. 全列插入：值的顺序与表结构字段的顺序完全一一对应
    insert into 表名 values (...)
    例:
    insert into students values(0, 'xx', default, default, '男');
    
-- 2. 部分列插入：值的顺序与给出的列顺序对应
    insert into 表名 (列1,...) values(值1,...)
    例:
    insert into students(name, age) values('王二小', 15);
    
-- 3. 全列多行插入
    insert into 表名 values(...),(...)...;
    例:
    insert into students values(0, '张飞', 55, 1.75, '男'),(0, '关羽', 58, 1.85, '男');
    
-- 4. 部分列多行插入
    insert into 表名(列1,...) values(值1,...),(值1,...)...;
    例：
    insert into students(name, height) values('刘备', 1.75),('曹操', 1.6);
```

修改数据

```mysql
update 表名 set 列1=值1,列2=值2... where 条件
例：
update students set age = 18, gender = '女' where id = 6;
```

删除数据

```mysql
delete from 表名 where 条件
例：
delete from students where id=5;
```

> 问题:

> 上面的操作称之为物理删除，一旦删除就不容易恢复，我们可以使用逻辑删除的方式来解决这个问题。

```sql
-- 添加删除表示字段，0表示未删除 1表示删除
alter table students add isdelete bit default 0;
-- 逻辑删除数据
update students set isdelete = 1 where id = 8;
```

> **说明:**

- 逻辑删除，本质就是修改操作





### as和distinct关键字

#### 	as取别名

1. 使用 as 给字段起别名

   ```sql
   select id as 序号, name as 名字, gender as 性别 from students;
   ```

2. 可以通过 as 给表起别名

   ```sql
   -- 如果是单表查询 可以省略表名
   select id, name, gender from students;
   
   -- 表名.字段名
   select students.id,students.name,students.gender from students;
   
   -- 可以通过 as 给表起别名 
   select s.id,s.name,s.gender from students as s;
   ```

   **说明:**

   - 在这里给表起别名看起来并没有什么意义,然而并不是这样的，我们在后期学习 自连接 的时候，必须要对表起别名。

### distinct去重

distinct可以去除重复数据行。

```sql
select distinct 列1,... from 表名;

例： 查询班级中学生的性别
select name, gender from students;

-- 看到了很多重复数据 想要对其中重复数据行进行去重操作可以使用 distinct
select distinct name, gender from students;
```

## 5.where条件查询

**where语句支持的运算符:**

1. 比较运算符
2. 逻辑运算符
3. 模糊查询
4. 范围查询
5. 空判断

**where条件查询语法格式如下:**

```sql
select * from 表名 where 条件;
例：
select * from students where id = 1;
```

### 2. 比较运算符查询

1. 等于: =
2. 大于: >
3. 大于等于: >=
4. 小于: <
5. 小于等于: <=
6. 不等于: != 或 <>

**例1：查询编号大于3的学生:**

```sql
select * from students where id > 3;
```

**例2：查询编号不大于4的学生:**

```sql
select * from students where id <= 4;
```

**例3：查询姓名不是“黄蓉”的学生:**

```sql
select * from students where name != '黄蓉';
```

**例4：查询没被删除的学生:**

```sql
select * from students where is_delete=0;
```

### 3. 逻辑运算符查询

1. and
2. or
3. not

**例1：查询编号大于3的女同学:**

```sql
select * from students where id > 3 and gender=0;
```

**例2：查询编号小于4或没被删除的学生:**

```sql
select * from students where id < 4 or is_delete=0;
```

**例3：查询年龄不在10岁到15岁之间的学生:**

```sql
select * from students where not (age >= 10 and age <= 15);
```

**说明:**

- 多个条件判断想要作为一个整体，可以结合‘()’。

### 4. 模糊查询

1. like是模糊查询关键字
2. %表示任意多个任意字符
3. _表示一个任意字符

**例1：查询姓黄的学生:**

```sql
select * from students where name like '黄%';
```

**例2：查询姓黄并且“名”是一个字的学生:**

```sql
select * from students where name like '黄_';
```

**例3：查询姓黄或叫靖的学生:**

```sql
select * from students where name like '黄%' or name like '%靖';
```

### 5. 范围查询

1. between .. and .. 表示在一个连续的范围内查询
2. in 表示在一个非连续的范围内查询

**例1：查询编号为3至8的学生:**

```sql
select * from students where id between 3 and 8;
```

**例2：查询编号不是3至8的男生:**

```
select * from students where (not id between 3 and 8) and gender='男';
```

### 6. 空判断查询

1. 判断为空使用: is null
2. 判断非空使用: is not null

**例1：查询没有填写身高的学生:**

```sql
select * from students where height is null;
```

**注意:**

1. 不能使用 where height = null 判断为空
2. 不能使用 where height != null 判断非空
3. null 不等于 '' 空字符串