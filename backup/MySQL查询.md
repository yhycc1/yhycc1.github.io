## 基本的数据类型
char(10)          'ABC          '    定长,后面会补上空格，效率高，一般用于**身份证**，**电话号码**等
vacchar(10)      'ABC'             不定长，**效率偏低**

> 在SQL中，我们有如下约束：
 *NOTNULL*-指示某列不能存储NULL值。
 *UNIQUE*-保证某列的每行必须有唯一的值。
 *PRIMARYKEY-NOTNULL*和*UNIQUE*的结合。确保某列（或两个列多个列的结合）有唯一标识，有助 
于更容易更快速地找到表中的一个特定的记录。
 *'FOREIGNKEY**'*-保证一个表中的数据匹配另一个表中的值的参照完整性。
 *CHECK*-保证列中的值符合指定的条件。
 *DEFAULT*-规定没有给列赋值时的默认值。

## 创建数据表
删除表
DROP TABLE IF EXISTS 'websites';

`create table 表名(
	字段名 类型 约束,
	字段名 类型 约束,
)编码,存储引擎;`

例如创建一个学生表：
create table students(
	name vacchar 约束;
	age int 约束;
);
## 1.查询
--查询所有字段
--select * from 表名；
`select * from students;`
`select * from classes;`
`select id,name from classes;`
--查询指定字段
--select 列1，列2，from 表名;
`select name,age from students;`
--使用 as 给字段起别名
--select 字段as 名字.：from 表名；
`select name as 姓名，age as 年龄 fromn students;`
--select 表名字段....from 表名；
`select students.name, students.age from students;`
--可以通过 as 给表起别名
--select 别名.字段....from 表名 as 别名；
`select students.name, students.age from students;`
`select s.name,s.age from students as s;`
--失败的select students.name,students.age from students as s;

--消除重复行
--distinct 字段
`select distinct gender from students;`

## 2.条件
### 比较运算符
==**select ...from 表名 where**==
--查询大于18岁的信息
`select * from students where age>18;`
`select id,name,gender from students where age>18;`
--查询小于18岁的信息
`select * from students where age<18;`
--查询小于或者等于18岁的信息
--查询年龄为18岁的所有学生的名字
`select * from students where age=18;`
-!=或者<>

### 逻辑运算符
--==**and**==
--18到28之间的所有学生信息
`select * fromn students where age>18 and age<28;`
--失败select * from students where age>18 and <28;
--18岁以上的女性
`select * from students where age>18 and gender="女";`
`select * from students where age>18 and gender=2;`
--==**or**==
--18以上或者身高查过180（包含）以上
`select * from studentss where age>188 or height>=180;`
--==**not**==
--不在 18岁以上的女性 这个范围内的信息
`select * from students where not age>18 and gender=2;`
`select * from students where not (age>18 and gender=2);`
--年龄不是小于或者等于18 并且是女性
`select * from students wheree（not age<=18)and gender=2;`
### 模糊查询
--==**like**==
--%替换1个或者多个
--替换1个
--查询姓名中以“小”开始的名字
`select name from students where name="小";`
`select name from students where name like“小%”；`
--查询姓名中有“小”所有的名字
`select name from students  where name like“%小%”；`
--查询有2个字的名字
`select name from students where name like "_";`
--查询有3个字的名字
`select name from students where name like"_";`
--==查询至少有2个字的名字==
`select name from students where name like"_%";`
``
--**rlike 正则**
--查询以 周开始的姓名
`select name from students where name like"^周.*";`
--查询以 周开始、伦结尾的姓名
`select name from students where name like"^周.*伦$";`
### 范围查询
-- ==**in(1,3,8)**== 表示在一个非连续的范围内
--查询年龄在18、34的姓名
`select name,age from students where age in(18,34);`
--==**between**...**and**==...表示在一个连续的范围内
--查询 年龄在18到34之间的的信息
`select name,age from students where age between 18 and 34;`
--not between...and...表示不在一个连续的范围内
查询 年龄不在在18到34之间的的信息
select from students where age ==not between== 18 and 34;
--==**空判断**== `is null`
--查询身高为空的信息
`select * from students where height is null;`
`select * from students where height is not null;`

## 3.排序

## 4.聚合函数
### ==count==(总数)
--查询男性有多少人，女性有多少人
`select count(*) as 男性人数 from students where gender = 1;`
`select count(*) as 女性人数 from students where gender = 2;`
### max
--查询最大年龄
`select max(age) from students;`
### min
--查询女性的最小身高
`select min(hegiht) from students where gender = 2`
### sum
--计算所有人的年龄之和
`select sum(age) from students;`
### ==avg==
--计算平均年龄
`select avg(age) from students;`
--计算平均年龄
`select sum(age)/count(*) from students;`
### ==**round**==
--四舍五入 round（123.23，1）保留1位小数 123.2
--计算所有人的平均年龄，保留2位小数
`select round(avg(age),2) from students;`
--计算男性的平均身高,保留2位小数
`select round(avg(height),2) from students where gender = 2;`
## 5.分组
### **group by**
--按照性别分组，查询所有的性别
~~`select name from students group by gender;`
`select * from students group by gender;`~~
`select gender from students group by gender;`
--失败select * from students group by gender;
--计算每种性别中的人数
`select gender,count( * ) from students group by gender;`
--计算男性的人数
`select gender,count( * ) from students where gender=1 group by gender;`
### **group_concat**(...)
--查询同种性别中的姓名(男性)
`select gender,group_concat(name) from students where gender=1 group by gender;`
`select gender,group_concat(name, age, id) from students where gender=1 group by gender;`
`select gender,group_concat(name,", age, "", id） from students where gender=1 group bygender;`
### having
--查询平均年龄超过30岁的性别，以及姓名 having avg（age）>30
`select gender, group_concat(name),avg(age) from students group by gender having avg(age)>30;`
--查询每种性别中的人数多于2个的信息
`select gender,group_concat(name) from students group by gender having count(*)>2;`
