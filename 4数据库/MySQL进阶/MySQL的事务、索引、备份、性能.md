# MySQL的事务、索引、备份、性能

## 一、事务

定义：将一组SQL语句放在同一批次内去执行

事务必须具备以下四个属性，简称ACID属性

**原子性**（Atomicity）、**一致性**（Consistency）、**隔离性**（Isolation）、**持久性**（Durability）



▼ 示例：使用事务模拟顾客A给卖家B转账的操作(事务成功) 

```mysql
# 关闭自动提交模式(默认开启)
SET AUTOCOMMIT = 0;

#开启事务
START TRANSACTION;

# 事务的操作
UPDATE account SET cash = cash - 500 WHERE `name` = '顾客A';
UPDATE account SET cash = cash + 500 WHERE `name` = '商家B';

#如果前面两条SQL语句没有错误，将事务提交到数据库中
COMMIT;

#开启自动提交模式
SET AUTOCOMMIT = 1;
```

▼ 示例：使用事务模拟顾客A给卖家B转账的操作(事务失败) 

```mysql
#关闭自动提交模式(默认开启)
SET AUTOCOMMIT = 0;

#开启事务
START TRANSACTION;

# 事务的操作 
UPDATE account SET cash = cash - 500 WHERE `name` = '顾客A';
UPDATE account SET cash = cash + 500 WHERE `name` = '卖家B';

#事务回滚，数据回到本次事务的初始状态 
ROLLBACK;

#开启自动提交模式	
SET AUTOCOMMIT = 1;
```



## 二、索引

### 1.作用
* 提高查询速度：使用分组和排序子句进行数据检索时，可以显著减少分组和排序的时间 全文检索字段进行搜索优化（全文索引fulltest）
* 确保数据的唯一性（主键索引primary key、唯一索引unique）
* 可以加速表和表之间的连接，实现表与表之间的参照完整性（外键索引）

### 2.索引类型

* #### 主键索引（PRIMARY KEY）
  确保数据记录的唯一性（必需非空 NOT NULL）
  确定特定数据记录在数据库中的位置
  只能有一个主键索引，但可以有多个字段组成（复合主键）

  

* #### 唯一索引（UNIQUE）

  避免同一个表中某数据列中的值重复（可以为空 NULL 但只能有一个NULL值）
  唯一索引可有多个

  

* #### 常规索引（INDEX）
  index和key关键字都可设置常规索引
  应加在查找条件的字段
  不宜添加太多常规索引，影响数据的插入、删除和修改操作建表后追加
  ALERT TABLE 表名 ADD  索引类型（数据列名）

  

* #### 全文索引（FULLTEXT）
只能用于MyISAM类型的数据表 MyIASM引擎不支持事务 几乎不使用
只能用于 CHAR 、 VARCHAR、TEXT数据列类型
适合大型数据集



### 3.查看索引

```mysql
SHOW  INDEX / KEYS FROM 表名
```

```mysql
ALTER TABLE `result` ADD INDEX `INDEX_ID_NO` (`StudentNo`,`SubjectId`);
DROP INDEX `FULLTEXT_NAME` ON `category`;
ALTER TABLE `category` DROP INDEX `FULLTEXT_NAME`;
SHOW INDEX FROM `category`;
SHOW KEYS FROM `category`;
```



### 4.删除索引

```mysql
DROP  INDEX 索引名 ON    表名

ALTER TABLE 表名   DROP  INDEX  索引名

ALTER TABLE 表名   DROP  PRIMARY KEY
```

### 5.注意

* 索引不是越多越好

* 不要对经常变动的数据加索引

* 小数据量的表建议不要加索引

* 索引一般应加在查找条件的字段



## 三、MySQL备份

* 使用SQLyog工具  
	导出 选中数据库右键-备份/导出-备份数据库
	导入 选中连接右键-执行SQL脚本

* 使用mysqldump命令 
	导出  mysqldump -u root -p 库名 > 绝对路径 
	导入  先登录mysql -u root -p  ------->  USE 库名  ------->  source 绝对路径

* 使用SQL语句(纯数据)
	导出  SELECT * INTO OUTFILE '绝对路径' FROM 表名;
	导入  LOAD DATA INFILE '绝对路径' INTO TABLE 表名;

	```mysql
	SELECT * INTO OUTFILE 'd://student.xls' FROM student;
	LOAD DATA INFILE 'd://student.txt' INTO TABLE student2;
	```
	
	



##四、性能
```mysql
EXPLAIN  表名  （DESC 表名）

EXPLAIN  SELECT语句

EXPLAIN `student`;
EXPLAIN SELECT * FROM student WHERE studentname LIKE '朱%';
```







