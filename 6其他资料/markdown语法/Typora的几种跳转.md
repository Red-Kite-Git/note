# Typora 的几种跳转

Typora中实现跳转的方式：	Ctrl + 鼠标单击链接

## 一、超链接跳转
```markdown
[描述内容](链接/路径 "悬停显示内容")
```

[百度](https://www.baidu.com/ "跳转到百度网页")

## 二、参考链接
```markdown
[展示文本][a(引用名)]
[a]:网址/本地路径
```
[百度][a]
[a]:https://www.baidu.com

## 三、本页本文件跳转

```markdown
[展示文本](# 一级标题文本)
[展示文本](## 二级标题文本)
```

[跳转到顶部一级标题](# Typora 的几种跳转)
[跳转到标题：参考链接](## 二、参考链接)

## 四、本地文件跳转

我们可以根据 「相对路径」或者 「绝对路径」 来完成 Markdown 文件跳转到另一个本地文件的作用。而这种链接跳转跟第一种「超链接跳转」是一样的。

// 跳到当前目录下的Readme1.md文件
[Readme1](Readme1.md)

// 跳到当前目录下的Readme3文件夹
[Readme3](Readme3)

// 相对路径跳到上级目录的某个文件
[Readme2](../Docs/Readme2.markdown)

// 绝对路径跳转到C盘下的某个md文件
[Readme4](C:/Develop/Docs/Readme1.md)
注意⚠️⚠️ ：对于相对链接地址，当基于 Markdown 的规范导出为 HTML 时，它不会转换为真正的绝对文件路径



原文链接：https://blog.csdn.net/qq_41907769/article/details/121722716
