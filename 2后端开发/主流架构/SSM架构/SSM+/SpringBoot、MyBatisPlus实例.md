# SpringBoot+MybatisPlus实例（Maven项目）

## 一、创建Maven项目

### 1.环境

### 2.配置文件

Maven配置文件pom.xml		SpringBoot配置	application.yml

## 二.配置文件详解

▼ Maven配置文件	pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- 项目的父级依赖（spring-boot-starter-parent） -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.12</version>
        <relativePath/>
    </parent>

    <!-- 项目位置 -->
    <groupId>cn.kgc</groupId>
    <artifactId>SpringBoot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>SpringBoot</name>
    <description>SpringBoot</description>
    
    <!-- 打包方式 -->
    <packaging>jar</packaging>

    <!-- 属性信息 -->
    <properties>
        <java.version>11</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.6.13</spring-boot.version>
    </properties>

    <dependencies>
        <!-- 添加SpringBoot依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 添加mybatis依赖 -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.3.1</version>
        </dependency>
        <!-- 添加mybatisPlus依赖 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.3.2</version>
        </dependency>
        <!-- 添加mySql依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.49</version>
        </dependency>
        <!-- 添加druid（数据源）依赖 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.6</version>
        </dependency>
        <!-- 添加jdbc依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!-- 添加lombok依赖 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.24</version>
        </dependency>
        <!-- 添加swagger依赖 -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.6.1</version>
        </dependency>
        <!-- 添加swagger-ui依赖 -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.6.1</version>
        </dependency>
        <!-- 添加swagger-bootstrap-ui依赖 -->
        <dependency>
            <groupId>com.github.xiaoymin</groupId>
            <artifactId>swagger-bootstrap-ui</artifactId>
            <version>1.9.1</version>
        </dependency>


        <!-- 添加hutool依赖 -->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.8.28</version>
        </dependency>
    </dependencies>

    <!-- 依赖管理：约束依赖的版本 但不会引入依赖 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

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
                <configuration>
                    <!-- 指定主类（程序入口） -->
                    <mainClass>cn.kgc.Application</mainClass>
                    <!-- 是否跳过该插件 -->
                    <skip>false</skip>
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

▼ SpringBoot配置	application.yml

```yaml
# 服务器配置
server:
  # 服务器端口
  port: 80

# Spring Boot 配置
spring:
  banner:
    location: classpath:banner/banner.txt
  # 应用程序配置
  application:
    # 应用程序名称
    name: SpringBoot
  # 配置文件
  profiles:
    # 激活的配置文件
    active: dev
  mvc:
    path match:
      matching-strategy: ant_path_matcher
  #设置时区
  jackson:
    time-zone: GMT+8

# MyBatis配置
mybatis:
  # Mapper文件的位置
  mapper-locations: classpath:mapper/*.xml
  # 实体类的包名
  type-aliases-package: cn.kgc.entity
  # 配置
  configuration:
    # 是否开启下划线转驼峰命名
    map-underscore-to-camel-case: true
    # 自动映射行为
    auto-mapping-behavior: full
    # 配置日志实现类
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

# 配置日志级别
logging:
  # 配置日志级别
  level:
    # 设置cn.kgc.controller包的日志级别为debug
    cn.kgc.controller: debug
```

## 三、创建类

[EasyCode插件](..\..\..\..\6管理与工具\IDEA插件\EasyCode.md) 生成entity、mapper、service、controller

## 四、添加注解



## 五、添加注解



## 六、统一结果封装

接口的返回值类型不同、运行时异常等，导致前端获取的数据不统一难以处理

