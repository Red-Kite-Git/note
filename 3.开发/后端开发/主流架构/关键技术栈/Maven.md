# Maven

## 一、介绍

Maven 是 Apache 软件基金会组织维护的一款**自动化构建工具**，专注服务于 Java 平台的项目构建和 依赖管理

## 二、核心概念-POM

pom.xml配置文件详解

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

<!-- 项目的父级依赖 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.6.12</version>
    <relativePath/>
</parent>

<!-- 项目打包安装到本地仓库的路径（项目位置） -->
<groupId>项目坐标（执行Maven命令时打包的坐标位置）【cn.kgc】</groupId>
<artifactId>分模块开发时的模块id【SpringBoot】</artifactId>
<version>版本号【1.0-SNAPSHOT】</version>
<packaging>打包方式【jar】</packaging>

<name>Maven_SpringBoot</name>
<url>https://maven.apache.org</url>

<!-- 属性信息 -->
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>

<!-- 引入项目依赖 -->
<dependencies>
    <!--
    示例：添加hutool依赖（指定在仓库中的坐标）
    如果仓库中没有依赖，按照setting.xml中配置的优先级：本地仓库>私服仓库>中央仓库（公共仓库）自动下载
    -->
    <dependency>
        <groupId>cn.hutool</groupId>
        <artifactId>hutool-all</artifactId>
        <version>5.8.28</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<!-- 项目构建 -->
<build>
    <!-- 项目插件 -->
    <plugins>
        <!-- 编译插件 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>11</source>
                <target>11</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
        <!-- 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>${spring-boot.version}</version>
            <!-- 配置 -->
            <configuration>
                <!-- 主类 -->
                <mainClass>cn.kgc.Application</mainClass>
                <!-- 跳过 -->
                <skip>true</skip>
            </configuration>
            <executions>
                <execution>
                    <id>repackage</id>
                    <goals>
                        <goal>repackage</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

</project>
```



## 三、Maven生命周期命令

### 1. Clean 生命周期
* **clean**：**清除**项目之前构建产生的文件（ `target`目录）

### 2. Default 生命周期
* validate：对项目进行基本的验证，检查项目的配置是否正确，像必要的配置信息是否齐全、依赖是否可解析等。若验证不通过，Maven 会终止构建过程
* compile：编译项目的源代码，通常是`src/main/java`目录下的 Java 文件，编译结果会存于`target/classes`目录
* test：运用单元测试框架（例如 JUnit）来测试编译后的代码。测试代码一般在`src/test/java`目录，测试结果会显示出来，若有测试失败，Maven 默认会终止构建
* **package**：把编译后的代码按照pom.xml文件中指定的方式**打包**成可分发的格式，常见的有 JAR（Java Archive）、WAR（Web Application Archive）等。打包后的文件会存于`target`目录
* verify：对集成测试的结果进行验证，保证包是有效的且符合质量要求。比如，它可能会检查代码覆盖率、静态代码分析结果等
* **install**：此阶段会把**打包**好的文件（如 JAR、WAR）**安装**到本地的 Maven 仓库。这样一来，本地其他项目就能够引用这个项目作为依赖。
* deploy：把打包好的文件发布到远程的 Maven 仓库，供团队内其他开发者或者外部用户使用

### 3.Site 生命周期
* **site** ：生成项目的站点文档，涵盖项目的概述、API 文档、测试报告等。生成的站点文档一般存于`target/site`目录



## 三、IDEA集成Maven项目

IDEA关闭所有项目的主界面配置设置

文件  >  设置  >  构建、执行、部署  >  构建工具  >  Maven



