## 基本的数据类型
char(10)          'ABC          '    定长,后面会补上空格，效率高，一般用于**身份证**，**电话号码**等
vacchar(10)      'ABC'             不定长，**效率偏低**

> 在SQL中，我们有如下约束：
 *notnull*-指示某列不能存储NULL值。
 *unique*-保证某列的每行必须有唯一的值。
 *primary key--notnull*和*unique*的结合。确保某列（或两个列多个列的结合）有唯一标识，有助 
于更容易更快速地找到表中的一个特定的记录。
 *'foreignkey**'*-保证一个表中的数据匹配另一个表中的值的参照完整性。
 *check*-保证列中的值符合指定的条件。
 *default*-规定没有给列赋值时的默认值。

## 创建数据表
```mysql
删除表
DROP TABLE IF EXISTS 'websites';

create table 表名(
	字段名 类型 约束,
	字段名 类型 约束,
)编码,存储引擎;`

例如创建一个学生表：
create table students(
	name vacchar 约束;
	age int 约束;
);
```
## 1.查询
```mysql
--查询所有字段
-- select * from 表名；
select * from students;
select * from classes;
select id,name from classes;
--查询指定字段
--select 列1，列2，from 表名;
select name,age from students;
--使用 as 给字段起别名
--select 字段as 名字.：from 表名；
select name as 姓名，age as 年龄 fromn students;
--select 表名字段....from 表名；
select students.name, students.age from students;
--可以通过 as 给表起别名
--select 别名.字段....from 表名 as 别名；
select students.name, students.age from students;
select s.name,s.age from students as s;
--失败的select students.name,students.age from students as s;

