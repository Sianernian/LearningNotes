```mysql
create database studentsdb;

use studentsdb;

create table student_info(
	s_id  			char(4) not null  primary key,
	s_name 		 	char(8)  not null  ,
	s_sex 			char(2) ,
	s_data	 		 date,
	s_addr 	 		varchar(50)
)engine=InnoDB default charset=utf8;


create table curriculum(
	class_id 			char(4) not null  primary key,
	class_name 		 	char(50),
	score   			int
)engine=InnoDB default charset=utf8;



create table grade(
	s_id 			char(4) not null ,
	class_id   		char(4) not null ,
	point   		int,
    PRIMARY KEY ( s_id , class_id)
)engine=InnoDB default charset=utf8;

('0001','张青平','男','2000-10-01','衡阳市东风路77号'),
('0002','刘东阳','男','1998-12-09','东阳市八一北路33号');
insert into  student_info
(s_id,s_name,s_sex,s_data,s_addr)
values
('0003','马晓夏','女','1995-05-12','长岭市五一路763号'),
('0004','钱忠理','男','1994-09-23','滨海市洞庭大道279号');
('0005','孙海洋','男','1995-04-03 ','长岛市解放路27号'),
('0006','郭小斌','男', '1997-11-10' ,'南山市红旗路113号'),
('0007','肖月玲','女'  ,'1996-12-07' ,'东方市南京路11号' ),
('0008','张玲珑','女 ', '1997-12-24', '滨江市新建路97号'); 

```







| 学号 | 姓名   | 性别 | 出生日期   | 家族住址            |
| ---- | ------ | ---- | ---------- | ------------------- |
| 0001 | 张青平 | 男   | 2000-10-01 | 衡阳市东风路77号    |
| 0002 | 刘东阳 | 男   | 1998-12-09 | 东阳市八一北路33号  |
| 0003 | 马晓夏 | 女   | 1995-05-12 | 长岭市五一路763号   |
| 0004 | 钱忠理 | 男   | 1994-09-23 | 滨海市洞庭大道279号 |
| 0005 | 孙海洋 | 男   | 1995-04-03 | 长岛市解放路27号    |
| 0006 | 郭小斌 | 男   | 1997-11-10 | 南山市红旗路113号   |
| 0007 | 肖月玲 | 女   | 1996-12-07 | 东方市南京路11号    |
| 0008 | 张玲珑 | 女   | 1997-12-24 | 滨江市新建路97号    |