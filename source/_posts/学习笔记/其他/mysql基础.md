# 1. mysql命令

```mysql
net start mysql --开启sql服务
net stop mysql--关闭sql服务
mysql -uroot -p + password;//命令行mysql
show databases/(tables);//展示所有数据库或者表
use dataname;打开指定数据库
create database/(table) name;//创建数据库或者表
drop database(table) name;//删除数据库或者表
describe/(desc) tablename ;//显示表这一列的东西
create table tablename(参数 类型(长度),参数 类型(长度);//创建表
alter database dbname character set utf8;//修改数据库的字符集
flush privileges;//刷新数据库
show create database/(table) name;//展示创建数据库或者表的结构
                       
alter table old_name rename new_name;//重命名表(数据库不重命名)
alter table tablename modify id int(10);//修改字段类型
alter table tablename change gener sex verchar[1];//修改字段
alter table tablename add name int()[first\last,after age];//添加属性        
alter table tablename drop score;//删除字段
alter table tablename modify old after ;
//多字段主键要用表级约束而不是列级约束
alter table tablename add primary key (age id)//已有的表添加主键约束
-- 添加外键约束                       
create table 表名(
    id interesting(10),
    foreign key() references 外表(字段名);
);       
```

#### 数据库数据的增删查改

```mysql
insert tablename value (1,2,3),();
select *from tablename;
select DISTINCT <字段名称>as '新名称' from 表名 where 条件 order by <字段>[顺序],<字段2>[顺序];
--查询语句 排序的时候第二个字段在第一个字段排序后字段一相同的字段按字段2的顺序排序,其中 DISTINCT关键字可以删除重复元素
SELECT product_name, product_type, regist_date
FROM Product
WHERE regist_date < '2009-09-27';
-- 日期的比较
-- 字段为字段内容里所以的数据
select * from 表名 where 字段 in(字段内容);                       
-- 字段为空的                       
select * from 表名 where 字段 is null;   
-- 模糊查询 通配符 % 与 _
select * from 表名 where 字段 [no]like '%s%';-- 字段中含有s的               
select * from 表名 where 字段 [no]like '20130930%'--   查俺们班的人
-- 一个字符为_ 多个为%
 -- 测试整型的模糊查询
-- 插入
insert into 表名(字段) values();                       
-- 删除
delete from 表名 where 表达式;                       
truncate table 表名;                       
-- 更新             
update 表名 set 字段=内容,字段2=内容 where 表达式;                       
-- 限制查询 从下标为三开始的5行内容
select * from 表名 where 条件 limit [3,5]   
```



mysql中一些常用的函数

```mysql
date(表中date字段)提取日期
year(表中date字段)提取年
current_time--当前时间
COUNT： 计算表中的记录数（行数）
SUM： 计算表中数值列中数据的合计值
AVG： 计算表中数值列中数据的平均值
MAX： 求出表中任意列中数据的最大值
MIN： 求出表中任意列中数据的最小值
SELECT COUNT(*)
FROM Product;
count()
group_concat()
```

```
limit 开始位置,j
```

### mysql事务

acid原则

a原子性:事务中的操作要么都发生，要么都不发生。

c 一致性:事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。

i 隔离性:事务之间是隔离的，一个事务不应该影响其它事务运行效果。

1. 脏读
2. 不可重复读
3. 幻读

d 持久性:	事务一旦提交，则持久化保存在数据库中。

事务自动提交 set autocommit=0/1

开启事务

start transaction 

insert 数据

提交 commit 

回滚 rollback

 savepoint 点名 事务保存点

rollback to savepoint 点名

删除保存点 release savepoint 点名

create database  shop  character set utf8 collate utf8_general_ci

use shop

