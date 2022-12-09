[TOC]

#### @author：an ####

### 1、MySQL：sql语句

```mysql
① 创建表语句: create table name(...);
② 登录语句：mysql -u root -p 回车
		   输入密码 回车
		   登录成功
③ 命令行登录没有权限：
	cmd : 右键 管理员角色运行
	防火墙：s尝试关闭防火墙再试
④ 设置某个字段主键自增不为空：
	sid Integer(10) NOT NULL AUTO_INCREMENT PRIMARY KEY
⑤ 修改表的结构：alter table t_name 
	a、增加某个字段：alter table stu1 add column type
	b、对某个列进行修改：alter table stu1 modify column type ...其他约束
	c、某个字段冗余，删除该字段：alter table stu1 drop column(慎用或者不用)
⑥ 删除某张数据表：drop table 表名；
```

### 2、mysql数据类型：

```mysql
①数值类型:int  double decimal
②日期：DATE、TIME...
③字符串：
	a、char(定长字符，无论是否全部使用，都会分配存储空间)
		s_name char(20)
		s_name1 -> "yu" -> 在存储空间中也是占用20个长度
	b、varchar(可变长字符，最多占用用户申请的存储空间，如果未完全占用，则按照实际的使用控件进行分配)
		s_name varchar(20)
		s_name1 -> "yu" -> 按照实际使用2个长度占用空间
```

### 3、mysql字段约束：字面意思约束意为限制，需要满足某个规则，或者某个条件,称之为约束：

```mysql
① 实体完整性约束：PRIMARY KEY、UNIQUE、auto_increment
	注意： auto_increment自增不能单独使用，需要与primary key结合使用，即如果一个字段被使用了auto_increment自增约束，则一定会被设置为primary key
② 域完整性约束：
	a、非空约束: not null
	b、默认值：default value
③ 引用完整性约束：描述多个表结构之间的关系
	a、外键约束：foreign key
	b、语法：constraint cons_name 
				forgign key(column)
				references table1(column1)
```

### 4、MySQL的CRUD操作

```mysql
①向数据表中插入数据：
	insert into table1(column1, column2, column3)
		values(value1,value2,value3);
实际开发中sql使用占位符:
	String sql = "insert into table1(column1, column2) 
	values(?,?)"
占位符的作用：防止sql注入

①基本查询语句：
a、select column1, column2, column3 from table1
b、select column1 nick1, column2 nick2 from table1
c、select column1, column2 from table1,table2
	多个表查询：笛卡尔积
d、条件查询：
 ① where 子句： where 子句可以包含多个条件
 	and：多个条件必须同时存在才算满足条件
 	or：多个条件需要同时满足，会将满足所有的条件的数据均返回
 	like：模糊查询。%可以匹配所有内容
 		前置模糊："xx%"
 		中间模糊："%xxx%"
 		后置模糊："%xxx"
 ② 对查询到的数据进行相关操作：
 	a、数值：算术运算
 	b、字符串：使用字符串函数对字符串进行操作，比如截取
 	c、日期： 可以通过日期函数进行特殊的处理，比如获取特定年份，获取特定月份
② 高级查询（符合查询）：
	对查询结果进行排序：order by column1, column2 规则
	排序规则两种：asc(升序)、desc(降序)
```

#### 	5、Where查询：

```
① 区间查询：between... and...
	查询符合要求的在某个范围之内的数据
② 排序：order by 列名 asc/desc;
```

### 查询成绩在70到90之间的所有科目的同学的信息成绩并以成绩进行升序排列

① 本质：查询成绩，以分数升序

```mysql
select aaa from bbb score where ccc and socre.num>=70 and socre.num <=90 order by num asc
```

② 科目信息、学生信息
分析：科目信息在course表中
	 学生姓名在student表中
aaa代表的是：student.sname, course.cname, score.num

分析，由于aaa查询的字段存在于三张表中，因此，需要重多个表中进行查询：
bbb代表的是： student、course、score

进一步分析，三张表（多张表联合查询），需要添加联合查询条件,即

ccc为连接条件：
	因为student为基础表、course也是基础表、score是业务扩展表，因此需要socre分别与student和course进行连接
   score.student_id = student.sid
   and 
   score.course_id = courese.cid

因此，最终的SQL语句是:

```mysql
select
	stu.sname, c.cname, s.num
from
	student stu,
	course  c,
	score   s
where
	s.student_id = stu.sid
	and
	s.course_id = c.cid
	and 
	s.num >= 70 and s.num <= 90
order by s.num asc;
```

