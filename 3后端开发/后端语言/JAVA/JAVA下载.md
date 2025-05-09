一 Java体系结构
JavaSE:  Java基础
JavaEE:  企业级开发
JavaME:  移动开发   淘汰(×)


二  JDK  JRE  JVM
JDK: java开发工具包
JRE: java运行环境
JVM: java虚拟机 （虚拟机可以虚拟出一台小型计算机）
安装JDK时，在JDK中包含了JRE，在JRE包含了JVM。

JDK
│
├── JRE
│   ├── JVM
│   ├── 核心类库
│   └── 其他组件
│
└── 开发工具
    ├── javac       编译器，用于将 Java 源文件编译为字节码
    ├── jdb         调试器，用于调试 Java 程序
    ├── java        程序启动器，用于运行编译后的 Java 程序
    ├── javadoc     文档生成器，用于根据源代码中的注释生成 API 文档
    └── jar         打包工具,用于创建、管理和解压缩 JAR 文件（Java ARchive），这些文件通常包含多个类和关联的资源


三  JDK下载安装配置
1 下载JDK并安装。

2 配置环境变量
（1）电脑  --> 右键  --> 属性 --> 高级系统设置 -->高级  --> 环境变量
（2）新建环境变量
JAVA_HOME
C:\Program Files\Java\jdk-11
（3）修改环境变量
PATH
在原有的值的后面追加以下内容:
%JAVA_HOME%\bin
上面的配置等价于  C:\Program Files\Java\jdk-11\bin

为什么要配置bin文件夹,bin文件夹下有java的执行命令。配置环境变量,才能够在任意文件夹
下使用java的命令。

3 测试环境变量
按windows键 --> 输入cmd  -->打开DOS窗口 输入  --> java -version





## 程序运行原理；

1.java程序的执行过程
编写java源文件 -->  由编译器编译为class文件  -->  javac运行class文件

2.java跨平台的原理
JDK中包含JRE运行环境，JRE中又包含JVM，只需要在不同平台下载对应的JVM 就可以跨平台运行class文件