# DQL数据查询语言

(SELECT)  用于查询数据库对象中所包含的数据

## SELECT语句（查）

```mysql
SELECT [DISTINCT] 
    [表名(表别名).]字段名1 [[AS']字段别名1[']],
    [表名(表别名).]字段名2 [[AS']字段别名2[']],
    ... 
FROM 表名 [[AS']表别名[']] 
    [, 表名2 [[AS']表别名2[']]]
    [JOIN 表名3 [[AS']表别名3[']] ON 连接条件1]
    [LEFT JOIN 表名4 [[AS']表别名4[']] ON 连接条件2]
    [RIGHT JOIN 表名5 [[AS']表别名5[']] ON 连接条件3]
    [FULL JOIN 表名6 [[AS']表别名6[']] ON 连接条件4]
    [CROSS JOIN 表名7 [[AS']表别名7[']]]
WHERE 筛选条件
GROUP BY [表名(表别名).]字段名1, [表名(表别名).]字段名2, ...
HAVING 分组筛选条件
ORDER BY [表名(表别名).]字段名1 [ASC|DESC], [表名(表别名).]字段名2 [ASC|DESC], ...
LIMIT 数量 [OFFSET 偏移量];
```



##一、单表查询

### 1.查询部分字段

```mysql
SELECT StudentNo,Sex,StudentName FROM student;
```

### 2.查询全部字段

查询全部字段使用 * 

```mysql
SELECT * FROM student;
SELECT StudentNo AS'学生编号',StudentName '学生姓名',Sex 性别  FROM student s1;

SELECT phone+1 tel FROM student;

SELECT subjectId FROM result;
```

### 3.DISTINCT关键字

DISTINCT关键字 用于去除重复项

```mysql
SELECT DISTINCT subjectId FROM result;
```

### 4.WHERE条件查询子句

使用WHERE关键字对查询加以条件限制，可以是表达式、函数等等

```mysql
SELECT 100*3 , VERSION();

SELECT SubjectId,student.studentNo,studentName,StudentResult+10 '原成绩+10' FROM result,student WHERE SubjectId=1;

SELECT StudentNo 学号, StudentName 姓名, Sex 性别, GradeId 年级, Phone 电话 FROM student WHERE GradeId=1;

SELECT SubjectName 课程名称,ClassHour 总课时,ClassHour/10 '均课时/天' FROM `subject`;

SELECT studentNo,studentName,sex,phone,address,email,BornDate FROM student WHERE BornDate >= '1996-01-01' AND sex='男';

SELECT gradeid,studentNo,studentName,sex,phone,address,email,BornDate FROM student WHERE gradeId=1 OR gradeId=2;

SELECT gradeid,studentNo,studentName,sex,phone,address,email,BornDate FROM student WHERE gradeId BETWEEN 1 AND 2;

SELECT SubjectId,SubjectName,ClassHour,GradeId FROM `subject` WHERE SubjectId=1;
```

### 5.IN、BETWEEN、AND关键字

in 表示取值范围  	或者使用between and

```mysql
SELECT StudentNo,StudentName, Phone, Address FROM student WHERE gradeId IN (
    SELECT gradeId FROM grade WHERE gradeName='S1' OR gradeName='S2'
);
SELECT StudentNo,StudentName, Phone, Address FROM student WHERE gradeId IN (1,2);
SELECT StudentNo,StudentName, Phone, Address FROM student WHERE gradeId BETWEEN 1 AND 2;
```

### 6.ORDER BY排序查询子句

```mysql
order by 字段1 desc/asc [,字段2 desc/asc] [,...]
```

desc降序 asc升序	默认为asc

```mysql
SELECT `StudentNo`,`StudentName`,TIMESTAMPDIFF(YEAR, `BornDate`, CURDATE()) 'age',`BornDate`,`Email`
	FROM `student`
	ORDER BY `BornDate` ASC;
	
SELECT `StudentNo`,`StudentName`,`BornDate`,`Email`,`GradeId`
	FROM `student`
	ORDER BY `GradeId` ASC,`BornDate` DESC;

SELECT `student`.`StudentNo`,`StudentName`,`StudentResult`,`SubjectName`
	FROM `student` LEFT JOIN `result` ON `student`.`StudentNo`=`result`.`StudentNo`
	LEFT JOIN `subject` ON `result`.`SubjectId` = `subject`.`SubjectId`
		WHERE `SubjectName` = '设计MySchool数据库'
		ORDER BY `StudentResult` DESC;
```

###  7.LIMIT限制查询子句

```mysql
limit [m,]n  或者  limit n offset m
```

从第m + 1条开始，默认为0(第一条数据下标从0开始)	显示n条记录

```mysql
SELECT * FROM `student` LIMIT 5;

SELECT * FROM `student` LIMIT 0,10;
SELECT * FROM `student` LIMIT 10 OFFSET 0;
```

### 8.GROUP BY分组查询子句

