# DDL数据定义语言

(CREATE、DROP、ALTER)  定义和管理数据对象，建库建表 删库删表		

```mysql
USE myschool;
```




### 创建数据库

```mysql
create databases [if not exists] 库名;
```

```mysql
CREATE DATABASE IF NOT EXISTS `myschool`;
```



### 查看所有数据库

```mysql
SHOW DATABASES
```



###使用数据库作为当前操作对象

```mysql
use 库名;
```

```mysql
USE `myschool`;
```



###创建表
```mysql
create table [if not exists] 表名(
	字段名 列类型 [属性] [索引] [注释],
	字段名2 列类型 [属性] [索引] [注释]
	[,......]
)[engine] [charset] [comment] [;]
```

> 属性： [primary key] [[not] null] [default 'M'] [auto_increment] [comment 'M']  [unsigned] [zerofill]
> 常用类型(最大长度)：int(11)	char(255)	varchar	date	time	datetime
> engine：**innoDB**	MyISAM	HEAP	BOB	CSV

```mysql
SHOW ENGINE;
SHOW FUNCTION STATUS;
```

```mysql
CREATE TABLE IF NOT EXISTS `student`(
	id INT PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '学生编号',
	`name` VARCHAR(255) NOT NULL COMMENT '学生姓名',
	sex CHAR NOT NULL DEFAULT '男' COMMENT '学生性别',
	phone VARCHAR(11) COMMENT '手机号',
	birth DATE NOT NULL COMMENT '生日',
	address VARCHAR(500) NOT NULL COMMENT '地址'
)ENGINE=INNODB CHARSET=utf8 COMMENT='学生信息表';
```



### 删除数据库

```mysql
drop database [if exists] 库名;
```

```mysql
DROP DATABASE IF EXISTS `myschool`;
```



### 删除表

```mysql
drop table 表名;
```

```mysql
DROP TABLE `grade`;
```



### 修改表命

```mysql
alter table 旧表名 rename as 新表名;
```

```mysql
ALTER TABLE `student` RENAME AS `sutdentInfos`;
```



### 添加字段到表

```mysql
alter table 表名 add 字段名 列类型 [属性]; 
```

```mysql
ALTER TABLE `subject` ADD `version` INT(11) DEFAULT '1' COMMENT '乐观锁';
```



### 删除表中字段

```mysql
alter table 表名 drop 字段名;
```

```mysql
ALTER TABLE `subject` DROP `version`;
```



### 修改表数据

```mysql
alter table 表名 modify 字段名 列类型 [属性];

alter table 表名 change 旧字段名 新字段名 列类型 [属性];
```

```mysql
ALTER TABLE `subject` CHANGE subjectNo subjectId INT(11);
```



### 查看表中的字段

```mysql
desc 表名;
```

```mysql
DESC `student`;
```



### 显示创建语句

```mysql
show create table 表名;
```

```mysql
SHOW CREATE TABLE `student`;
SHOW CREATE DATABASE `myschool`;
```