在上述sql功能的基础上，输出所有符合要求的数据的同学的班级信息
分析：班级信息是在class表，class表是一个基础表
	student表与class表有外键关联关系，在student表中,有一个外键，关联到了class表中的cid

```mysql
select
	cla.caption, stu.sname, c.cname, s.num
from
	class   cla,
	student stu,
	course  c,
	score   s
where
	stu.class_id = cla.cid
	and 
	s.student_id = stu.sid
	and
	s.course_id = c.cid
	and 
	s.num >= 70 and s.num <= 90
order by s.num asc;
```

在完成上述功能的基础上，查询并输出对应课程的任课老师的信息：
分析：老师的信息是基础信息表，在teacher表中,该表被课程表所关联引用。在course表中，有一个外键teacher_id关联自teacher表，因此:

```mysql
select
	cla.caption, stu.sname, c.cname, s.num, t.tname
from
	class   cla,
	student stu,
	course  c,
	score   s,
	teacher t
where
	stu.class_id = cla.cid
	and 
	s.student_id = stu.sid
	and
	s.course_id = c.cid
	and 
	c.teacher_id = t.tid
	and 
	s.num >= 81 and s.num <= 100
order by s.num asc;
```

③ 枚举：在给定的数值之间，表示符合数据

```mysql
num in(70，90，86)
```

④ substring(column1,start,length)：对某个字段进行截取。
注意：start从1开始，而非从0开始。

⑤ 数学聚合函数:应用对象为字段
	求和：sum,将指定列当中的每个数据进行相加，返回总和
	计数：count，对筛选查询的数据进行计数计算，返回共多少条数据

需求：请查询出三年级三班班级的分数在80分以上的人数?

```mysql
select count(num) from score where score.num > 80 and bbb 
```

分析：sql语句的基本结构已经写出，只需要完成部分条件即可
信息“三年级三班”，该字段信息在class基础表中，因此，如果通过班级信息进行筛选，需要将score表与class表进行连接

```mysql
select count(num) student_count
from
	score s,
	student stu,
	class cla
where 
	s.student_id = stu.sid
	and
	stu.class_id = cla.cid
	and 
	cla.caption = '三年级三班'
	and 
	s.num > 80;
```

需求：查询出学生张三的各科总成绩?
查询出学号为xxx的

```mysql
select sum(num) 
from 
	score s,
	student stu
where
	s.student_id = stu.sid
	and 
	stu.sname = '张三';
```

分析：“张三”信息在student这张基础表中,不在成绩表中;如果想要使用"张三"作为条件，需要进行表的连接
	让student和score表进行连接

```mysql
	student.sid = score.student_id
```

⑥ 平均值函数：算平均数
需求：查询出三年级三班的物理这门课的平均分数?

```mysql
select avg(num) 
from
	score
where 
	aaaa
```

分析：定语：三年级三班的物理这门课
a、 三年级三班：存在于基本表中的数据，class
b、 物理：存在于基本表course中
因此，要想使用这两个数据作为条件，需要将上述两个表与成绩表进行连接

```mysql
class.cid = 
and
courese.cid = score.courese_id
```



问题：在score表和course表中，没有一个外键引用自class.cid，因此如果想要将class表连接，需要引入另外一个表student，因为student表中有一个外键引用自class

因此，需要再次引入student表,最终，表的连接变为：

```mysql
from
	score s,
	course c,
	class cla,
	student stu
where 
	s.student_id = stu.sid
	and 
	c.cid = s.course_id
	and 
	stu.cid = cla.cid
	and 
	cla.caption = '三年级三班'
	and 
	c.cname = '物理';

select avg(num) average 
from
	score s,
	course c,
	class cla,
	student stu
where 
	s.student_id = stu.sid
	and 
	c.cid = s.course_id
	and 
	stu.class_id = cla.cid
	and 
	cla.caption = '三年级三班'
	and 
	c.cname = '物理';
```

⑦ group by：分组查询
注意：使用group by分组查询时，需要和聚合函数一起使用

⑧ 查询字句：在查询语句当中，使用查询语句得到的结果作为数据源

```mysql
	select *
	from
	(select * from score) as qs;
```

注意：在使用查询子句时，需要特别注意使用规范的限制条件进行查询.

⑨ union：联合查询
结构：
	(select1字句) 
	union 
	(select2字句)
	union 
	(select3字句)
	..
注意：
	a、union查询可以涉及同一张表，也可以是多张表
	b、如果使用union，各个select子句查询的列数必须相等，否则会报错：ERROR 1222 (21000): The used SELECT statements have a different number of columns