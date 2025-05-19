#  MyBatis 

## 一、MyBatis简介

### 1.官网
MyBatis（官网：https://mybatis-flex.com/ ）是一款优秀的 **持久层框架**，用于简化**JDBC**的开发。

### 2.MyBatis特性
1.  MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架
2.  MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集
3. MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录
4. MyBatis 是一个 半自动的ORM（Object Relation Mapping）框架

### 3.持久层框架对比
|       框架       |                             特点                             |
| :--------------: | :----------------------------------------------------------: |
|       JDBC       | SQL夹杂在Java代码中耦合度高，导致硬编码内伤。维护不易且实际开发需求中 SQL 有变化，频繁修改的情况多见 代码冗长，开发效率低 |
| Hibernate 和 JPA | 操作简便，开发效率高程序中的长难复杂 SQL 需要绕过框架 内部自动生产的 SQL，不容易做特殊优化基于全映射的全自动框架，大量字段的 POJO 进行部分映射时比较困难。反射操作太多，导致数据库性能下降 |
|     MyBatis      | 轻量级，性能出色SQL 和 Java 编码分开，功能边界清晰。Java代码专注业务、SQL语句专注数据开发效率稍逊于HIbernate，但是完全能够接受 |



## 二、搭建MyBatis开发环境

### 1.开发环境

IDEA、MySQL

> MySQL不同版本的注意事项
> 1、驱动类driver-class-name
>     MySQL 5版本使用jdbc5驱动，驱动类使用：com.mysql.jdbc.Driver
>     MySQL 8版本使用jdbc8驱动，驱动类使用：com.mysql.cj.jdbc.Driver
> 2、连接地址url
>     MySQL 5版本的url：
>         jdbc:mysql://localhost:3306/ssm
>     MySQL 8版本的url：
>         jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC

### 2.创建项目（JAVAWeb项目）

加入Jar包

### 3.创建MyBatis的配置文件

​		**核心配置文件** mybatis-config.xml		**数据库配置文件** database.properties		**日志配置文件** log4j.properties
>1. 命名上均为习惯，文件名非强制固定
>2. 核心配置文件主要用于配置连接数据库的环境以及MyBatis的全局配置信息
>3. 核心配置文件应放在resources根目录下

### 4. 创建接口与映射文件

在Mapper接口中定义方法，Mapper.xml映射文件中写标签
相关概念：**ORM**（Object Relationship Mapping）对象关系映射
**对象**：Java的实体类对象
**关系**：关系型数据库
**映射**：二者之间的对应关系

|   JAVA    | 数据库  |
| :-------: | :-----: |
|    类     |   表    |
|   属性    | 字段/列 |
| 对象/实例 | 记录/行 |

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

>Tip：
>
>1. 映射文件的命名：
>
>     用表所对应的实体类的类名+Mapper.xml，一个映射文件对应一个实体类，对应一张表的操作
>
>2. MyBatis中可以面向接口操作数据，要保证两个一致：
>     mapper接口的全类名和映射文件的命名空间（namespace）保持一致
>     mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性保持一致

### 5. 测试环境

```java
//读取MyBatis的核心配置文件
InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
//创建SqlSessionFactoryBuilder对象
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new
SqlSessionFactoryBuilder();
//通过核心配置文件所对应的字节输入流创建工厂类SqlSessionFactory，生产SqlSession对象
SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
//创建SqlSession对象，此时通过SqlSession对象所操作的sql都必须手动提交或回滚事务
//SqlSession sqlSession = sqlSessionFactory.openSession();
//创建SqlSession对象，此时通过SqlSession对象所操作的sql都会自动提交
SqlSession sqlSession = sqlSessionFactory.openSession(true);
//通过代理模式创建UserMapper接口的代理实现类对象
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
//调用UserMapper接口中的方法，就可以根据UserMapper的全类名匹配元素文件，通过调用的方法名匹配
映射文件中的SQL标签，并执行标签中的SQL语句
int result = userMapper.insertUser();
//sqlSession.commit();
System.out.println("结果："+result);
```