```mysql
group by 字段;		按照字段进行分组显示

having 条件;	     用条件对分组后的数据进一步筛选
```

```mysql
SELECT `GradeId`,COUNT(`StudentNo`) FROM `student` 
GROUP BY `GradeId`;

SELECT `result`.`SubjectId`,`SubjectName`,MAX(`StudentResult`),MIN(`StudentResult`),AVG(`StudentResult`) FROM `result`
LEFT JOIN `subject` ON `result`.`SubjectId`=`subject`.`SubjectId`
WHERE `StudentResult` >= 60
GROUP BY `SubjectId`;

SELECT `result`.`SubjectId`,`SubjectName`,AVG(`StudentResult`) 'avgResult' FROM `result`
LEFT JOIN `subject` ON `result`.`SubjectId`=`subject`.`SubjectId`
GROUP BY `SubjectId`
HAVING avgResult > 75;

SELECT `SubjectId`,`SubjectName` FROM `subject` WHERE `SubjectId` IN(
	SELECT `SubjectId` FROM (
		SELECT AVG(`StudentResult`) 'avgResult',`SubjectId` FROM `result`
		GROUP BY `SubjectId`
		HAVING avgResult > 75
	) temp
);

SELECT `grade`.`GradeId`,`GradeName`,COUNT(`StudentNo`) 'maleSexCount' FROM `grade`
LEFT JOIN `student` ON `grade`.`GradeId`=`student`.`GradeId`
WHERE `Sex`='男'
GROUP BY `GradeId`
HAVING maleSexCount > 10;

SELECT `GradeId`,`GradeName` FROM `grade` WHERE `GradeId` IN (
	SELECT `GradeId` FROM (
		SELECT COUNT(`StudentNo`) 'maleSexCount',`GradeId` FROM `student`
		WHERE `Sex` = '男'
		GROUP BY `GradeId`
		HAVING maleSexCount > 10
	) temp
);

SELECT `grade`.`GradeId`,`GradeName` FROM `grade`
LEFT JOIN (
	SELECT COUNT(`StudentNo`) 'maleSexCount',`GradeId` FROM `student`
	WHERE `Sex` = '男'
	GROUP BY `GradeId`
	) temp
ON `grade`.`GradeId`=temp.`GradeId`
WHERE maleSexCount > 10;

SELECT `SubjectName`,MAX(`StudentResult`) 'maxr',MIN(`StudentResult`) 'minr',AVG(`StudentResult`) 'avgr'
FROM `result`
INNER JOIN `subject` ON `result`.`SubjectId`=`subject`.`SubjectId`
GROUP BY `result`.`SubjectId`
HAVING avgr >= 60;

SELECT `SubjectName`,maxr,minr,avgr
FROM `subject`
LEFT JOIN(
SELECT `SubjectId`,MAX(`StudentResult`) 'maxr',MIN(`StudentResult`) 'minr',AVG(`StudentResult`) 'avgr'
FROM `result`
GROUP BY `SubjectId`
HAVING avgr >= 60 )temp
ON `subject`.`SubjectId`=temp.`SubjectId`;


```

### 9.LIKE模糊查询子句

like 用于匹配字符  可以与通配符一起使用，%匹配0个或者任意多个字符	_匹配单个字符

```mysql
SELECT studentname FROM student WHERE studentName LIKE '姜%';

SELECT studentname FROM student WHERE studentName LIKE '姜_';

SELECT SubjectName FROM `subject` WHERE SubjectName LIKE '%系统';

SELECT SubjectName FROM `subject` WHERE SubjectName LIKE '使用%';

SELECT studentNo '"李"同学学号',StudentResult '"李"同学成绩' FROM result WHERE studentNo IN (
    SELECT studentNo FROM student WHERE studentname LIKE '张%'
);
```

匹配字符默认不区分大小写

```mysql
SELECT SubjectId,SubjectName,ClassHour,GradeId FROM `subject` WHERE SubjectName LIKE '%JAVA%';
```

### 10.NULL空值

字段 is [not] null

```mysql
SELECT StudentNo,StudentName, Phone, Address FROM student WHERE address IS NULL;

SELECT StudentNo,StudentName, Phone, Address FROM student WHERE Email IS NOT NULL;
```

没有赋值过的字段数据才是null  null的数据表现为  (NULL)

如果之前有数据/(NULL)现在删除完所有字符 或者 添加一个空字符串，就不是null了

###			11.多表查询

如果从多张表中查询数据，查询的字段在表中有相同的字段，查询时必须要指定从哪个表

```mysql
SELECT s.StudentNo,StudentName,StudentResult FROM student s,result;
```



## 二、连表查询（Join）

### 1.内连接查询 （INNER JOIN）

```mysql
表1 [inner] join 表2 on 关联表达式    内连接查询
```

表1的每一条数据逐条分别跟表2的每一条数据关联  如果满足关联条件就保留，否则不保留