create table `account`(

```
开启一组事务
set autocommit =0 
start transaction 
update account set money=money+100 where name =`a
commit
rollback
set autocommit =1;

```

 

)engin = innodb default charset =utf8

##### 索引

帮助高效获取数据的数据结构

分类

1. 主键索引

    主键不可以重复

2. 唯一索引

3. 常规索引

4. 全文索引

   显示所有索引

   show INDEX FROM STUDENT

   alter table student add fulltext index student; 

   explain 分析sql

   索引原则

   不对经常修改的字段添加索引

   小数据量不需要

   一般加在用来查询的字段上

   





```mysql
create database study;
use study;
-- 学生表Fd
create table student(
std_id int(10)  auto_increment,
std_name VARCHAR(5) NOT NULL,
email VARCHAR(20) NOT NULL UNIQUE,
sex VARCHAR(1) DEFAULT '男',
age int(3) NOT NULL,
primary key(std_id));
-- 查看表的结构
DESC student;
-- 插入数据
INSERT INTO student(std_id,std_name,email,age)VALUES(1,'小王','123456@qq.com',21);
-- 查看数据
SELECT *FROM student;
-- 追加数据
INSERT into student(std_name,email,age)VALUES('小笼包','1232123@qq.com',22);
-- 查看数据
SELECT *FROM student;
-- 增加时间戳

-- 更新表

-- 查看表内容

-- 作业二

-- 表一 学生表
CREATE TABLE student2(
std_id int(10) auto_increment,
std_name VARCHAR(5)NOT NULL,
major_id int(10)NOT NULL,
PRIMARY KEY(std_id));
-- 查看表的结构
DESC student2;
-- 添加数据
INSERT INTO student2(std_id,std_name,major_id)VALUES(1,'小王','2013');
-- 查看数据
SELECT * FROM student2;
-- 添加性别字段

-- 表二 课程表
CREATE TABLE curriculum(
cur_id int(10),
cur_name VARCHAR(10) NOT NULL,
score int(1) NOT NULL,
PRIMARY KEY(cur_id));
-- 查看表的结构
DESC curriculum;
-- 添加数据
INSERT INTO curriculum(cur_id,cur_name,score)VALUES(1,'大学美育',1);
-- 添加字段自增
ALTER TABLE curriculum MODIFY cur_id INT ( 10 ) auto_increment;
INSERT INTO curriculum(cur_name,score)VALUES('大学体育',1);
INSERT INTO curriculum(cur_name,score)VALUES('javaweb开发',1);
-- 查看数据
SELECT * FROM curriculum;
```

```mysql
select  course.cno,cname,score.sno from course,score where score.cno=course.cno and cname="数据结构";
select sno from score where score.cno=( select cno from course where cname="数据结构");
# 求学生人数不足3人的系及其相应的学生数求学生人数不足3人的系及其相应的学生数
select sdept,count(*) from student group by sdept having count(*)<3;
-- sname 不能详细显示出来
SELECT SNAME, SDEPT,COUNT(*)  FROM STUDENT  GROUP BY SDEPT having SDEPT="CS";
# 学生的编号是唯一的所以没有出错
SELECT SAGE FROM STUDENT GROUP BY SAGE;
# (建立视图)  建立计算机系的学生的视图STUDENT_CS。
create view STUDENT_GB  as SELECT * FROM STUDENT WHERE SDEPT="CS";
select * from STUDENT_GB;
# (建立视图)  建立由学号和平均成绩两个字段的视图STUDENT_GR\
select  score from score;
create view STUDENT_GR as select s.sno,avg(s.score) from (select student.sno as sno,ifnull(score,0) score  from student left join score on score.sno=student.sno) as s group by s.sno;
# (视图查询)利用视图STUDENT_GR求平均成绩为88分以上的学生学号和平均成绩。
select * from STUDENT_GR where `avg(s.score)`>88;
# (视图更新)利视图STUDENT_CS，增加学生( ‘96006’, ‘张然’, ‘CS’, ‘02’, ‘男’, 19 )
insert into STUDENT_GB(sno, sname, sdept, sclass, sage, ssex) values ("96006","张然", "CS", "02", 19, "男");
# （视图更新)利用视图STUDENT_CS，将学生年龄增加1岁。观察其结果
select * from student_gb;
update student_gb set sage=sage+1;
# 求年龄在19岁与22岁(含20岁和22岁)之间的学生的学号和年龄。
select sno ,sage from student where sage>=19 and sage<=22;
# 求选修了课程的学生学号。
select distinct sno from score;
# 求各门课程的平均成绩与总成绩
select course.cname,ifnull(avg(score),0),ifnull(sum(score),0) sum from course left join score on course.cno=score.cno group by course.cno;

```

if()函数,case when then end 表达式

```mysql
select employee_id, case when employee_id % 2 = 1 and name not regexp('^M') then salary else 0 end as bonus
from Employees
;
select employee_id ,(if(employee_id%2=1 and name not like "M%"),salary,0) as  bonus  from employees order by employee_id;
-- 正则限定开头符号 ^
-- 不以M开头的名字
name not regexp/rlike('^M')
-- 模糊查询
 name not like "M%"

 sex = 如果case when sex='m' 符合then 'f' 否则else 'm' end
 if(条件,返回,否则)
```

count(),group_concat() 的使用 distinct

```mysql
select distinct sell_date,count(distinct product) num_sold,GROUP_CONCAT( distinct product) products  from activities group by sell_date order by sell_date;
```

两个正则

```
CONDITIONS REGEXP '^DIAB1' 以DIAB1开头的字符
CONDITIONS REGEXP '\\sDIAB1' 以空格加DIAB1的字符
```

db表用户对数据库的权限

dos命令

```shell
mysql -h 127.0.0.1 -p8080 -uroot -p123 homework
```

```mysql
 CREATE DATABASE `homework` /*!40100 DEFAULT CHARACTER SET utf8 */ |
```

mysql在命令行

```shell
mysql -uroot -p [要使用的数据库];
```

```mysql
show databases;
show tables;
show create database/table [数据库名/表名];
use mysql ;
-- 查看存在的用户
select user from user;
-- 添加用户注意符号
create user
 text @"127.0.0.1"
 IDENTIFIED by "123";
 -- 改密码
 SET PASSWORD FOR 'usernamexxx'@'hostxxx' = PASSWORD('newpasswordxxx');
 -- 当前用户
 SET PASSWORD = PASSWORD("newpasswordxxx");
 -- 删除用户
 DROP USER 'usernamexxx'@'hostxxx';
-- 或者
delete from user where user.user="用户名";
```

union all 关键字和 union 

```mysql
select employee_id from (
    -- 将两个表的字段合成一个
    select employee_id
    from employees
    union all
    select employee_id
    from salaries
    
) as ans
group by employee_id
having count(employee_id) = 1
order by employee_id
;
union all 关键字和 union 
-- 加了all会显示重复的,不加就会过滤掉
```

limit取第二高薪资

```mysql
select (select DISTINCT salary end  from employee order by  salary desc limit 1,1) as SecondHighestSalary ;
```

```mysql
mysql中isnull,ifnull,nullif的用法如下：

isnull(expr) 的用法： 如expr 为null，那么isnull() 的返回值为 1，否则返回值为 0。 mysql> select isnull(1+1); -> 0 mysql> select isnull(1/0); -> 1 使用= 的null 值对比通常是错误的。
a
isnull() 函数同 is null比较操作符具有一些相同的特性。请参见有关is null 的说明。

IFNULL(expr1,expr2)的用法：

假如expr1 不为 NULL，则 IFNULL() 的返回值为 expr1; 否则其返回值为 expr2。IFNULL()的返回值是数字或是字符串，具体情况取决于其所使用的语境。

mysql> SELECT IFNULL(1,0);
-> 1
mysql> SELECT IFNULL(NULL,10);
-> 10
mysql> SELECT IFNULL(1/0,10);
-> 10
mysql> SELECT
IFNULL(1/0,'yes');

        ->   'yes
```

```mysql
limit n子句表示查询结果返回前n条数据

offset n表示跳过x条语句

limit y offset x 分句表示查询结果跳过 x 条数据，读取前 y 条数据

使用limit和offset，降序排列再返回第二条记录可以得到第二大的值。
```

### 各种连接

```
inner join：2表值都存在

outer join：附表中值可能存在null的情况。

总结：

①A inner join B：取交集

②A left join B：取A全部，B没有对应的值，则为null

③A right join B：取B全部，A没有对应的值，则为null

④A full outer join B：取并集，彼此没有对应的值为null

上述4种的对应条件，在on后填写。
```

datediff函数

```mysql
MySQL 使用 DATEDIFF 
两个日期间的差值
```

