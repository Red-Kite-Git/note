#  Sprig、Spring MVC、MyBatis的集成框架

## 一、简介

SSM（Spring + Spring MVC + MyBatis）是一种经典的 Java 企业级应用开发架构，结合了三个开源框架的优势，适用于中大型 Web 应用尤其是对 SQL 性能要求较高的快速开发

### **1. 框架组成**

- **Spring**（核心容器）：
  - 提供依赖注入（DI）和面向切面编程（AOP），管理各层组件的生命周期。
  - 支持事务管理、资源整合（如数据库连接池、消息队列）等。

- **Spring MVC**（表现层框架）：

  - 基于 MVC 模式，处理 Web 请求与响应，实现前后端分离。
  - 通过注解（如`@Controller`、`@RequestMapping`）简化开发。

- **MyBatis**（持久层框架）：

  - 轻量级 ORM 工具，通过 XML 或注解映射 SQL 语句。

  - 支持动态 SQL、缓存机制，灵活性高。


### **2. 架构分层**

SSM 将应用分为**表现层**、**业务逻辑层**、**持久层**，各层职责明确：

- **表现层**（Spring MVC）：接收用户请求，调用业务层，返回视图或 JSON。

- **业务逻辑层**（Spring）：处理业务逻辑，调用持久层，控制事务。

- **持久层**（MyBatis）：操作数据库，封装数据访问逻辑。

### **4. 优势**

- **解耦性**：各层独立，方便维护和扩展。

- **灵活性**：MyBatis 支持自定义 SQL，适合复杂查询。

- **性能优化**：MyBatis 的延迟加载和缓存机制提升数据库访问效率。

- **社区支持**：成熟框架，文档丰富，问题易排查。

## 二、搭建开发环境

### 1.IDEA创建项目（JAVA项目）

lib目标加入Jar包添加为库，创建实体entity包、接口与映射文件mapper包、测试test包等等

### 2.配置文件

​		**Spring配置文件applicationContext.xml**		**Mybatis配置文件mybatis-config.xml**		

​		**SpringMVC配置文件**		**日志配置文件 log4j.properties**

>1. 命名上均为习惯，文件名非强制固定
>2. 核心配置文件应放在resources根目录下
>3. Spring配置文件
>4. Mybatis配置文件
>5. 日志配置文件
>6. 核心配置文件主要用于配置连接数据库的环境以及MyBatis的全局配置信息

### 3.创建接口与映射文件

```java
public interface 接口（文件）名{
    返回类型 方法名;
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="对应的Mapper接口的相对根目录路径">
    <>
</mapper>
```

## 三、核心配置文件详解

### 1.mybatis-config.xml	↓

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

    <!-- 引入 database.properties 文件 -->
    <properties resource="database.properties"/>
    
    <settings>
        <!-- 配置mybatis的log实现为LOG4J -->
        <setting name="logImpl" value="LOG4J"/>
        <!-- 配置自动映射级别 NONE禁止自动映射 PARTIAL自动映射没有嵌套的结果集 FULL自动映射任意复杂的结果集 -->
        <setting name="autoMappingBehavior" value="FULL"/>
    </settings>

    <!-- 定义别名 -->
    <typeAliases>
        <!-- <typeAlias alias="SmbmsUser" type="cn.kgc.entity.SmbmsUser"/> -->
        <package name="cn.kgc.entity"/>
    </typeAliases>

    <!-- 运行环境 -->
    <environments default="development">
        <environment id="development">
            <!--配置事务管理，采用JDBC的事务管理  -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- POOLED:mybatis自带的数据源，JNDI:基于tomcat的数据源 -->
            <dataSource type="POOLED">
                <!-- driver:数据库驱动-->
                <!-- ${driver} 从database.properties读取driver属性的值 -->
                <property name="driver" value="${driver}"/>
                <!--url:数据库连接地址-->
                <property name="url" value="${url}"/>
                <!--username:数据库账号-->
                <property name="username" value="${user}"/>
                <!--password:数据库密码-->
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 映射文件 将mapper文件加入到配置文件中 -->
    <mappers>
        <!-- <mapper resource="cn/kgc/mapper/SmbmsUserMapper.xml"/> -->
        <!-- <mapper url="file:E:\CodeLearn\JAVA\SSMframework\Mybatis\src\cn\kgc\mapper\SmbmsUserMapper.xml"/> -->
        <package name="cn.kgc.mapper"/>
    </mappers>
    
</configuration>
```

### 2.database.properties	↓

```properties
driver=com.mysql.jdbc.Driver
#在向 MySQL 传递数据的过程中，使用 Unicode 编码格式，并将字符集设置为 UTF-8
url=jdbc:mysql://127.0.0.1:3306/smbms?useUnicode=true&characterEncoding=utf-8
user=root
password=
```

### 3.log4j.properties	↓

```properties
#日志级别 FATAL(致命)>ERROR(错误)>WARN(警告)>INFO(信息)>DEBUG(调试)
log4j.rootLogger=DEBUG,CONSOLE
log4j.logger.cn.smbms.dao=debug
log4j.logger.com.ibatis=debug
log4j.logger.com.ibatis.common.jdbc.SimpleDataSource=debug
log4j.logger.com.ibatis.common.jdbc.ScriptRunner=debug
log4j.logger.com.ibatis.sqlmap.engine.impl.SqlMapClientDelegate=debug
log4j.logger.java.sql.Connection=debug
log4j.logger.java.sql.Statement=debug
log4j.logger.java.sql.PreparedStatement=debug
log4j.logger.java.sql.ResultSet=debug
log4j.logger.org.tuckey.web.filters.urlrewrite.UrlRewriteFilter=debug
######################################################################################
# Console Appender  \u65e5\u5fd7\u5728\u63a7\u5236\u8f93\u51fa\u914d\u7f6e
######################################################################################
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.Threshold=error
log4j.appender.CONSOLE.Target=System.out
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=[%p] %d %c - %m%n
######################################################################################
# DailyRolling File  \u6bcf\u5929\u4ea7\u751f\u4e00\u4e2a\u65e5\u5fd7\u6587\u4ef6\uff0c\u6587\u4ef6\u540d\u683c\u5f0f:log2009-09-11
######################################################################################
log4j.appender.file=org.apache.log4j.DailyRollingFileAppender
log4j.appender.file.DatePattern=yyyy-MM-dd
log4j.appender.file.File=log.log
log4j.appender.file.Append=true
log4j.appender.file.Threshold=error
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-M-d HH:mm:ss}%x[%5p](%F:%L) %m%n
log4j.logger.com.opensymphony.xwork2=error

```



## 四、



## 五、



## 六、



## 七、



## 八、
