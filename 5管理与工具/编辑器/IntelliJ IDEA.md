# IntelliJ IDEA

## 一、简介

[IntelliJ IDEA官网](https://www.jetbrains.com/)



## 二、快捷键

### 缩写

psvm/main       --->    public static void main(String [] args){}
"str".sout      --->    System.out.println("str");
"str".serr      --->    System.err.println("str");
fori            --->    for (int i = 0;i< ;i++) {}
ifn             --->    if (i == null) {}
变量值.var       --->    对应变量类型 变量名 = 变量值
Object.for       --->    for (Object o : objects) {}

### 通用快捷键

alt+ ←/→                	切换当前打开的窗口
alt+ ↑/↓                 切换到上一个/下一个方法
ctrl+ ←/→               控制光标切换到上一个/下一个单词
ctrl+shift+ ←/→         选中光标前/后的一个单词或字符串

ctrl+d                  复制光标处一行的代码至下一行/复制选中的几行至下几行
ctrl+shift+v            从历史复制库中粘贴

ctrl+enter/shift+enter  在光标下一行创建一行空行
ctrl+alt+enter          在光标上一行创建一行空行

ctrl+"/"                选中目标行，生成/取消生成单行注释
ctrl+shift+"/"          选中目标行，生成/取消生成多行注释

ctrl+shift+"L"          代码格式自动修正
ctrl+alt+"O"            优化import语句

alt+mouse_left竖向滑行	多个竖向光标
alt+shift+mouse_left     多个光标

ctrl+alt+"T"            包围代码块
alt+insert              生成...
ctrl+"O"                方法重写
ctrl+alt+"M"            方法抽取
ctrl+shift+"O"          （自定义设置，打开...）



## 三、插件

### 通用

#### Chinese (Simplified) Language Pack / 中文语言包

编辑器界面中文语言包

#### Translation

ctrl+shift+"O"            打开翻译窗口（//已移除，替换为通用：打开...）
ctrl+shift+"Y"            翻译
ctrl+shift+"X"            翻译并替换

#### CodeGeeX: Al Coding Assistant

alt+shift+"V"            生成注释
alt+shift+"E"            解释代码
alt+shift+"R"            代码重构



### GenerateAllSetter

一键调用一个对象的所有的set方法,get方法等

在方法上生成两个对象的转换



### Apifox Helper

Apifox Helper 是 Apifox 团队针对 IntelliJ IDEA 所推出的插件，可以在 IDE 中识别本地 Java、Kotlin 后端项目的源代码，直接在 IDE 侧边栏调试接口，自动生成 API 文档并同步到 Apifox 的项目中。
对于常见的开发框架，Apifox Helper 插件能够做到开箱即用，实现真正的代码零侵入。仅通过识别最基本的业务代码，即可生成一份详尽的 API 文档。



### EasyCode

* 基于IntelliJ IDEA开发的代码生成插件，支持自定义任意模板（Java，html，js，xml）
* 只要是与数据库相关的代码都可以通过自定义模板来生成。支持数据库类型与java类型映射关系配置
* 支持同时生成生成多张表的代码。每张表有独立的配置信息。完全的个性化定义，规则由你设置

**文件  >  设置  >  其他设置  >  EasyCode**

导入模板  [EasyCodeConfig.json](..\..\resources\file\EasyCodeConfig.json)



### Alibaba Java Coding Guidelines(Fix Some Bug)

阿里规约



### MyBatisX

* Mapper 和 XML 可以来回跳转
* mybatis.xml，mapper.xml 提示
* mapper 和 xml 支持像 jpa 一样的自动提示符（参考 MybatisCodeHelperPro）
* 集成 mybatis 生成器 Gui （从免费的 mybatis 插件复制）