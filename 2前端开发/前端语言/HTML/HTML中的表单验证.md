# HTML表单验证

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<!--
			 pattern正则表达式作用: 验证用户输入的内容是否符合定义的规则。如果不符合格则,则阻止用户提交。
			 (1)  输入的内容必须是数字,长度为11位。  \d表示0-9之间的数字 。 {11} 表示出现的次数。       \d{11}
			 (2)  输入的内容必须是数字,长度在4-12位之间。  \d{4,12} 
			 (3)  密码可以由数字 字母 下划线  $ 符号组成 长度4到12位之间。
						\w 表示字母 
						[a-zA-Z0-9] 表示可以由字母和数字组成
						[a-zA-Z0-9_$]{4,12}  
			(4)  密码必须以字母开始,可以由数字 字母 下划线  $ 符号组成,长度是4位及以上
			            [a-zA-Z][a-zA-Z_$]{4,}
		-->
		<form method="post">
			<p><input type="text" name="username" placeholder="提示:请输入用户名称" required pattern="[A-z]{8,12}"></p>
			<p><input type="password" name="password" placeholder="提示:请输入密码" required pattern="\d{11}"></p>
		<!-- 三种提交方式 -->
		<button>提交</button>
		<input type="submit" value="提交">
		<input type="button" value="提交">
		</form>
	</body>
</html>
```