## 三、核心配置文件详解

### 1.mybatis-config.xml	▼

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

### 2.database.properties	▼

```properties
driver=com.mysql.jdbc.Driver
#在向 MySQL 传递数据的过程中，使用 Unicode 编码格式，并将字符集设置为 UTF-8
url=jdbc:mysql://127.0.0.1:3306/smbms?useUnicode=true&characterEncoding=utf-8
user=root
password=
```

### 3.log4j.properties	▼

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



## 四、MyBatis获取参数值的方式

MyBatis获取参数值的两种方式：字符串拼接\${}和占位符赋值\#{}
${}使用字符串拼接的方式拼接sql，为字符串类型或日期类型的字段进行赋值时，需要手动加单引号
\#{}使用占位符赋值的方式拼接sql，为字符串类型或日期类型的字段进行赋值时，可以自动加单引号

### 1. 单个参数

  1. ##### 使用 `@Param()` 注解
Mapper接口形参处添加 `@Param("参数名")` 注解，标识mapper接口中的方法参数
Mapper.xml映射文件通过 `${参数名}` 或 `#{参数名}` 访问就可以获取相对应的值

### 2. 多个参数

  1. ##### 使用多个 `@Param` 注解
Mapper接口形参处添加多个 `@Param("参数名1")`  `@Param("参数名2")` 注解，标识mapper接口中的方法参数
Mapper.xml映射文件通过 `${参数名}` 或 `#{参数名}` 访问就可以获取相对应的值

  2. ##### 使用map集合
Mapper接口形参处传入一个Map集合，Map中可以存入参数 `{ "Map的键" , "Map的值" }`
Mapper.xml映射文件通过 `${Map的键}` 或 `#{Map的键}` 访问就可以获取对应的Map的值

  3. ##### 实体类
形参处传入一个Entity实体，Entity类中定义属性 `属性类型 属性名 = 属性值`
Mapper.xml映射文件通过 `${属性名}` 或 `#{属性名}` 访问就可以获取对应的属性值



## 五、MyBatis的增删改查

创建好Mapper接口和Mapper映射文件后，在Mapper映射文件的 `<mapper> </mapper>` 标签内写SQL语句

### 1. 新增
> **insert标签属性**
>  id：接口中的方法
> parameterType：方法形参类型（可省）

```XML
<insert id="方法名" parameterType="参数类型">
	SQL新增语句
</insert>
```



### 2. 删除

> **select标签属性**
> id：接口中的方法
> parameterType：方法形参类型

```XML
<delete id="方法名" parameterType="参数类型">
	SQL删除语句
</delete>
```



### 3. 修改/更新

> **update标签属性**
> id：接口中的方法
> parameterType：方法形参类型

```XML
<update id="方法名" parameterType="参数类型">
	SQL更新语句
</update>
```



### 4. 查询

> **select标签属性**
> id：接口中的方法
> resultType：查询返回的结果集对应类型（实体类Bean 集合中泛型）
> resultMap：查询返回的结果集对应类型（实体类Bean 集合中泛型）
> parameterType：方法形参类型

```XML
<select id="方法名" resultType="JavaBean" parameterType="参数类型">
	SQL查询语句
</select>
```

> Tip：select查询标签必须设置属性resultType或resultMap至少之一，用于设置实体类和数据库表的映射关系
>     resultType：自动映射，用于属性名和表中字段名一致的情况
>     resultMap：自定义映射，用于一对多或多对一或字段名和属性名不一致的情况（在配置文件中可以设置映射级别）



## 六、自定义映射resultMap