```mysql
SELECT s.studentNo,s.studentName,r.studentResult 
	FROM student s INNER JOIN result r ON s.StudentNo = r.StudentNo;

SELECT s.studentNo,s.studentName,r.studentResult  FROM student s ,result r 
	WHERE s.StudentNo = r.StudentNo;
	
SELECT studentNo,studentName,gradeName
	FROM student INNER JOIN grade ON student.GradeId = grade.GradeId
			WHERE gradeName = 'S1';
			
SELECT `StudentNo`,`StudentName`,`Phone`,`Email`,`Sex`
	FROM student WHERE `GradeId`=(
		SELECT `GradeId` FROM `grade` WHERE `GradeName`='S1'
	);
	
SELECT `StudentNo`,`StudentName`,`Phone`,`Email`,`Sex`
	FROM `student` INNER JOIN `grade` ON `student`.`GradeId` = `grade`.`GradeId`
		WHERE `GradeName` = 'S1';

SELECT `student`.`StudentNo`,`StudentName`,`ExamDate`,`StudentResult`,`grade`.`GradeId`,`GradeName`
	FROM `student` INNER JOIN `result` INNER JOIN `grade`
	ON `student`.`GradeId`=`grade`.`GradeId` AND `student`.`StudentNo` = `result`.`StudentNo`;

SELECT `student`.`StudentNo`,`StudentName`,`ExamDate`,`StudentResult`,`grade`.`GradeId`,`GradeName`
	FROM `student` INNER JOIN `result` ON `student`.`StudentNo` = `result`.`StudentNo`
	INNER JOIN `grade` ON `student`.`GradeId`=`grade`.`GradeId`;
```



### 2.外连接查询 （LEFT JOIN）

* 左外连接：`表1 left [outer] join 表2 on 关联表达式 `
表1（左表）的记录与表2（右表）的记录逐条关联  满足关联条件保留  不满足也保留，关联不到的数据会记录为null

```mysql
SELECT `student`.`StudentNo`,`student`.`StudentName`,`StudentResult`
	FROM `student` LEFT JOIN `result` ON `student`.`StudentNo` = `result`.`StudentNo`
		WHERE `StudentResult` IS NULL;

SELECT `student`.`StudentNo`,`student`.`StudentName`,`StudentResult`,`ExamDate`
	FROM `student` LEFT JOIN `result` ON `student`.`StudentNo` = `result`.`StudentNo`
	INNER JOIN `grade` ON `student`.`GradeId`=`grade`.`GradeId`
		WHERE `GradeName` = 'S1';
		
SELECT `StudentNo`,`StudentName`,`grade`.`GradeId`,`GradeName`
	FROM `grade` LEFT JOIN `student` ON `student`.`GradeId` = `grade`.`GradeId`;
```

* 右外连接：`表1 right [outer] join 表2 on 关联表达式 `（基本不用，与左外连接查询相反，一般都用左外连接查询）
表2（右表）的记录与的表1（左表）记录逐条关联  满足关联条件保留  不满足也保留，关联不到的数据会记录为null

```mysql
SELECT `StudentNo`,`StudentName`,`grade`.`GradeId`,`GradeName`
	FROM `student` RIGHT JOIN `grade` ON `student`.`GradeId` = `grade`.`GradeId`;
```



### 3.自连接查询（递归查询）

```mysql
SELECT p.`categoryName` '父分类',c.`categoryName` '子分类'
	FROM `category` p INNER JOIN `category` c
	ON p.`categoryId` = c.`pId`;
```



## 三、子查询（Subquery）

子查询指的是在一个 SQL 查询里嵌套另一个查询。子查询能够置于 `SELECT`、`FROM`、`WHERE` 等子句中

* 优点：子查询能让复杂查询分步骤进行，逻辑更清晰，易于理解与维护
* 缺点：子查询可能需要多次执行，性能上不如连表查询，尤其是在处理大数据集时

```mysql
SELECT StudentNo,StudentName,Phone,Address FROM student WHERE gradeId IN (
	SELECT gradeId FROM grade WHERE gradeName='S1' OR gradeName='S2'
);

SELECT StudentNo,StudentName FROM student WHERE StudentNo IN (
	SELECT StudentNo FROM result WHERE StudentResult >= 80 AND SubjectId = (
		SELECT SubjectId FROM `subject` WHERE SubjectName = '使用C#语言开发数据库应用系统'
	)
);

SELECT `StudentNo`,`StudentName` FROM `student` WHERE `StudentNo` IN (
	SELECT DISTINCT `StudentNo` FROM `result` WHERE `StudentResult` >= 99
);

SELECT `student`.`StudentNo`,`StudentName`,`StudentResult` FROM `student`
	LEFT JOIN `result` ON `student`.`StudentNo` = `result`.`StudentNo`
	WHERE `SubjectId` = (
		SELECT `SubjectId` FROM `subject` WHERE `SubjectName` = '深入.NET平台和C#编程'
	)
ORDER BY `StudentResult` DESC
LIMIT 5;
```

