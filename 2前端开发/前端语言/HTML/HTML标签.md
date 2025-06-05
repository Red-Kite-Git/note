# HTML标签

## 一、文本标签

注：text通用表示展示在页面上的内容

### 1.标题标签`<h>`
`<h1>`通常是最重要的标题，字体最大，`<h6>`字体最小
```html
<h1>text</h1> - <h6>text</h6>
```

### 2.段落标签`<p>`
将文本分成段落，在段落之间会有一些间距
```html
<p>text</p>
```

### 3.换行符`<br>`
```html
<br/>
```

### 4.水平线`<hr>`
```html
<hr/ >
```

### 5.加粗字体`<strong>`
```html
<strong>text</strong>
```

### 6.斜体`<em>`
```html
<em>text</em>
```


## 二、图像标签`<img>`
```html
<img [src="path"][alt="TXT"][title="TXT"][width="number px"][height="number px"]/>
```
> src 属性：指定图像的源文件路径
> alt 属性：当图像无法显示时或者用于辅助技术时TXT代替图片在页面显示
> title 属性：如果鼠标悬停在图片上 显示TXT内容
> width与height 属性：指定图片在页面展示的 宽与高 默认单位px


## 三、链接标签
超链接(外部链接)/锚链接(内部链接)标签
```html
<a [herf="path"][target="value"][name="name"]>展示内容</a>
```
> herf 属性：path为跳转的html页面地址或配合name属性使用"#name"（锚链接）
> name 属性：表示当前超链接的名字（标记）
> target 属性：让跳转的页面显示受value控制  _self新开页面显示（默认） _blank在本页显示_top_parent


## 四、列表标签
### 1.无序列表`<ul>`
列表项用`<li>`标签表示，并且会在每个列表项前面显示一个项目符号（如圆点）
```html
<ul>
  <li>TXT</li>
  ...
</ul>
```

### 2.有序列表`<ol>`
列表项用`<li>`标签，自动为列表项添加数字序号
```html
<ol>
  <li>TXT</li>
  ...
</ol>
```

### 3.自定义列表`<dl>`
列表项分级用`<dt>`与`<dd>`标签
```html
<dl>
	<dt>TXT</dt>
		<dd>TXT</dd>
		...
	...
</dl>
```


## 五、表格标签
* 表格`<table>`中`<tr>`与`<th>`是表格的一行	
  `<th>`是表头（一般第一行）默认粗体显示文本	`<tr>`是表单
* `<tr>`与`<tr>`标签内部又包含	 `<td>`是一行中的一个单元格
```html
<table [width="80%"][border="number"][align="center"]>
	<th>
        <td [colspan="number"]>TXT</td>
		...
    </th>
	<tr>
		<td [colspan="number"]>TXT</td>
		...
	</tr>
	...
</table>
```
> `width` 属性：指定图片在页面展示的 宽与高 默认单位px 比例单位%（占总页面的百分比）
> `border `属性：指定表格的边框粗度 默认为1px 默认单位px
> `align `属性：指定位置关系（页面与表格/表格与单元格/单元格与TXT）` center`水平中间
> `colspan`与`rowspan` 属性：指定单元格向左（水平跨行）/下（垂直跨列）合并的单元格数 默认为0



## 五、表单标签
### 1.form标签
用于创建表单，是用户输入数据的区域。它可以包含各种表单元素
```html
<form [action][method]></form>
```
> action 属性：指定表单数据提交的 URL
> method 属性：指定提交的方法  get/post
> 表单元素的提交方式
> * get方式: 提交的参数会在url中显示，传递的参数有长度限制，不安全
> * post方式: 提交的参数不会在url中显示，传递的参数没有长度限制，相对安全

### 2.input标签
最常用的表单元素之一
```html
<input [type][name][maxlength][size][value][disabled][readonly] />
```
> type 属性：多种类型的输入框
文本框text 密码框password 单选按钮radio 多选按钮checkbox 提交按钮submit 重置按钮resrt 隐藏域标签hidden
> name 属性：输入框的名字  /*在type="radio"/"checkbox"时，同一组的单选按钮name属性需要一致*/
> maxlength属性：输入内容时限制输入内容的最大长度
> size 属性：输入框本身展示的尺寸
> value 属性：输入框中待提交给服务器的值
> disabled 属性：禁用当前输入框（页面变灰）
> readonly 属性：锁定输入框（页面只能看不能输入）

### 3.textarea标签
用于创建多行文本输入区域，例如用于用户输入评论或长篇内容
```html
<textarea [value]></textarea>
```

### 4.select下拉框

* `<select>`用于创建下拉列表
* `<option>`定义下拉列表中的选项

```html
<select>
    <option selected>选项1</option>
	...
</select>
```

>selected属性：进入页面默认选择此选项



## 六、div标签与span标签

### 1.div标签

是一个块标签，里面可以放内容或继续放标签 使用div标签+css样式可以解决大部分布局
```html
<div>TXT</div>
```

### 2.span标签

`<span>`标签是一个内联元素，本身没有特殊的语义，用于对文本或其他内联元素进行分组，方便对这些元素应用样式或者通过脚本操作

```html
<span style="">TXT</span>
```

## 注释与特殊符号
### 1.注释
`<!-- 注释内容 -->`运行时不加载注释的内容  但是页面可以看到源码 开发时一般不使用

### 2.特殊符号
`&nbsp;` 空格     `&gt;` >    ` &lt;` <     `&quot;` 双引号     `&copy;` 商标符号