# DML数据操作语言

(INSERT、UPDATE、DELETE)  用于操作数据库对象中所包含的数据

## 一、INSERT语句（增）

```mysql
INSERT INTO 表名 [(字段名1,字段名2,...)] VALUES ('值1','值2',...)[,('值1','值2',...)][;]
TIP：不写字段名表示对表中所有数据增加
```
```mysql
   INSERT INTO `subject` (subjectId,SubjectName,ClassHour,gradeId) 
   		VALUES (3,'JAVA',100,2);
   INSERT INTO student (`name`,sex,phone,birth,address,gradeId)
   		VALUES  ('李四','女','14824595834','2004-05-20','徐州',1),
   				('king','男','12300000000','2000-12-01','南京',1),
   				('dell','男','12300000044','2000-12-05','南京',1);
   INSERT INTO grade (gradeName) VALUES ('大一') , ('大二') , ('大三') , ('大四');
```



## 二、DELETE、TRUNCATE语句（删）

```mysql
DELETE FROM 表名 [WHERE 条件表达式][;]
TIP:不加WHERE条件会把全部数据删除

TRUNCATE 表名[;]
```

> TIP：DELETE与TRUNCATE的区别
> （1） 使用delete删除表中的数据，如果事务没提交，事务回滚（后悔）时删除的数据还能被恢复
> （2） 默认情况下，数据库中的事务自动提交。    turncate不支持事务
> （3） 使用truncate删除所有数据，自增字段会重置回从1开始

```mysql
DELETE FROM grade WHERE gradeId = 8;
DELETE FROM grade;

TRUNCATE grade;
```



## 三、UPDATE语句（改）

```mysql
update 表名 set 字段名 = 值 [,字段名2 = '值2',...] [where 条件表达式][;]
TIP:不加WHERE条件会把对应字段的全部数据修改
```

> 等于 =
>不等于 <> / !=
> 大于 >
> 小于 <
> 大于等于 >=
> 小于等于 <=
> 并且 and / &&
> 或者 or / ||
> 范围之间 between 值1 and 值2

```mysql
UPDATE student SET `sex` = '女' WHERE id = 2;
UPDATE student SET `name` = 'king', phone = '12345678900' WHERE id = 2;
UPDATE `subject` SET ClassHour = ClassHour + 10  WHERE ClassHour < 50;
UPDATE `subject` SET ClassHour = ClassHour - 10  WHERE ClassHour > 100 && ClassHour < 200;
UPDATE `subject` SET ClassHour = ClassHour - 10  WHERE ClassHour BETWEEN 100 AND 200;
UPDATE `subject` SET ClassHour = ClassHour - 10  WHERE ClassHour > 110 OR gradeId = 1;
```

