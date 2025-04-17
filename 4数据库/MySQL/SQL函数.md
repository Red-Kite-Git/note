# MySQL函数

```mysql
USE myschool;
```

## 一、不常用的函数

数学函数、字符串函数、日期时间函数、系统信息函数




## 二、统计函数

### 1.count() 记录总数
```mysql
SELECT COUNT(*) FROM student;

SELECT COUNT(StudentNo) FROM student WHERE sex = '男';

SELECT COUNT(StudentNo) FROM student WHERE GradeId IN (
	SELECT `GradeId` FROM grade WHERE GradeName = 'S1' 
);

SELECT COUNT(StudentNo) '深入.NET平台和C#编程 课程的考试不及格的学生数量' FROM result WHERE StudentResult < 60 AND SubjectId = (
	SELECT SubjectId FROM `subject` WHERE SubjectName = '深入.NET平台和C#编程'
);

SELECT COUNT(SubjectId) 'S2年级的课程数量' FROM `subject` WHERE GradeId = (
	SELECT GradeId FROM grade WHERE GradeName = 'S2'
);
```

###			2.sum() 求和

```mysql

```

###			3.avg() 平均值
```mysql
SELECT AVG(`StudentResult`) FROM result WHERE subjectId = (
	SELECT subjectId FROM `subject` WHERE subjectName = '深入.NET平台和C#编程'
);
SELECT studentNo,studentName FROM student WHERE studentNo IN (
SELECT studentNo FROM result WHERE StudentResult > (
	SELECT AVG(`StudentResult`) FROM result WHERE subjectId = (
		SELECT subjectId FROM `subject` WHERE subjectName = '深入.NET平台和C#编程'
		)
	) AND subjectId = (
		SELECT subjectId FROM `subject` WHERE subjectName = '深入.NET平台和C#编程'
		)
);
```


###			4.max() 最大值
```mysql
SELECT StudentNo,StudentName,Phone,BornDate FROM student WHERE BornDate = (
	SELECT MAX(BornDate) FROM student
);
SELECT `StudentNo`,`StudentName` FROM student WHERE `StudentNo` IN (
	SELECT `StudentNo`FROM result WHERE `ExamDate` = (
		SELECT MAX(`ExamDate`) FROM result WHERE subjectId = (
			SELECT `SubjectId` FROM `subject` WHERE `SubjectName` = '深入.NET平台和C#编程'
		) # 结果是一个时间 可能有其他考试，所有需要在上一层再次筛选
	) AND subjectId = (
		SELECT `SubjectId` FROM `subject` WHERE `SubjectName` = '深入.NET平台和C#编程'
	) # 最大考试时间 再加上 指定课程编号
);
SELECT `StudentNo`,`StudentName` FROM student WHERE `StudentNo` IN (
	SELECT `StudentNo`FROM result WHERE StudentResult = (
		SELECT MAX(`StudentResult`) FROM result WHERE subjectId = (
			SELECT `SubjectId` FROM `subject` WHERE `SubjectName` = '深入.NET平台和C#编程'
		)
	) AND `SubjectId` = (
		SELECT `SubjectId` FROM `subject` WHERE `SubjectName` = '深入.NET平台和C#编程'
	)
);
```


###			5.min() 最小值
```mysql
SELECT `StudentNo`,`StudentName` FROM student WHERE `StudentNo` IN (
	SELECT `StudentNo` FROM result WHERE StudentResult = (
		SELECT MIN(`StudentResult`) FROM result 
	)
);

SELECT `StudentNo`,`StudentName` FROM student WHERE `StudentNo` IN (
	SELECT `StudentNo`FROM result WHERE StudentResult = (
		SELECT MIN(`StudentResult`) FROM result WHERE subjectId = (
			SELECT `SubjectId` FROM `subject` WHERE `SubjectName` = '深入.NET平台和C#编程'
		)
	) AND `SubjectId` = (
		SELECT `SubjectId` FROM `subject` WHERE `SubjectName` = '深入.NET平台和C#编程'
	)
);
```



