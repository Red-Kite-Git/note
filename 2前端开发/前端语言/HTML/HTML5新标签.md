# HTML5新标签

## 一、HTML5新增内容

### 1.新增标签

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<input list="cities"/>
		<datalist id="cities">
			<option value="北京">
			<option value="上海">
			<option value="广州">
			<option value="深圳">
		</datalist>
		
		
			<!-- 有些浏览器目前不支持 -->
			<p>我在<time datetime="2025-02-14">情人节</time>有个约会。</p>
			<p>我在<input type="date" value="2025-02-14">情人节</time>有个约会。</p>
			<p>我是一段文字，请看<mark>我的标记</mark>部分。</p>
			
		<div>
			偏爱
			<audio src="audio/偏爱.mp3" controls>
				您的浏览器不支持audio标签
			</audio>
		</div><hr />
		
		<div>
			<video src="video/movie.mp4" controls>
				您的浏览器不支持video标签
			</video>
			<video src="video/vedio.mpg" controls>
				您的浏览器不支持video标签
			</video>
		</div><hr />
		
		<canvas id="myCanvas" width="400px" height="400px"></canvas>
		<script>
			// script标签内写JS代码（javascript）
			//找到页面上id为myCanvas的元素
			let myCanvas= document.getElementByld("myCanvas");
			//绘制图形
			var ctx = myCanvas.getContext("2d");
			//设置颜色
			ctx.fillStyle = "red";
			//绘制矩形
			ctx.fillRect(0,0,100,100);
		</script>

	</body>
</html>
```

### 2.新增全局属性

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div class="container">
			<p contenteditable="true">这是一个可编辑的字段</p>
			<!-- 可编辑的 拼写检查 -->
			<p contenteditable="true" spellcheck="true">这是可编辑的段落。请试着编辑文本。</p>
			<ul>
				<!-- tab键选中的次序 -->
				<li tabindex="2">苹果</li>
				<li tabindex="1">橘子</li>
				<li tabindex="3">香蕉</li>
			</ul>
			<!-- 隐藏的元素 -->
			<p hidden>隐藏的文字</p>
		</div>
	</body>
</html>
```

### 3.input标签type属性新增值

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<form>
			<!-- required表示必填 -->
		请输入邮箱：<input type="email" required>
		<button>提交</button>
		</form><hr />
		
		<form>
		请输入网址：<input type="url" required>
		<button>提交</button>
		</form><hr />
		
		<form>
		请选择颜色：<input type="color" required>
		<button>提交</button>
		</form><hr />
		
		<form>
		请输入查询：<input type="search" required>
		<button>提交</button>
		</form><hr />
		
		<form>
		请输入年龄：<input type="number" value="18" min="18" max="100" step="1" required>
		<button>提交</button>
		</form><hr />
		
		<form>
		请选择身高：<input type="range" value="180" min="100" max="200" step="1" required>
		<button>提交</button>
		</form><hr />
		
		<form>
		请选择日期：<input type="date" required><input type="time" required>
		<button>提交</button>
		</form><hr />
	</body>
</html>
```



## 二、页面布局对比

### HTML4中的页面布局 ▼
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			*{
				margin: 0px;
				padding: 0px;
			}
			.box{
				width: 90%;
				height: 1024px;
				border: 1px solid black;
			}
			.header{
				width: 100%;
				height: 100px;
				background-color: pink;
				text-align: center;
				line-height: 100px;
			}
			.nav{
				background-color: greenyellow;
			}
			.aside{
				width: 25%;
				height: 500px;
				background-color: lightblue;
				border: 1px solid lightblue;
				float: left;
			}
			.main{
				height: 500px;
				background-color: yellow;
			}
			.footer{
				width: 100%;
				height: 100px;
				background-color: pink;
				text-align: center;
				line-height: 100px;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="header">页面头部</div>
			<div class="container">
				<div class="nav">导航</div>
				<div class="aside">侧边栏</div>
				<div class="main">正文</div>
			</div>
			<div class="footer">页面底部</div>
		</div>
	</body>
</html>
```

### HTML4中的页面布局 ▼

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			.container{
				width: 99%;
				height: 900px;
				border: 1px solid black;
				margin: 0 auto;
			}
			.header{
				height: 80px;
				line-height: 80px;
				background-color: pink;
				text-align: center;
			}
			.nav{
				width: auto;
				height: 30px;
				background-color: aqua;
			}
			.aside{
				width: 20%;
				height: 400px;
				background-color: lightblue;
				float: left;
			}
			.article{
				height: 400px;
				background-color: yellow;
			}
			.footer{
				height: 80px;
				line-height: 80px;
				background-color: pink;
				text-align: center;
			}
		</style>
	</head>
	<body>
		<div class="container">
			<header class="header">页面头部</header>
			<main class="main">
				<nav class="nav">页面导航条</nav>
				<div class="content">
					<aside class="aside">页面侧边栏</aside>
					<article class="article">页面主体内容</article>
				</div>
			</main>
			<footer class="footer">页面底部</footer>
		</div>
	</body>
</html>
```