--消除重复行
--distinct 字段
select distinct gender from students;
```
## 2.条件
### 比较运算符
**select ...from 表名 where**
```mysql
--查询大于18岁的信息
select * from students where age>18;
select id,name,gender from students where age>18;
--查询小于18岁的信息
select * from students where age<18;
--查询小于或者等于18岁的信息
--查询年龄为18岁的所有学生的名字
select * from students where age=18;
-!=或者<>
```
### 逻辑运算符
#### **and**
```mysql
--18到28之间的所有学生信息
select * fromn students where age>18 and age<28;
--失败select * from students where age>18 and <28;
--18岁以上的女性
select * from students where age>18 and gender="女";
select * from students where age>18 and gender=2;
```
#### **or**
```mysql
--18以上或者身高查过180（包含）以上
select * from studentss where age>188 or height>=180;
```
#### **not**
```mysql
--不在 18岁以上的女性 这个范围内的信息
select * from students where not age>18 and gender=2;
select * from students where not (age>18 and gender=2);
--年龄不是小于或者等于18 并且是女性
select * from students wheree（not age<=18)and gender=2;
```
### 模糊查询
#### **like**
-- **%** 替换1个或者多个
--替换1个
```mysql
--查询姓名中以“小”开始的名字
select name from students where name="小";
select name from students where name like“小%”；
--查询姓名中有“小”所有的名字
select name from students  where name like“%小%”；
--查询有2个字的名字
select name from students where name like "_";
--查询有3个字的名字
select name from students where name like"_";
```
--**查询至少有2个字的名字**
`select name from students where name like"_%";`
#### **rlike 正则**
```mysql
--查询以 周开始的姓名
select name from students where name like"^周.*";
--查询以 周开始、伦结尾的姓名
select name from students where name like"^周.*伦$";
```
### 范围查询
#### **in(1,3,8)** 
 **int()** 表示在一个非连续的范围内
```mysql
--查询年龄在18、34的姓名
select name,age from students where age in(18,34);
```
#### **between**...**and**...
**between**... **and** ...表示在一个连续的范围内
```mysql
--查询 年龄在18到34之间的的信息
select name,age from students where age between 18 and 34;
--not between...and...表示不在一个连续的范围内
--查询 年龄不在在18到34之间的的信息
select from students where age not between 18 and 34;
```

#### **空判断** is null
```mysql
--查询身高为空的信息
select * from students where height is null;
select * from students where height is not null;
```

## **3.排序**
**order by**   **asc**从小到大 , **desc**从大到小
```mysql
--查询18到34岁的男性，按照年龄从小到大排序
select * from students where (age between 18 and 34) and gender = 1 order by age acs;
--查询18到34岁的男性，按照身高从大到小排序
select * from students where (age between 18 and 34) and gender = 1 order by height desc
--查询18到34岁的男性，按照身高从大到小排序，如果身高相同的情况下按照年龄从小到大排序
select * from students where (age between 18 and 34) and gender = 1 order by height desc,age acs;
--按照年龄从小到大、身高从高到矮的排序
select * from students order by age asc, height desc;
```
## 4.聚合函数
### count(总数)
```mysql
--查询男性有多少人，女性有多少人
select count(*) as 男性人数 from students where gender = 1;
select count(*) as 女性人数 from students where gender = 2;
```
### max
```mysql
--查询最大年龄
select max(age) from students;
```
### min
```mysql
--查询女性的最小身高
select min(hegiht) from students where gender = 2
```
### sum
```mysql
--计算所有人的年龄之和
select sum(age) from students;
```
### avg
```mysql
--计算平均年龄
select avg(age) from students;
--计算平均年龄
select sum(age)/count(*) from students;
```
### round
```mysql
--四舍五入 round（123.23，1）保留1位小数 123.2
--计算所有人的平均年龄，保留2位小数
select round(avg(age),2) from students;
--计算男性的平均身高,保留2位小数
select round(avg(height),2) from students where gender = 2;
```
## **5.分组**
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
```mysql
--查询同种性别中的姓名(男性)
select gender,group_concat(name) from students where gender=1 group by gender;
select gender,group_concat(name, age, id) from students where gender=1 group by gender;
select gender,group_concat(name,", age, "", id） from students where gender=1 group bygender;
```
### having（后面不接where）
having 和where一样。having用在分组之后的条件
```mysql
--查询平均年龄超过30岁的性别，以及姓名  having avg（age）>30
select gender, group_concat(name),avg(age) from students group by gender having avg(age)>30;
--查询每种性别中的人数多于2个的信息
select gender,group_concat(name) from students group by gender having count(*)>2;
```

## **6.分页**
### limit  start,count
```MySQL
--限制查询出来的数据个数(显示查出来的个数)
select * from students where gender=1 limit 2;
--查询前5个数据
select * from students limit 0,5;
--查询id 6-10（包含）的书序
select * from students limit 5,5;
--每页显示2个，第1个页面
select * from students limit 0,2;
--每页显示2个，第2个页面
select * from students limit 2,2;
--每页显示2个，第3个页面
select * from students limit 4,2;
--每页显示2个，第4个页面
select * from students limit 6,2;
```

**规律**：*limit（第N页-1）*每个的个每页的个数*；

每页显示2个，显示第6页的信息，按照年龄从小到大排序
--失败`select * from students limit 2 *（6-1）,2;`
--失败`select * from students limit 10,2 order by age asc;
--正确`select * from students`
## 7.连接查询
### inner join 
**inner join 表名 on         内连接**  

![Pasted image 20241008104744](https://github.com/user-attachments/assets/61f6e2ab-48d0-4db0-b230-4b02182529d4)



```mysql
--查询 有能够对应班级的学生以及班级信息
select * from students inner join classes on students.cls_id = classes.id;

！！！！
--按要求显示姓名、班级
select students.*,classes.name from students inner join classes on students.cls_id = classes.id;
--按显示学生姓名、班级
select students.name,classes.name from students inner join classes on students.cls_id = classes.id;

--给数据表换个名字
select s.name,c.name from students as s inner join classes as c on s.cls_id = c.id;

！！！！
--查询 有能够对应班级的学生以及班级信息,按照班级进行排序
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id order by c.name;
--当时同一个班级的时候按照学生的id进行从小到大排序
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id order by c.name,s.id;
```

### left join
**left join 表名 on    左连接**
![Pasted image 20241008110750](https://github.com/user-attachments/assets/d9e00e7a-4075-4a45-9c00-4048e0369ca1)

左连接查询：查询的结果为两个表匹配到的数据，左表特有的数据，对于右表中不存在的数据使用nul填充
```mysql
--查询每位学生对应的班级信息
select * from students left join classes on students.cls_id = classes.id;
```
![Pasted image 20241008111624](https://github.com/user-attachments/assets/d6090b3d-9325-43b5-b2fb-573881c5b91a)


```mysql
--查询没有对应班级的学生
select * from students as s left join classes as c on s.cls_id = c.id having c.id is null;
```

![Pasted image 20241008112556](https://github.com/user-attachments/assets/0b2ea055-ed70-4ab2-8fe0-2587ba967d80)

## 8.自关联
```mysql
--查询有多少省份
select * from areas where pid is null;

!!!!
--查询山东省有哪些市
select * from areas as province inner join areas as city on city.pid = province.aid having province.atitle = "山东省";
select province.atitle,city.atitle from areas as province inner join areas as city on city.pid = province.aid having province.atitle = "山东省";
--查询青岛市有哪些县城
select province.atitle,city.atitle from areas as province inner join areas as city on city.pid = province.aid having province.atitle = "青岛市";
```
## 9.子查询
```mysql
--查询最高的男生信息
select * from students where height=188;
select * from students where height=(select max(height) from students);
```
**子查询** 相比自关联 查询速度慢，一般不用