> **resultMap属性**
> id：表示自定义映射的唯一标识
> type：查询的数据要映射的实体类的类型
>
> **子标签及属性**
> id：设置主键的映射关系
> result：设置普通字段的映射关系
> association：设置多对一的映射关系
> collection：设置一对多的映射关系属性：
> property：设置映射关系中实体类中的属性名
> column：设置映射关系中表中的字段名
> ofType：设置映射关系中实体类中集合的泛型



### 1.字段属性不一致

若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射


```xml
<resultMap id="标识名" type="与结果集对应的Bean实体类">
    <id column="主键字段名" property="属性名"/>
    <result column="非主键字段名1" property="属性名1"/>
    <result column="非主键字段名2" property="属性名2"/>
    <.../>
</resultMap>
<select id="方法名" resultMap="标识名">
	SQL查询语句
</select>
```

> Tip:
> 若字段名和实体类中的属性名不一致，但是字段名符合数据库的规则（使用_），实体类中的属性名符合Java的规则（使用驼峰）此时也可通过以下两种方式处理字段名和实体类中的属性的映射关系
> * 可以通过为字段起别名的方式，保证和实体类中的属性名保持一致
> * b>可以在MyBatis的核心配置文件中设置一个全局配置信息mapUnderscoreToCamelCase，可以在查询表中数据时，自动将_类型的字段名转换为驼峰



### 2.多对一

若多个字段名与实体类中的属性为多个字段对应一个属性时

* #### 级联方式处理映射关系

```xml
<resultMap id="标识名" type="与结果集对应的Bean实体类">
    <id column="主键字段名" property="属性名"/>
    <result column="非主键字段名1" property="类名.属性名1"/>
    <result column="非主键字段名2" property="类名.属性名2"/>
    <.../>
</resultMap>
<select id="方法名" resultMap="标识名">
	SQL查询语句
</select>
```

* #### 使用association处理映射关系

```xml
<resultMap id="标识名" type="与结果集对应的Bean实体类">
	<id column="主键字段名" property="属性名"/>
	<association property="Bean中的另一个Bean" javaType="smbmsProvider">
		<id column="主键字段名" property="属性名"/>
	</association>
</resultMap>
<select id="方法名" resultMap="标识名">
	SQL查询语句
</select>
```

* #### 分步查询



### 3.一对多

若多个字段名与实体类中的属性为多个字段对应一个属性时

* #### 使用collection处理映射关系

```xml
<resultMap id="标识名" type="与结果集对应的Bean实体类">
	<id column="主键字段名" property="属性名"/>
	<collection property="Bean中的集合" ofType="集合的泛型">
		<id column="主键字段名" property="属性名"/>
	</collection>
</resultMap>
<select id="方法名" resultMap="标识名">
	SQL查询语句
</select>
```

* #### 分步查询

>Tip：多对一或一对多除了以上都还可以选择分步查询处理
>分步查询的优点：可以实现延迟加载但是必须在核心配置文件中设置全局配置信息：
>lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
>aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。
>否则，每个属性会按需加载此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。
>此时可通过association和collection中的fetchType属性设置当前的分步查询是否使用延迟加载， fetchType="lazy(延迟加
>载)|eager(立即加载)"



## 七、动态SQL

### 1.if标签
if标签可通过test属性的表达式进行判断，若表达式的结果为true，则标签中的内容会执行；反之标签中的内容不会执行
```xml
<if test="表达式">
	Sql语句
</if>
```



### 2.where标签

where和if一般结合使用：
若where标签中的if条件表达式都不满足，则where标签没有任何功能，即不会添加`WHERE`关键字
若where标签中的if条件表达式满足，则where标签会自动添加`WHERE`关键字，并将条件最前方多余的`AND`去掉
```xml
<where>
    <if test="条件表达式">
    	AND Sql表达式
    </if>
</where>
```



### 3.set标签

set和if一般结合使用：
若set标签中的if条件表达式都不满足，则set标签没有任何功能，即不会添加`SET`关键字
若set标签中的if条件表达式满足，则set标签会自动添加`SET`关键字，并将语句最后方多余的 `,` 去掉

```xml
<set>
	<if test="条件表达式">
		Sql赋值表达式 ,
	</if>
</set>
```



### 4.trim标签

trim和if一般结合使用：
用于去掉或添加标签中的内容
> **常用属性：**
> prefix：在trim标签中的内容的前面添加某些内容
> prefixOverrides：在trim标签中的内容的前面去掉某些内容
> suffix：在trim标签中的内容的后面添加某些内容
> suffixOverrides：在trim标签中的内容的后面去掉某些内容
```xml
<trim prefix="WHERE(SET)" suffixOverrides="AND(OR)">
    <if test="条件表达式">
    	OR Sql表达式
    </if>
</trim>
```



### 5.choose、when、otherwise标签

choose标签相当于JAVA的swich分支语句，when相当于case关键字，otherwise相当于defot关键字
```xml
<choose>
	<when test="proCode != null and proCode != ''">
		AND proCode LIKE CONCAT('%',#{proCode},'%')
	</when>
	<otherwise>
		AND 1=1
	</otherwise>
</choose>
```



### 6.foreach

foreach标签：用于遍历
> **常用属性：**
> collection：传入的参数类型（list、array等）
> item：数组中的每次遍历的元素
> open：遍历最前面拼接的内容
> close ：遍历最后面拼接的内容
> separator：每次遍历后拼接的内容
```xml
<foreach collection="遍历的目标类型" item="每次遍历的元素" open="遍历前拼接的内容" close="遍历后拼接的内容" separator="每次遍历后拼接的内容">
	#{userId}
</foreach>
```



### 7.sql标签

可以记录一段公共sql片段，在使用的地方通过include标签进行引入

```xml
<sql id="片段名">
	SQL语句的片段
</sql>
select <include refid="片段名"></include> from ...
```



## 八、MyBatis的缓存

MyBatis缓存有两种，**一级缓存**和**二级缓存**，用于提升程序运行效率

> **查询的顺序：**
> 先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用。
> 如果二级缓存没有命中，再查询一级缓存
> 如果一级缓存也没有命中，则查询数据库
> SqlSession关闭之后，一级缓存中的数据会写入二级缓存

### 1.一级缓存

一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问

> 一级缓存失效的情况：
>
> * 不同的SqlSession对应不同的一级缓存
> * 同一个SqlSession但是查询条件不同
> * 同一个SqlSession两次查询期间执行了任何一次增删改操作
> * 同一个SqlSession两次查询期间手动清空了缓存

### 2.二级缓存

二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取

二级缓存开启的条件：
a>在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
b>在映射文件中设置标签

```xml
    <!--
    二级缓存针对于Mapper文件 （针对接口开启二级缓存） 在语句标签使用useCache="true"
        eviction="FIFO"：缓存策略：FIFO先进先出
        size="512"：缓存大小：最多缓存512个对象
        flushInterval="60000"：自动刷新时间：60秒刷新一次
        readOnly="true"：只读：true只能读取 false可读写
    查询数据顺序：二级缓存 -> 一级缓存 -> 数据库 （匹配到值直接返回）
    当一级缓存关闭，一级缓存会放入二级缓存中（如果设置了二级缓存）
    -->
    <cache eviction="FIFO" size="512" flushInterval="60000" readOnly="true"/>
    
在mapper配置文件中添加的cache标签可以设置一些属性：
①eviction属性：缓存回收策略，默认的是 LRU。
        LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。
        FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。
        SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
        WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
②flushInterval属性：刷新间隔，单位毫秒
        默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新
③size属性：引用数目，正整数
        代表缓存最多可以存储多少个对象，太大容易导致内存溢出
④readOnly属性：只读， true/false
        true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。
        false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false。
```

c>二级缓存必须在SqlSession关闭或提交之后有效
d>查询的数据所转换的实体类类型必须实现序列化的接口

>**二级缓存失效的情况：**
>
>* 两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效
