# Spring

## 一.Spring简介

Spring是分层的 Java SE/EE应用 full-stack（全栈的） 轻量级开源框架，以 **IOC**（Inverse Of Control：控制反转）和 **AOP**（Aspect Oriented Programming：面向切面编程）为内核。

提供了**展现层 SpringMVC**和**持久层 Spring JDBCTemplate**以及**业务层事务管理**等众多的企业应用技术，还能整合开源世界众多著名的第三方框架**MyBatis**等和类库，逐渐成为使用最多的Java EE 企业应用开源框架

### 1.官网

[春天 |家 --- Spring | Home](https://spring.io/)

### 2.特性优势

* 方便解耦，简化开发
通过Spring 提供的IOC容器，可以将对象间的依赖关系交由Spring进行控制，避免硬编码所造成的过度耦合用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用
* AOP 编程的支持
通过 Spring的AOP功能，方便进行面向切面编程，许多不容易用传统OOP实现的功能可以通过AOP轻松实现
* 声明式事务的支持
可以将我们从单调烦闷的事务管理代码中解脱出来，通讨声明式方式灵活的进行事务管理，提高开发效率和质量
* 方便程序的测试
可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情
* 方便集成各种优秀框架
Spring对各种优秀框架（Struts、Hibemate、Hessian、Quartz等）的支持
* 隆低JavaEE API的使用难度
Spring对JaveEE API（如JDBC、JavaMail、远程调用等）进行了薄薄的封装层，使这些API的使用难度大为降低
* Java 源码是经典学习范例
Spring的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对Java 设计模式灵活运用以及对Java技术的高深造诣。它的源代码无意是Java技术的最佳实践的范例

## 二.搭建开发环境

### 1.开发环境

IDEA

### 2.创建项目

加入Jar包

### 3.创建Spring的配置文件

## 三.IoC/DI

### 1. 控制反转IoC

控制反转是一种思想

将对象的创建权利交出去，交给第三方容器负责
将对象和对象之间的关系维护权交出去，交给第三方容器负责

通过依赖注入DI的方式实现

### 2. 依赖注入DI

依赖注入DI （Dependency Injection）实现了控制反转的思想，是指Spring创建对象的过程中，将对象依赖属性通过配置进行注入

依赖注入常见的实现方式有两种：

set注入
构造注入
所以 IoC 是一种控制反转的思想，而 DI 是对IoC的一种具体实现。

Bean管理：指Bean对象的创建，以及Bean对象中属性的赋值（或Bean对象之间关系的维护）

### 3. IoC容器实现

Spring中的IoC容就是IoC思想的一个落地产品实现。IoC容器中管理的组件也叫做bean。在创建bean之前，首先需要创建IoC容器，Spring提供了IoC容器的两种实现方式

① BeanFactory

 这是IoC容器的基本实现，是Spring内部使用的接口。面向Spring本身，不提供给开发人员使用。

② ApplicationContext

 BeanFactory的子接口，提供了更多高级特性，面向Spring的使用者，几乎所有场合都使用 ApplicationContext，而不是底层的BeanFactory

③ ApplicationContext的主要实现类

类型	说明
ClassPathXmlApplicationContext	通过读取类路径下的xml格式配置文件创建IoC容器对象
FileSystemApplicationContext	通过文件系统路径读取xml格式配置文件创建IoC容器对象

### 4. 基于XML管理bean

4.1 环境准备
① 创建子工程 spring-ioc-xml

② pom.xml中引入spring依赖，并刷新maven

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.24</version>
    </dependency>
</dependencies>
③ 创建spring配置文件：resources/bean.xml



④ 在工程目录java下创建类 cn.tedu.spring.iocxml.User

public class User {
    private String username;
    private String password;
    
    public void userMethod(){
        System.out.println("userMethod执行~~");
    }
4.2 获取bean方式
根据id获取

id属性是bean的唯一标识，所以根据bean标签的id属性可以精确获取到一个组件对象。

① bean.xml

<bean id="user" class="cn.tedu.spring.iocxml.User"></bean>
② 创建测试类UserTest

public class UserTest {

    public static void main(String[] args) {
        // 1.加载配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        // 2.根据id获取bean
        User user1 = (User) context.getBean("user");
        System.out.println("1-根据id获取对象:" + user1);
        user1.userMethod();
    }

根据类型获取

User user2 = context.getBean(User.class);
System.out.println("2-根据类型获取bean:" + user2);
user2.userMethod();
根据id和类型获取

User user3 = context.getBean("user", User.class);
System.out.println("3-根据id和类型获取bean：" + user3);
user3.userMethod();
注意

当根据类型获取bean时，要求IoC容器中指定类型的bean只能有一个，当配置两个时会抛出异常

<bean id="user" class="cn.tedu.spring.iocxml.User"></bean>
<bean id="user2" class="cn.tedu.spring.iocxml.User"></bean>



4.3 基于setter依赖注入
类有属性，创建对象过程中，向属性注入具体的值

方式1：使用set方法完成（使用xml中的标签实现）

方式2：基于构造器完成

案例

① 创建Package名为dibase，创建Book类

package cn.tedu.spring.DI;

public class Book {
    private String bookName;
    private String bookAuthor;

    // 无参构造函数
    public Book() {}
    
    // 全参构造函数
    public Book(String bookName, String bookAuthor) {
        this.bookName = bookName;
        this.bookAuthor = bookAuthor;
    }
    
    public String getBookName() {
        return bookName;
    }
    
    public void setBookName(String bookName) {
        this.bookName = bookName;
    }
    
    public String getBookAuthor() {
        return bookAuthor;
    }
    
    public void setBookAuthor(String bookAuthor) {
        this.bookAuthor = bookAuthor;
    }
    
    @Override
    public String toString() {
        return "Book{" +
                "bookName='" + bookName + '\'' +
                ", bookAuthor='" + bookAuthor + '\'' +
                '}';
    }
}
2② 创建spring配置文件：resources目录下创建 bean-di.xml

<!-- set方法注入 -->
<bean id="book" class="cn.tedu.spring.DI.Book">
    <!--2.使用property标签注入-->
    <property name="bookName" value="java"></property>
    <property name="bookAuthor" value="tedu"></property>
</bean>
③ 创建测试类TestBook进行测试

public class BookTest {
    // spring的set方法注入
    @Test
    public void springSetTest(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean-di.xml");
        Book book = context.getBean("book", Book.class);
        System.out.println("book = " + book);
    }
}
4.4 基于构造器依赖注入
说明

通过构造器方式实现依赖注入

操作步骤说明

创建类，定义属性，生成有参数构造方法
进行xml配置
创建测试类测试
① 创建电影信息类Film，定义属性并生成全参构造方法

public class Film {
    // 电影名称、主演
    private String title;
    private String actor;

    // 全参构造
    public Film(String title, String actor) {
        System.out.println("Film的有参构造已经执行~~");
        this.title = title;
        this.actor = actor;
    }
    
    public String getTitle() {
        return title;
    }
    
    public void setTitle(String title) {
        this.title = title;
    }
    
    public String getActor() {
        return actor;
    }
    
    public void setActor(String actor) {
        this.actor = actor;
    }
    
    @Override
    public String toString() {
        return "Film{" +
                "title='" + title + '\'' +
                ", actor='" + actor + '\'' +
                '}';
    }
② 在bean-di.xml中进行注入配置

<!-- 构造器注入演示：Film类 -->
<bean id="film" class="cn.tedu.spring.DI.Film">
    <constructor-arg name="title" value="霸王别姬"></constructor-arg>
    <constructor-arg name="actor" value="张国荣"></constructor-arg>
</bean>
③ 创建测试类TestFilm测试

public class FilmTest {

    @Test
    public void FilmConsDITest(){
        // 1.加载配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext("bean-di.xml");
        // 2.获取指定bean
        Film film = context.getBean("film", Film.class);
        // 3.输出测试
        System.out.println("film = " + film);
    }
}
4.5 特殊值处理注入
4.5.1 字面量赋值
string number = 10;

声明一个变量number，初始化为 10，此时number就不代表字符number了，而是作为一个变量的名字。当引用number时，实际拿到的结果是 10。

而如果number是带引号的 “number” ，则它不是一个变量，而代表 number 本身这个字符串。

这就是字面量，所以字面量没有引申含义，就是我们看到的这个数据本身。

<!-- 使用value属性给bean的属性赋值时，spring会把value的属性值看作是字面量 -->
<property name="number" value="1016"></property>
4.5.2 null值
使用 标签，或者 标签 实现注入。

① Film类中增加电影描述属性

// 1.电影描述
private String description;
// 2.生成对应的 set() get() 方法，重新生成toString()方法
// 3.重新生成全参构造方法
② bean-di.xml配置文件调整

<!-- 构造器注入演示：Film类 -->
<bean id="film" class="cn.tedu.spring.DI.Film">
    <constructor-arg name="title" value="霸王别姬"></constructor-arg>
    <constructor-arg name="actor" value="张国荣"></constructor-arg>
    <!-- 电影描述注入空值null -->
    <constructor-arg name="description">
        <null></null>
    </constructor-arg>
</bean>
③ 执行测试类进行测试

课堂练习

cn.tedu.spring下创建包exercise，在包下创建商品表 Product，类属性如下：

商品标题：title

商品库存：num

商品销量：sales

商品描述：comment

实现 商品Product类的创建，setter() getter() toString()，

通过配置文件bean-product.xml

通过set方式注入一组数据（商品描述为null值）；

通过构造参数方式注入一组数据（商品描述为null值）；

创建测试类TestProduct进行测试。

练习答案

① Product类

public class Product {
    private String title;
    private Integer num;
    private Integer sales;
    private String comment;
    // 无参构造函数、有参构造函数  setter() getter() toString() 
}
② bean-product.xml

<!-- set方法注入 -->
<bean id="product" class="cn.tedu.spring.exercise.Product">
    <property name="title" value="手机"></property>
    <property name="num" value="100"></property>
    <property name="sales" value="1000"></property>
    <property name="comment">
        <null></null>
    </property>
</bean>

<!-- 构造参数方法注入 -->
<bean id="productCons" class="cn.tedu.spring.exercise.Product">
    <constructor-arg name="title" value="电脑"/>
    <constructor-arg name="num" value="2"/>
    <constructor-arg name="sales" value="3"/>
    <constructor-arg name="comment">
        <null></null>
    </constructor-arg>
</bean>
③ ProductTest测试类

@Test
public void testProduct(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-product.xml");
    Product product = context.getBean("product", Product.class);
    System.out.println("product = " + product);
}

4.5.3 xml实体
说明

< > 小于号、大于号在XML文档中用来定义标签的开始，具有特殊含义，在注入属性值时不能够随便使用，

可以使用XML实体 &lt; &gt; 来代替

表示方式

普通字符	xml实体

<!-- xml实体 -->
<bean id="filmEntity" class="cn.tedu.spring.DI.Film">
    <constructor-arg name="title" value="霸王别姬"></constructor-arg>
    <constructor-arg name="actor" value="张国荣"></constructor-arg>
    <!--xml实体表示-->
    <constructor-arg name="description" value="&lt;真好看啊电影&gt;"></constructor-arg>
</bean>
4.5.4 CDATA区
CDATA区，是xml中一种特有的写法，在CDATA区中可以包含特殊符号

表示方式：

 <![CDATA[内容]]> ，在内容区域可以存放普通字符和特殊符号

CDATA区存放特殊符号演示

<!-- xml实体-CDATA区 -->
<bean id="filmCdata" class="cn.tedu.spring.DI.Film">
    <constructor-arg name="title" value="霸王别姬"></constructor-arg>
    <constructor-arg name="actor" value="张国荣"></constructor-arg>
    <!--xml实体表示-->
    <constructor-arg name="description">
        <!-- CDATA区存放数据，可通过 CD + Tab键自动补全格式 -->
        <value><![CDATA[<真好看啊>]]></value>
    </constructor-arg>
</bean>
4.6 对象类型属性注入
需要注入的数据类型为对象，而不是一个字面量。

环境准备

准备一个一对多案例，比如部门Dept和员工Emp是一对多的关系。

① 创建包diobj，并创建部门类Dept

public class Dept {
    // 部门名称
    private String dName;

    // 定义方法，用于测试输出
    public void deptFunc(){
        System.out.println("Dept部门名称：" + dName);
    }
    
    public void setdName(String dName) {
        this.dName = dName;
    }
    
    public String getdName() {
        return dName;
    }
    
    @Override
    public String toString() {
        return "Dept{" +
                "dName='" + dName + '\'' +
                '}';
    }
}
② 创建员工类Emp，创建setter() getter() 和 toString()方法

public class Emp {
    // 员工所属部门的对象、姓名、工资
    private Dept dept;
    private String eName;
    private Double salary;

    // 定义方法测试
    public void work(){
        System.out.println(eName + "薪资:" + salary);
        dept.deptFunc();
    }
    
    public void setDept(Dept dept) {
        this.dept = dept;
    }
    
    public void seteName(String eName) {
        this.eName = eName;
    }
    
    public void setSalary(Double salary) {
        this.salary = salary;
    }
    
    public Dept getDept() {
        return dept;
    }
    
    public String geteName() {
        return eName;
    }
    
    public Double getSalary() {
        return salary;
    }
    
    @Override
    public String toString() {
        return "Emp{" +
                "dept=" + dept +
                ", eName='" + eName + '\'' +
                ", salary=" + salary +
                '}';
    }

4.6.1 引用外部bean
说明：

 可以通过在当前bean标签中通过 ref属性引用外部bean的方式实现。



示例：通过使用外部bean方式，在员工中注入部门对象

① 配置文件 bean-diobj.xml

<!--在Emp中注入Dept
    方式1：引用外部bean
        1.创建两个类对象：dept 和 emp
        2.在emp的bean标签中，通过property标签注入dept的bean
    -->
<bean id="dept1" class="cn.tedu.spring.diobj.Dept">
    <property name="dName" value="开发部"></property>
</bean>

<bean id="emp1" class="cn.tedu.spring.diobj.Emp">
    <!-- 普通属性注入 -->
    <property name="eName" value="张三丰"></property>
    <property name="salary" value="50000.0"></property>
    <!-- 对象类型注入，使用ref属性 -->
    <property name="dept" ref="dept1"></property>
</bean>
② 创建测试类测试 TestDept

public class TestDept {

    // 对象类型注入测试用例
    @Test
    public void testObjDI(){
        // 1.加载xml配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext("bean-diobj.xml");
        // 2.获取bean对象
        Emp emp1 = context.getBean("emp1", Emp.class);
        // 3.测试(调用员工emp对象的方法)
        System.out.println("emp1 = " + emp1);
        emp1.work();
    }
}
4.6.2 内部bean
在需要注入对象的bean标签中内嵌 对象类型属性的 bean标签即可。



 内嵌bean



① bean-diobj.xml进行属性注入配置

<!--在Emp中注入Dept
        方式2：引用内部bean
            在emp的bean标签中，通过内嵌部门bean标签方式实现
        -->
<bean id="emp2" class="cn.tedu.spring.diobj.Emp">
    <property name="eName" value="张无忌"/>
    <property name="salary" value="8000.0"/>
    <!--对象注入-->
    <property name="dept">
        <bean id="dept2" class="cn.tedu.spring.diobj.Dept">
            <property name="dName" value="销售部"/>
        </bean>
    </property>
</bean>

② 使用测试类测试

// 对象类型注入：内嵌bean
@Test
public void testObjDi2(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-diobj.xml");
    Emp emp2 = context.getBean("emp2", Emp.class);
    System.out.println("emp2 = " + emp2);
    emp2.work();
}

4.6.3 级联属性赋值（了解）
可以在标签中给需要注入对象的属性重新赋值！

① 配置文件编写

<!--方式3：级联属性(需要注入的属性)赋值-->
<bean id="dept3" class="cn.tedu.spring.diobj.Dept">
    <property name="dName" value="市场部"/>
</bean>

<bean id="emp3" class="cn.tedu.spring.diobj.Emp">
    <!-- 普通属性注入 -->
    <property name="eName" value="赵敏"/>
    <property name="salary" value="5000.0"/>
    <!-- 对象类型注入 -->
    <property name="dept" ref="dept3"/>
    <!-- 级联属性(Dept)赋值 -->
    <property name="dept.dName" value="客服部"></property>
</bean>

② 测试类测试

// 对象类型注入：级联属性赋值
@Test
public void testObjDi3(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-diobj.xml");
    Emp emp3 = context.getBean("emp3", Emp.class);
    System.out.println("emp3 = " + emp3);
    emp3.work();
}

4.7 数组类型属性注入
使用 标签和子标签实现。

说明：一个人除了姓名、年龄等属性外，还会有爱好，一个人的爱好可能有多个，可以把多个爱好存入数组中。

创建包：cn.tedu.spring.diarray

① 在diarray包中创建类：Person

public class Person {
    // 姓名、年龄、爱好
    private String name;
    private String age;
    private String[] hobby;

    // 定义测试方法
    public void run(){
        System.out.println("Persen is running...");
        // 打印数组测试
        System.out.println(Arrays.toString(hobby));
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getAge() {
        return age;
    }
    
    public void setAge(String age) {
        this.age = age;
    }
    
    public String[] getHobby() {
        return hobby;
    }
    
    public void setHobby(String[] hobby) {
        this.hobby = hobby;
    }
    
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age='" + age + '\'' +
                ", hobby=" + Arrays.toString(hobby) +
                '}';
    }
}
② 新建配置文件：bean-diarray.xml 进行注入

<!-- 创建Person对象并注入属性 -->
<bean id="person" class="cn.tedu.spring.diarray.Person">
    <!-- 普通属性注入 -->
    <property name="name" value="孙悟空"/>
    <property name="age" value="36"/>
    <!-- 数组属性注入，使用<array>标签 -->
    <property name="hobby">
        <array>
            <value>抽烟</value>
            <value>喝酒</value>
            <value>烫头</value>
        </array>
    </property>
</bean>

③ 编写测试类TestPerson测试

public class TestPerson {

    // 数组注入测试用例
    @Test
    public void testArray(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean-diarray.xml");
        Person person = context.getBean("person", Person.class);
        System.out.println("person = " + person);
        person.run();
    }
}

4.8 集合类型属性注入
4.8.1 List集合属性注入
场景1：使用 标签下的 子标签和 子标签实现。

场景2：使用 标签下的 子标签和子标签实现。ref标识引用其他的bean

环境说明：创建老师类Student和学生类Student，一个老师可以有多个学生，在老师类中存入所教学生的对象，将其存入List集合中。

环境准备

① 创建包 dimap

② Teacher类

public class Teacher {

    // 老师姓名
    private String tName;
    // 老师所教学生的对象，放到List集合中
    private List<Student> studentList;
    
    public String gettName() {
        return tName;
    }
    
    public void settName(String tName) {
        this.tName = tName;
    }
    
    public List<Student> getStudentList() {
        return studentList;
    }
    
    public void setStudentList(List<Student> studentList) {
        this.studentList = studentList;
    }
    
    @Override
    public String toString() {
        return "Teacher{" +
                "tName='" + tName + '\'' +
                ", studentList=" + studentList +
                '}';
    }
}
③ Student类

public class Student {
    // 学生姓名、年龄
    private String sName;
    private String age;

    public String getsName() {
        return sName;
    }
    
    public void setsName(String sName) {
        this.sName = sName;
    }
    
    public Integer getAge() {
        return age;
    }
    
    public void setAge(Integer age) {
        this.age = age;
    }
    
    @Override
    public String toString() {
        return "Student{" +
                "sName='" + sName + '\'' +
                ", course='" + course + '\'' +
                '}';
    }
}

④ 创建配置文件：bean-dilistmap.xml 进行注入

<!-- 创建2个Student对象，用于Teacher对象的注入 -->
<bean id="stu1" class="cn.tedu.spring.dimap.Student">
    <property name="sName" value="梁山伯"/>
    <property name="age" value="43"/>
</bean>
<bean id="stu2" class="cn.tedu.spring.dimap.Student">
    <property name="sName" value="祝英台"/>
    <property name="age" value="33"/>
</bean>

<!-- 创建Teacher类的bean对象，并注入属性 -->
<bean id="teacher" class="cn.tedu.spring.dimap.Teacher">
    <!-- 普通属性注入 -->
    <property name="tName" value="沙师弟"/>
    <!-- List集合属性注入 -->
    <property name="studentList">
        <list>
            <ref bean="stu1"/>
            <ref bean="stu2"/>
        </list>
    </property>
</bean>
⑤ 测试类TestTeacher测试

public class TestTeacher {

    @Test
    public void testListMap(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean-dilistmap.xml");
        Teacher teacher = context.getBean("teacher", Teacher.class);
        System.out.println("teacher = " + teacher);
    
        List<Student> list = teacher.getStudentList();
        for (Student student: list) {
            System.out.println(student);
        }
    }
}
4.8.2 Map集合属性注入
使用标签下的 子标签、子标签、子标签 子标签 子标签实现

<bean id="xxx" class="xxx">
	<property name="xxx">
        <map>
            <!-- 第1条数据-字面量值演示 -->
			<entry>
            	<key><value>xxx</value></key>
                <value>xxx</value>
            </entry>
            <!-- 第2条数据-对象演示 -->
			<entry>
            	<key><value>xxx</value></key>
                <ref bean="xxx"></ref>
            </entry>
        </map>
​	</property>
</bean>
说明：使用上述的老师类和学生类，一个学生也可以有多个老师，在学生类Student中添加老师的属性，放到Map集合中。

① 调整Student类

// 1.学生的老师：可以有多个，放到Map集合中
private Map<String,String> teacherMap;

// 2.生成setter() getter()方法，重新生成toString()方法
② 创建配置文件：bean-dimap.xml

<!--Map集合属性注入-->
<bean id="stuMap" class="cn.tedu.spring.dilistmap.Student">
    <property name="sName" value="步惊云"/>
    <property name="age" value="36"/>
    <property name="teacherMap">
        <map>
            <entry>
                <key>
                    <value>1111</value>
                </key>
                <value>雄霸</value>
            </entry>
            <entry>
                <key>
                    <value>2222</value>
                </key>
                <value>断浪</value>
            </entry>
            <entry>
                <key>
                    <value>3333</value>
                </key>
                <value>大空翼</value>
            </entry>
        </map>
    </property>
</bean>
③ 创建测试类进行测试 TestMap

@Test
public void testMap(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-dimap.xml");
    Student student = context.getBean("stuMap", Student.class);
    System.out.println("student = "  + student);
}
4.8.3 引用集合类型bean注入
说明

通过使用 标签实现

使用步骤

在xml配置文件中引入util约束

<beans
	xmlns:util="http://www.springframework.org/schema/util"
	xsi:schemaLocation="
       http://www.springframework.org/schema/util
       http://www.springframework.org/schema/util/spring-util.xsd"
>

</beans>
使用util标签进入注入

<!-- Map集合util标签 -->
<util:map id="xxx"></util:map>

<!-- List集合util标签 -->
<util:list id="xxx"></util:list>
环境准备及操作步骤

添加课程类，一个学生可以上多门课程

① 在Student类中添加List集合属性

// 1.一个学生可以上多门课程,把课程名称放到List集合中
    private List<String> courseList;
// 2.生成get和set方法
// 3.重新生成toString()方法

② 创建spring配置文件：bean-diref.xml，引入util约束

<!-- 添加3行带有util的配置 -->

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:util="http://www.springframework.org/schema/util"
    
       xsi:schemaLocation="
       http://www.springframework.org/schema/util
       http://www.springframework.org/schema/util/spring-util.xsd
    
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
③ 配置xml文件完成注入

<!--引用集合类型bean注入-->
<bean id="stuUtil" class="cn.tedu.spring.dilistmap.Student">
    <property name="sName" value="孔慈"/>
    <property name="age" value="36"/>
    <property name="teacherMap" ref="teacherMap"></property>
    <property name="courseList" ref="courseList"></property>
</bean>

<util:map id="teacherMap">
    <entry>
        <key>
            <value>10000</value>
        </key>
        <value>小泽老师</value>
    </entry>
    <entry>
        <key>
            <value>10001</value>
        </key>
        <value>王老师</value>
    </entry>
</util:map>

<util:list id="courseList">
    <value>Spring</value>
    <value>SpringMVC</value>
    <value>MyBatis</value>
</util:list>

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
④ 创建测试方法进行测试

// 引用集合类型bean注入（util）
@Test
public void testRefBean(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-diref.xml");
    Student student = context.getBean("stuUtil", Student.class);
    System.out.println("student = " + student);
}
1
2
3
4
5
6
7
4.9 p命名空间
这也是一种注入方式，可以在xml中定义命名空间或者叫名称空间，可以简化xml代码。

在bean-diref.html中操作

① 在xml配置文件中定义命名空间

xmlns:p="http://www.springframework.org/schema/p"
1
② 在xml文件进行命名空间属性注入

<!-- p命名空间注入: 注入学生属性 -->
<bean id="studentp" class="cn.tedu.spring.iocxml.dimap.Student" p:sid="100" p:sname="铁锤妹妹" p:courseList-ref="courseList" p:teacherMap-ref="teacherMap">
1
2
③ 测试

// p命名空间注入测试用例
@Test
public void testRefP(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-diref.xml");
    Student studentp = context.getBean("studentp", Student.class);
    System.out.println("studentp = " + studentp);
}
1
2
3
4
5
6
7
4.10 引入外部属性文件
说明

 当前所有的配置和数据都在xml文件中，一个文件中有很多bean，修改和维护起来很不方便，生产环境中会把特定的固定值放到外部文件中，然后引入外部文件进行注入，比如数据库连接信息。

示例

将外部文件中的数据引入xml配置文件进行注入

① pom.xml中引入数据库相关依赖，并刷新maven

<!-- MySQL驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
</dependency>

<!-- 数据源，连接池依赖 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.21</version>
</dependency>
1
2
3
4
5
6
7
8
9
10
11
12
13
② resources目录下创建外部属性文件，一般为properties格式，定义数据库信息，比如：jdbc.properties

jdbc.user=root
jdbc.password=root
jdbc.url=jdbc://mysql://localhost:3306/spring
jdbc.driver=com.mysql.cj.jdbc.Driver
1
2
3
4
③ 创建spring配置文件bean-jdbc.xml，引入context的命名空间

使用context可以为XML外部实体注入定义，使得解析器在解析XML文档时可以正确地识别外部实体

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"
       
       xsi:schemaLocation="
       http://www.springframework.org/schema/context 
       http://www.springframework.org/schema/context/spring-context.xsd
       
       http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">


​    
​    <!-- 引入外部属性文件 -->
​    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
​    <!-- 完成数据库信息注入 -->
​    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
​        <property name="url" value="${jdbc.url}"></property>
​        <property name="username" value="${jdbc.user}"></property>
​        <property name="password" value="${jdbc.password}"></property>
​        <property name="driverClassName" value="${jdbc.driver}"></property>
​    </bean>
</beans>

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
④ 创建包jdbc，包中创建测试类 TestJdbc

public class TestJdbc {

    // 外部文件属性引入测试用例
    @Test
    public void demo02(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean-jdbc.xml");
        DruidDataSource druidDataSource = context.getBean("druidDataSource", DruidDataSource.class);
        System.out.println(druidDataSource.getUrl());
    }
}
1
2
3
4
5
6
7
8
9
10
4.11 bean的作用域
说明

 bean的作用域，是指在交给spring创建bean对象时，可以指定是单实例还是多实例，通过bean标签中的scope属性来指定，默认是单实例。

单实例和多实例

单实例

单实例（Singleton）是指某个类只能创建唯一的一个实例对象，并且该类提供一个全局的访问点（静态方法）来让外界获取这个实例，常常用在那些只需要一个实例来处理所有任务的场景下，例如配置类或数据库连接池等。

多实例

多实例（Multiple Instance）则是指可以在同一个类的定义下，创建多个实例对象。每个对象都是相互独立的，有自己的状态和行为；常常用于需要同时处理多个任务的场景。

在Spring中可以通过配置bean标签的scope属性来之地那个bean的作用域范围，具体如下

取值	含义	创建对象时机
singleton（默认）	在IoC容器中，这个bean的对象为单实例	IoC容器初始化时
prototype	这个bean在IoC容器中有多个实例	获取bean时
案例演示

① 创建包scope，并在包下创建类Sku

public class Sku {
    
}
1
2
3
② 创建spring的配置文件：bean-scope.xml

<!-- singleton:单实例 -->
<!-- 之后改为prototype多实例测试 -->
<bean id="sku" class="cn.tedu.spring.scope.Sku" scope="singleton"></bean>
1
2
3
③ 创建测试类TestOrders

@Test
public void testOrders(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-scope.xml");
    Orders orders = context.getBean("orders", Orders.class);
    System.out.println("orders = " + orders);
    Orders orders1 = context.getBean("orders", Orders.class);
    System.out.println("orders1 = " + orders1);
}
// 单实例，sku1和sku2地址相同
// 多实例，sku1和sku2地址不同
1
2
3
4
5
6
7
8
9
10
4.12 bean的生命周期
是指一个bean对象从创建到销毁的整个过程。

4.12.1 bean的完整生命周期
实例化阶段（bean对象创建）

在这个阶段中，容器会创建一个Bean的实例，并为其分配空间。这个过程可以通过构造方法完成。

属性赋值阶段

在实例化完Bean之后，容器会把Bean中的属性值注入到Bean中，这个过程可以通过set方法完成。

初始化阶段（bean对象初始化）

在属性注入完成后，容器会对Bean进行一些初始化操作；

初始化之前：bean的后置处理器可以接收到bean，此处可以对bean做相关操作。
初始化之后：bean的后置处理器可以接收到bean，此处可以对bean做相关操作。
使用阶段

初始化完成后，Bean就可以被容器使用了

销毁阶段

容器在关闭时会对所有的Bean进行销毁操作，释放资源。

4.12.2 生命周期验证
① 创建包life，创建类User

package cn.tedu.spring.life;

import org.springframework.beans.BeansException;

public class User {
    private String username;

    // 1.无参数构造
    public User(){
        System.out.println("1-bean对象创建，调用无参数构造。");
    }
    
    // 3.初始化阶段
    public void initMethod(){
        System.out.println("3-bean对象初始化，调用指定的初始化方法");
    }
    
    // 5.销毁阶段
    public void destoryMethod(){
        System.out.println("5-bean对象销毁，调用指定的销毁方法");
    }
    
    public String getUsername() {
        return username;
    }
    
    public void setUsername(String username) {
        this.username = username;
        // 2.给bean对象属性赋值
        System.out.println("2-通过set方法给bean对象赋值。");
    }
    
    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                '}';
    }
}


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
② 创建spring配置文件 bean-life.xml

<bean id="user" class="cn.tedu.spring.iocxml.life.User" scope="singleton" 
      init-method="initMethod" destroy-method="destroyMethod">
    <property name="username" value="聂风"></property>
</bean>
1
2
3
4
③ 创建测试类TestUser测试

@Test
public void testUser(){
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean-life.xml");
    User user = context.getBean("user", User.class);
    // 4.bean对象初始化完成，可以使用
    System.out.println("4-bean对象初始化完成，开发者可以使用了。");
    // 销毁bean
    context.close();
}
1
2
3
4
5
6
7
8
9
④ 后置处理器处理演示，新建类MyBeanPost

public class MyBeanPost implements BeanPostProcessor {
    // BeanPostProcessor接口
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("3之前:bean后置处理器，在初始化之前执行。" + beanName + ":" + bean);
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("3之后:bean后置处理器，在初始化之后执行。" + beanName + ":" + bean);
        return bean;
    }
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
⑤ 在spring的配置文件bean-life.xml中进行后置处理器配置

<!-- bean的后置处理器需要放到IoC容器中才能生效 -->
<bean id="myBeanPost" class="cn.tedu.spring.life.MyBeanPost"></bean>
1
2
⑥ 运行测试类测试

4.12.3 bean生命周期扩展
bean的初始化和销毁应用场景

初始化
创建连接池
加载资源文件
进行数据校验
销毁
关闭连接池
保存数据
释放占用的资源
后置处理器

实现自定义的Bean对象处理逻辑，比如在Bean实例化之前或者之后对Bean对象进行自定义的修改，可以方便地实现自定义逻辑和修改Bean对象的行为。

4.13 基于xml自动装配
自动装配说明：

根据指定的策略，在IoC容器中匹配某一个bean，自动为指定的bean中的所依赖的类类型或者接口类型属性赋值。

环境准备

① 创建包auto，创建部门和员工的两个java类

② 部门类 Dept

public class Dept {
    private String dName;

    @Override
    public String toString() {
        return "Dept{" +
                "dName='" + dName + '\'' +
                '}';
    }
    
    public String getdName() {
        return dName;
    }
    
    public void setdName(String dName) {
        this.dName = dName;
    }
}


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
③ 员工类 Emp

public class Emp {
    private String eName;
    private Dept dept;

    @Override
    public String toString() {
        return "Emp{" +
                "eName='" + eName + '\'' +
                ", dept=" + dept +
                '}';
    }
    
    public String geteName() {
        return eName;
    }
    
    public void seteName(String eName) {
        this.eName = eName;
    }
    
    public Dept getDept() {
        return dept;
    }
    
    public void setDept(Dept dept) {
        this.dept = dept;
    }
}

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
④ 创建spring配置文件bean-auto.xml

<!--通过byType和byName自动装配-->
<bean id="dept" class="cn.tedu.spring.iocxml.auto.Dept">
    <property name="dName" value="技术部"></property>
</bean>

<!--autowire="byType" 或者 autowire="byName"-->
<bean id="emp" class="cn.tedu.spring.iocxml.auto.Emp" autowire="byType">
    <property name="eName" value="步惊云"></property>
</bean>
1
2
3
4
5
6
7
8
9
⑤ 创建测试类测试TestAuto

@Test
public void testAuto(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean-auto.xml");
    Emp emp = context.getBean("emp", Emp.class);
    System.out.println("emp = " + emp);
}
1
2
3
4
5
6
使用bean标签的autowire属性设置自动装配效果；

自动装配方式：byType

 byType: 根据类型匹配IoC容器中的某个兼容类型的bean，为属性自动赋值；

 1. 如果在IoC中，没有任何一个兼容类型的bean能够为属性赋值，则改属性不装配，默认值为null；

 2. 如果在IoC中，有多个兼容类型的bean能够为属性赋值，则抛出异常 NoUniqueBeanDefinitionException

自动装配方式：byName

 byName：将自动装配的属性名，作为bean的id在IoC容器中匹配相对应的bean进行赋值

### 5 基于注解管理bean

 从Java5开始，Java增加了对注解（Annotation）的支持，它是代码中的一种特殊标记，可以在编译、类加载和运行时被读取，执行相应的处理。开发人员可以通过注解在不改变原有代码和逻辑的情况下，在源代码中嵌入补充信息。

 Spring从2.5版本开始提供了对注解技术的全面支持，我们可以使用注解来实现自动装配，简化Spring的xml配置。

Spring通过注解实现自动装配：

引入依赖
开启组件扫描
使用注解定义Bean
依赖注入
5.1 创建子工程
子工程：spring-ioc-annotation

在pom.xml中添加springframework的依赖，刷新maven

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.24</version>
    </dependency>
</dependencies>

5.2 开启组件扫描
 Spring默认不使用注解装配Bean，因此需要在Spring的xml配置中，通过context:component-scan元素开启Spring Beans的自动扫描功能。开启此功能后，Spring会自动从扫描指定的包（base-package属性设置）及其子包下的所有类，如果类上使用了@Component注解，就将该类装配到容器中。

① 工程下创建包：cn.tedu.spring.bean

② resources目录下创建spring配置文件 bean.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"
    
       xsi:schemaLocation="
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
    
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 2.开启组件扫描，让spring可以通过注解方式实现bean管理，包括创建对象、属性注入 -->
    <!-- base-package:扫描哪个包中的注解，在cn.tedu的包或者子包中建了类，在
     类上、属性上、方法上加了spring的@Component注解，这里就能扫描到-->
    <context:component-scan base-package="cn.tedu.spring"></context:component-scan>

</beans>


5.3 使用注解定义Bean
Spring提供了以下多个注解，这些注解可以直接标注在java类上，将它们定义成Spring Bean。

注解	说明
@Component	该注解用于描述Spring中的Bean，它是一个泛化的概念，仅仅标识容器中的一个组件（Bean），并且可以作用在任何层次，例如Service层、Dao层等，使用时只需将该注解标注在相应的类上即可。
@Respository	该注解用于数据访问层（Dao层）的类标识为Spring中的Bean，功能与@Component相同。
@Service	该注解通常作用在业务层（Service层），用于将业务层的类标识为Spring中的Bean，其功能与@Component相同。
@Controller	该注解通常作用在控制层（如SpringMVC的Controller），用于将控制层的类标识为Spring中的Bean，其功能与@Component相同。
③ 创建User类，并添加注解

// value可以不写，默认为类名首字母小写
//@Component(value = "user")  // <bean id="user" class="xxx">
//@Repository
//@Service
@Controller
public class User {

}

④ 创建测试类测试TestUser

public class TestUser {
    @Test
    public void testUser(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        User user = context.getBean("user", User.class);
        System.out.println("user = " + user);
    }
}

5.4 @Autowired注入
单独使用@Autowired注解，默认根据类型装配（byType）

@Autowired注解有一个required属性，默认值是true，表示在注入的时候要求被注入的Bean必须存在，如果不存在则报错。如果required属性设置为false，表示注入的Bean存在或者不存在都没关系，存在就注入，不存在也不报错。

5.4.1 属性注入
① cn.tedu.spring下创建包autowired，并在autowired下创建两个包：controller包 和 service包

② 控制器层controller.UserController

public class UserController {
    private UserService userService;
    
    public void addController(){
        System.out.println("controller is running...");
        userService.addService();
    }
}

③ 服务层service.UserService接口

public interface UserService {
    public void addService();
}

④ 服务层service.UserServiceImpl接口的实现类

public class UserServiceImpl implements UserService {
    @Override
    public void addService() {
        System.out.println("service is running...");
    }
}

⑤ 在UserController和UserSerivceImpl中添加@Controller注解和@Service注解

⑥ 在UserController中注入UserServiceImpl

@Controller
public class UserController {
    // 注入service
    // 第一种方式：属性注入
    @Autowired // 根据类型找到对象，完成注入
    private UserService userService;
}

⑦ 测试类测试autowired.TestUserController

public class TestUserController {

    @Test
    public void testUserController(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        UserController controller = context.getBean(UserController.class);
        controller.addController();
    }
}

5.4.2 set注入
① 修改UserController类

// 方式二：通过set方法注入
private UserService userService;

@Autowired
public void setUserService(UserService userService) {
    this.userService = userService;
}

② 测试

5.4.3 构造方法注入
① 修改UserController类

// 第三种方式：构造方法注入
private UserService userService;

@Autowired
public UserController(UserService userService) {
    this.userService = userService;
}

② 测试

5.4.4 形参上注入
① 修改UserController类

// 第四种方式：形参注入
private UserService userService;

public UserController(@Autowired UserService userService) {
    this.userService = userService;
}

② 测试

5.4.5 只有一个构造函数，无注解
① 修改UserController类

// 第五种方式：只有一个有参数构造函数，无注解
private UserService userService;

public UserController(UserService userService) {
    this.userService = userService;
}

② 测试

5.4.6 @Autowire注解和@Qualifier注解联合
① 再创建一个UserService接口的实现类service.UserServiceImpl2

@Service
public class UserServiceImpl2 implements UserService{
    @Override
    public void addService() {
        System.out.println("service2 is running...");
    }
}

② 测试发现报错

 因为UserService有两个实现类，而@Autowired注解根据byType定位，所以找到了两个实现类

③ 解决：修改UserController （使用两个注解）

// 1.第六种方式：根据类型和名称一起注入
@Autowired
@Qualifier(value = "userServiceImpl2")  // 类名首字母小写
private UserService userService;

// 2.将构造函数注释

5.5 @Resource注入
@Resource注解也可以完成属性注入。它和@Autowired注解的区别如下

@Resource注解是JDK扩展包中的，也就是说属于JDK的一部分。所以该解释是标准注解，更加具有通用性，而@Autowired注解是Spring框架自己的。
@Resource注解默认根据名称装配byName，未指定name时，使用属性名作为name，通过name找不到的话会自动启动通过类型byType装配。而@Autowired注解默认根据类型装配byType，如果想根据名称匹配，需要配合@Qualifier注解一起使用。
@Resource注解用在属性上、setter方法上
@Autowired注解用在属性上、setter方法上、构造方法上、构造方法参数上。
案例演示

① 工程下创建包 resource，和之前一样，创建controller和service两个包，并创建UserController类和UserService接口以及该接口的实现类UserServiceImpl

② 修改UserController

@Controller("myUserController")
public class UserController {
    // 根据名称进行注入
    @Resource(name="myUserService")
    private UserService userService;

    public void add(){
        System.out.println("controller...");
        userService.add();
    }
}

③ 修改ServiceControllerImpl1

@Service("myUserService")
public class UserServiceImpl implements UserService {

⑤ 测试

指定@Resource中的name，则根据名称装配
未指定name时，则根据属性名装配
未指定name，属性名也不一致，则根据类型装配
5.6 Spring全注解开发
全注解开发就是不再使用spring配置文件了，写一个配置类来代替配置文件。

① 工程下创建包：config，创建类SpringConfig

// 配置类
@Configuration
// 开启组件扫描
@ComponentScan("cn.tedu.spring")
public class SpringConfig {
}

② 在resource下创建测试类进行测试

public class TestUserControllerAnno {
    public static void main(String[] args) {
        // 加载配置类
        ApplicationContext context =
                new AnnotationConfigApplicationContext(SpringConfig.class);
        UserController controller = context.getBean(UserController.class);
        controller.add();
    }
}

## 四、基于XML管理bean

### 4.1 环境准备

### 4.2 获取bean方式

### 4.3 基于setter依赖注入

### 4.4 基于构造器依赖注入

### 4.5 特殊值处理注入

#### 4.5.1 字面量赋值

#### 4.5.2 null值

#### 4.5.3 xml实体

#### 4.5.4 CDATA区

### 4.6 对象类型属性注入

4.6.1 引用外部bean
4.6.2 内部bean
4.6.3 级联属性赋值（了解）

### 4.7 数组类型属性注入

### 4.8 集合类型属性注入

4.8.1 List集合属性注入
4.8.2 Map集合属性注入
4.8.3 引用集合类型bean注入

### 4.9 p命名空间

### 4.10 引入外部属性文件

### 4.11 bean的作用域

### 4.12 bean的生命周期

4.12.1 bean的完整生命周期
4.12.2 生命周期验证
4.12.3 bean生命周期扩展

### 4.13 基于xml自动装配

## 五、基于注解管理bean

5.1 创建子工程
5.2 开启组件扫描
5.3 使用注解定义Bean
5.4 @Autowired注入
5.4.1 属性注入
5.4.2 set注入
5.4.3 构造方法注入
5.4.4 形参上注入
5.4.5 只有一个构造函数，无注解
5.4.6 @Autowire注解和@Qualifier注解联合
5.5 @Resource注入
5.6 Spring全注解开发

## 六.AOP面向切面编程

### 1.概念

AOP又名Aspect Oriented Programming 意为 ‘面向切面编程’通过预编译和运行期间动态代理来实现程序功能的统一维护的一种技术。AOP思想是OOP(面向对象)的延续 在 OOP 中, 我们以类(class)作为我们的基本单元, 而 AOP中的基本单元是 Aspect(切面)，AOP是软件行业的热点，也是Spring框架中的一个重要内容，是函数式编程的一种延伸范式,

总结：这种在运行时生成代理对象来织入的，还可以在编译期、类加载期织入，动态地将代码在不改变原有的逻辑情况下切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。

面向切面编程（AOP是Aspect Oriented Program的首字母缩写） ，我们知道，面向对象的特点是继承、多态和封装。而封装就要求将功能分散到不同的对象中去，这在软件设计中往往称为职责分配实际上也就是说，让不同的类设计不同的方法。这样代码就分散到一个个的类中去了。这样做的好处是降低了代码的复杂程度，使类可重用

但是人们也发现，在分散代码的同时，也增加了代码的重复性。什么意思呢？比如说，我们在两个类中，可能都需要在每个方法中做日志。按面向对象的设计方法，我们就必须在两个类的方法中都加入日志的内容。也许他们是完全相同的，但就是因为面向对象的设计让类与类之间无法联系，而不能将这些重复的代码统一起来。

也许有人会说，那好办啊，我们可以将这段代码写在一个独立的类独立的方法里，然后再在这两个类中调用。但是，这样一来，这两个类跟我们上面提到的独立的类就有耦合了，它的改变会影响这两个类。那么，有没有什么办法，能让我们在需要的时候，随意地加入代码呢？

即假如一个流程分三个步骤，分别是X,A,Y，另一个流程的三个步骤是X,B,Y。 写在程序里，两个方法体分别是XAY和XBY，



显然，这出现了重复，违反了DRY原则。 你可以把X和Y分别抽成一个方法，但至少还是要写一条语句来调用方法，xAy，xBy，



重复依然存在。 如果控制反转来处理这问题，将采用模板方法的模式，在抽象父类方法体中声明x?y，其中?部分为抽象方法，由具体子类实现。 但这就出现了继承，而且调用者只能调用父类声明的方法，耦合性太强，不灵活。 所以，我们常看到，只有那些本来就是调用者调用父类声明的方法的情况，比如表现层，或者本来就不用太灵活，比如只提供增删改查的持久层，才总出现抽象父类的身影。

具体Controller is-a 抽象Controller，具体Dao is-a 抽象Dao，这大家都能接受。 但除了在抽象Controller、抽象Dao中固定的步骤之外，我们就不需要点别的吗？ 比如在某些Controller方法运行之前做点什么，在某些Dao方法运行之前之后做点什么？ 而且最好能基于配置，基于约定，而不是都死乎乎硬编码到代码里。

于是乎 面向切面横空出世



一般而言，我们管切入到指定类指定方法的代码片段称为切面，而切入到哪些类、哪些方法则叫切入点。有了AOP，我们就可以把几个类共有的代码，抽取到一个切片中，等到需要时再切入对象中去，从而改变其原有的行为。
这样看来，AOP其实只是OOP的补充而已。OOP从横向上区分出一个个的类来，而AOP则从纵向上向对象中加入特定的代码。有了AOP，OOP变得立体了。如果加上时间维度，AOP使OOP由原来的二维变为三维了，由平面变成立体了。从技术上来说，AOP基本上是通过代理机制实现的。
AOP在编程历史上可以说是里程碑式的，对OOP编程是一种十分有益的补充。

AOP体系:



### 2.相关术语

```
1.增强：实际就是抽取出来的通用代码
前置增强：在目标方法之前执行的通用代码
后置增强：在目标方法之后执行的通用的代码
2.目标方法：增强需要在哪些方法之前执行或者是在哪些方法执行之后，这些方法就叫目标方法
3.连接点：程序运行时能够插入增强的位置
4.切点（表达式）：通过切点找到连接点
5.切面：增强+切点
6.织入：将切面应用到目标方法上，生成代理对象的过程
```

#### 1.Aspect(切面)

Aspect 由 Pointcut 和 Advice 组成, 它既包含了横切逻辑的定义, 也包括了连接点的定义. Spring AOP就是负责实施切面的框架, 它将切面所定义的横切逻辑织入到切面所指定的连接点中.
AOP的工作重心在于如何将增强织入目标对象的连接点上, 这里包含两个工作:

如何通过 Pointcut 和 Advice 定位到特定的 Join Point 上
如何在 Advice 中编写切面代码.
可以简单地认为, 使用 @Aspect 注解的类就是切面.

总结：切面(aspect)：通知(即增强)和切点的结合。

#### 2.连接点(Join Point)

a point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.
程序运行中的一些时间点, 例如一个方法的执行, 或者是一个异常的处理.
在 Spring AOP 中, join point 总是方法的执行点, 即只有方法连接点.

总结：连接点(Join Point)：在应用执行过程中能够插入切面的一个点。（注：就是抽象的「切点」声明所指代的那些具体的点。）

JointPoint是程序运行过程中可识别的点，这个点可以用来作为AOP切入点。JointPoint对象则包含了和切入相关的很多信息。比如切入点的对象，方法，属性等。我们可以通过反射的方式获取这些点的状态和信息，用于追踪tracing和记录logging应用信息。

#### 3.切点(point cut)

匹配Join Point 的谓词(a predicate that matches Join Points).
Advice 是和特定的 Point Cut 关联的, 并且在 Point Cut相匹配的Join Point中执行.
在 Spring 中, 所有的方法都可以认为是 Join Point, 但是我们并不希望在所有的方法上都添加 Advice, 而

总结：切点(Pointcut ):一组连接点的总称,用于指定某个增强应该在何时被调用

通俗一点讲:

Pointcut 的作用就是提供一组规则(使用 AspectJ pointcut expression language 来描述) 来匹配joinpoint, 给满足规则的 joinpoint 添加 Advice .

Pointcut 是一种程序结构和规则，它用于选取Join Point并收集这些point的上下文信息。

Pointcut 通常包含了一系列的Joint Point，我们可以通过pointcut来同时操作jointpoint。单从概念上，可以把Pointcut 当做jointpoint的集合。

关于Join Point 和 Point Cut 的区别

在 Spring AOP 中, 所有的方法执行都是 Join point. 而 point cut 是一个描述信息, 它修饰的是 Join point, 通过 point cut, 我们就可以确定哪些 Join point 可以被织入 Advice. 因此Join point 和 point cut 本质上就是两个不同纬度上的东西.
总结：advice 是在 Join point 上执行的, 而 point cut 规定了哪些Join point可以执行哪些 advice

introduction

为一个类型添加额外的方法或字段. Spring AOP 允许我们为 目标对象 引入新的接口(和对应的实现). 例如我们可以使用 introduction 来为一个 bean 实现 IsModified 接口, 并以此来简化 caching 的实现.

#### 目标对象(Target)

织入 advice 的目标对象. 目标对象也被称为 advised object.
因为 Spring AOP 使用运行时代理的方式来实现 aspect, 因此 adviced object 总是一个代理对象(proxied object)
注意, adviced object 指的不是原来的类, 而是织入 advice 后所产生的代理类.

AOP proxy

一个类被 AOP 织入 advice, 就会产生一个结果类, 它是融合了原类和增强逻辑的代理类.
在 Spring AOP 中, 一个 AOP 代理是一个 JDK 动态代理对象或 CGLIB 代理对象.

#### 织入(Weaving)

将 aspect 和其他对象连接起来, 并创建 adviced object 的过程.
根据不同的实现技术, AOP织入有三种方式:

编译器织入, 这要求有特殊的Java编译器.
类装载期织入, 这需要有特殊的类装载器.
动态代理织入, 在运行期为目标类添加增强(Advice)生成子类的方式.
Spring 采用动态代理织入, 而AspectJ采用编译器织入和类装载期织入.
advice 的类型

before advice, 在 join point 前被执行的 advice. 虽然 before advice 是在 join point 前被执行, 但是它并不能够阻止 join point 的执行, 除非发生了异常(即我们在 before advice 代码中, 不能人为地决定是否继续执行 join point 中的代码)
after return advice, 在一个 join point 正常返回后执行的 advice
after throwing advice, 当一个 join point 抛出异常后执行的 advice
after(final) advice, 无论一个 join point 是正常退出还是发生了异常, 都会被执行的 advice.
around advice, 在 join point 前和 joint point 退出后都执行的 advice. 这个是最常用的 advice.

关于 AOP Proxy

Spring AOP 默认使用标准的 JDK 动态代理(dynamic proxy)技术来实现 AOP 代理, 通过它, 我们可以为任意的接口实现代理.
如果需要为一个类实现代理, 那么可以使用 CGLIB 代理. 当一个业务逻辑对象没有实现接口时, 那么Spring AOP 就默认使用 CGLIB 来作为 AOP 代理了. 即如果我们需要为一个方法织入 Advice , 但是这个方法不是一个接口所提供的方法, 则此时 Spring AOP 会使用 CGLIB 来实现动态代理. 鉴于此, Spring AOP 建议基于接口编程, 对接口进行 AOP 而不是类.

彻底理解 aspect, join point, point cut, advice
看完了上面的理论部分知识, 我相信还是会有不少朋友感觉到 AOP 的概念还是很模糊, 对 AOP 中的各种概念理解的还不是很透彻. 其实这很正常, 因为 AOP 中的概念是在是太多了, 我当时也是花了老大劲才梳理清楚的.
下面我以一个简单的例子来比喻一下 AOP 中 aspect, jointpoint, Pointcut 与 Advice 之间的关系.

让我们来假设一下, 从前有一个叫爪哇的小县城, 在一个月黑风高的晚上, 这个县城中发生了命案. 作案的凶手十分狡猾, 现场没有留下什么有价值的线索. 不过万幸的是, 刚从隔壁回来的老王恰好在这时候无意中发现了凶手行凶的过程, 但是由于天色已晚, 加上凶手蒙着面, 老王并没有看清凶手的面目, 只知道凶手是个男性, 身高约七尺五寸. 爪哇县的县令根据老王的描述, 对守门的士兵下命令说: 凡是发现有身高七尺五寸的男性, 都要抓过来审问. 士兵当然不敢违背县令的命令, 只好把进出城的所有符合条件的人都抓了起来.

来让我们看一下上面的一个小故事和 AOP 到底有什么对应关系.
首先我们知道, 在 Spring AOP 中 Join Point指代的是所有方法的执行点, 而Point Cut是一个描述信息, 它修饰的是Join Point, 通过 Point Cut, 我们就可以确定哪些 Join Point 可以被织入 Advice . 对应到我们在上面举的例子, 我们可以做一个简单的类比, Join Point 就相当于 爪哇的小县城里的百姓, Point Cut就相当于 老王所做的指控, 即凶手是个男性, 身高约七尺五寸, 而 Advice 则是施加在符合老王所描述的嫌疑人的动作: 抓过来审问.
为什么可以这样类比呢?

Join Point –>

爪哇的小县城里的百姓: 因为根据定义,Join Point 是所有可能被织入 Advice 的候选的点, 在 Spring AOP中, 则可以认为所有方法执行点都是 Join Point. 而在我们上面的例子中, 命案发生在小县城中, 按理说在此县城中的所有人都有可能是嫌疑人.

Point Cut –>

男性, 身高约七尺五寸: 我们知道, 所有的方法(JoinPoint) 都可以织入 Advice , 但是我们并不希望在所有方法上都织入 Advice , 而 Pointcut 的作用就是提供一组规则来匹配JoinPoint, 给满足规则的 JoinPoint 添加 Advice . 同理, 对于县令来说, 他再昏庸, 也知道不能把县城中的所有百姓都抓起来审问, 而是根据凶手是个男性, 身高约七尺五寸, 把符合条件的人抓起来. 在这里 凶手是个男性, 身高约七尺五寸 就是一个修饰谓语, 它限定了凶手的范围, 满足此修饰规则的百姓都是嫌疑人, 都需要抓起来审问.

advice –>

抓过来审问, Advice 是一个动作, 即一段 Java 代码, 这段 Java 代码是作用于Point Cut 所限定的那些Join Point 上的. 同理, 对比到我们的例子中, 抓过来审问 这个动作就是对作用于那些满足 男性, 身高约七尺五寸 的爪哇的小县城里的百姓.
aspect: aspect 是 Point Cut 与 Advice 的组合, 因此在这里我们就可以类比: “根据老王的线索, 凡是发现有身高七尺五寸的男性, 都要抓过来审问” 这一整个动作可以被认为是一个 aspect.
或则我们也可以从语法的角度来简单类比一下.我们在学英语时, 经常会接触什么 定语, 被动句 之类的概念, 那么可以做一个不严谨的类比, 即 Join Point 可以认为是一个 宾语, 而 Pointcut 则可以类比为修饰 Join Point 的定语, 那么整个 aspect 就可以描述为: 满足 Pointcut 规则的Join Point 会被添加相应的 Advice 操作.

增强(Advice ，另译为通知，但《3.x》作者不赞成)：在特定连接点执行的动作。



### 3.我们为什么要使用AOP？

我们可以利用AOP的思想来对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

在Spring AOP中业务逻辑仅仅只关注业务本身，将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中划分出来，通过对这些行为的分离，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候不影响业务逻辑的代码。

AOP相当于一个拦截器，去拦截一些处理，例如：当一个方法执行的时候，Spring 能够拦截正在执行的方法，在方法执行的前或者后增加额外的功能和处理，就是我们希望通过使用AOP来对我们的代码进行耦合度的降低 把原本杂乱交错的关系给分离开来 进而改变这些行为的时候不会因为杂乱的关系互相影响。

AOP应该怎样使用？
老规矩导依赖

 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
            <scope>test</scope>
        </dependency>
定义 aspect(切面)
@Aspect:作用是把当前类标识为一个切面供容器读取

如果一个类被打上了@Aspect就代表着他是一个切面类

当使用注解 @Aspect 标注一个 Bean 后, 那么 Spring 框架会自动收集这些 Bean, 并添加到 Spring AOP 中, 例如:

@Aspect//定义该类为切面
@Component
public class AspectDemo
{
}
注意, 仅仅使用@Aspect 注解, 并不能将一个 Java 对象转换为 Bean, 因此我们还需要使用类似 @Component 之类的注解.




注意, 如果一个 类被@Aspect 标注, 则这个类就不能是其他 aspect 的 advised object（目标对象） 了, 因为使用 @Aspect 后, 这个类就会被排除在 auto-proxying（自动代理） 机制之外.

声明 Pointcut （切入点）
一个 Pointcut 的声明由两部分组成:

一个方法签名, 包括方法名和相关参数
一个 Pointcut 表达式, 用来指定哪些方法执行是我们感兴趣的(即因此可以织入 Advice ).

package com.example.javalogframe.text;


import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;


@Aspect//定义该类为切面
@Component
public class AspectDemo
{

    @Pointcut("execution(* com.example.javalogframe.text.Jui_LogDemo.*(..))")//切点表达式
    public void aspectPointcut()
    {
        
    }

}

这个方法必须无返回值.

这个方法本身就是 Pointcut signature(签名), Pointcut 表达式使用@Pointcut 注解指定.
上面我们简单地定义了一个 Pointcut , 这个 Pointcut 所描述的是: 匹配所有在包 com.example.javalogframe.text.Jui_LogDemo 下的所有方法的执行.

切点标志符(designator)

AspectJ5 的切点表达式由标志符(designator)和操作参数组成. 如 “execution(* greetTo(..))” 的切点表达式, execution 就是 标志符, 而圆括号里的 * greetTo(..) 就是操作参数

execution(执行)

匹配 Join Point 的执行, 例如 “execution(* hello(..))” 表示匹配所有目标类中的 hello() 方法. 这个是最基本的 Pointcut 标志符.

within(内)

匹配特定包下的所有 Join Point, 例如 within(com.xys.*) 表示 com.xys 包中的所有连接点, 即包中的所有类的所有方法. 而 within(com.xys.demo2.*Service) 表示在 com.xys.demo2包中所有以 Service 结尾的类的所有的连接点.


@Pointcut("within(com.xys.demo2.*)")
public void pointcut2() {
}
this 与 target

this 的作用是匹配一个 bean, 这个 bean(Spring AOP proxy) 是一个给定类型的实例(instance of). 而 target 匹配的是一个目标对象(target object, 即需要织入 Advice的原始的类), 此对象是一个给定类型的实例(instance of).

bean

匹配 bean 名字为指定值的 bean 下的所有方法, 例如:

bean(*Service) // 匹配名字后缀为 Service 的 bean 下的所有方法
bean(myService) // 匹配名字为 myService 的 bean 下的所有方法

args

匹配参数满足要求的的方法.
例如:

切面类

package com.example.javalogframe.text;


import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;


@Aspect//定义该类为切面
@Component
public class AspectDemo
{

    @Pointcut("within(com.example.javalogframe.text.*)")//切点表达式
    public void aspectPointcut()
    {
     
    }
    
    @Before(value = "aspectPointcut() && args(name)")
    public void outName(String name){
        System.out.println("OUTNAME:"+name);
    }

 









}

方法类

package com.example.javalogframe.text;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Component
public class AspectText

{
    private static final Logger l = LoggerFactory.getLogger(AspectText.class);

    public void a()
    {
        l.info("我出来咯表哥");
    }
     
    public  String b(String name){
        l.info("name:{}",name);
        return "可以咯";
    }

 









}

启动类

package com.example.javalogframe.text;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.util.logging.Logger;

@Component
public class Jui_LogDemo
{
    private static final Logger LOGGER = Logger.getLogger(Jui_LogDemo.class.getName());

    @Autowired
    AspectText aspectText;
     
    public  @PostConstruct void Log()
    {
        aspectText.b("帅");
    }

}

不能在mian方法中调用也不能在自身的类里面调用b类 那样不算从Spring 容器中拿到的对象 所以AOP就会失效

输出效果:

 .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.0)
11:29:52 [main] INFO  com.example.javalogframe.JavaLogframeApplication - Starting JavaLogframeApplication using Java 1.8.0_301 on DESKTOP-KSHCS0C with PID 27016 (E:\LHJ\Java练习\java-logframe\target\classes started by Administrator in E:\LHJ\Java练习\java-logframe)
11:29:52 [main] INFO  com.example.javalogframe.JavaLogframeApplication - No active profile set, falling back to 1 default profile: "default"
OUTNAME:帅
11:29:53 [main] INFO  com.example.javalogframe.text.AspectText - name:帅
11:29:53 [main] INFO  com.example.javalogframe.JavaLogframeApplication - Started JavaLogframeApplication in 0.663 seconds (JVM running for 1.319)
    _____ _    _   log服务启动成功       _   _  _____ _______    _  ____  _    _ 
  / ____| |  | |   /\   | \ | |/ ____|___  / |  | |/ __ \| |  | |
 | (___ | |__| |  /  \  |  \| | |  __   / /| |__| | |  | | |  | |
  \___ \|  __  | / /\ \ | . ` | | |_ | / / |  __  | |  | | |  | |
  ____) | |  | |/ ____ \| |\  | |__| |/ /__| |  | | |__| | |__| |
 |_____/|_|  |_/_/    \_\_| \_|\_____/_____|_|  |_|\____/ \____/ 

我的项目结构图



当 aspectText.b 执行时, 则 Advice outName()就会执行, test 方法的参数 name 就会传递到 outName中.

所以我在切面类里面用within是去拿到text包下的所有方法,然后我在通过@Before前置增强在用args参数匹配找到匹配参数的方法然后输出拿到的参数

常用例子:

// 匹配只有一个参数 name 的方法
@Before(value = "aspectMethod()  &&  args(name)")
public void doSomething(String name) {
}


// 匹配第一个参数为 name 的方法
@Before(value = "aspectMethod()  &&  args(name, ..)")
public void doSomething(String name) {
}


// 匹配第二个参数为 name 的方法

Before(value = "aspectMethod()  &&  args(*, name, ..)")
public void doSomething(String name) {
}

@annotation

匹配由指定注解所标注的方法, 例如:

@Pointcut("@annotation(com.xys.demo1.AuthChecker)") 
public void pointcut()
{ 
}
则匹配由注解 AuthChecker 所标注的方法.

常见的切点表达式

// 匹配指定包中的所有的方法
execution(* com.xys.service.*(..))

// 匹配当前包中的指定类的所有方法
execution(* UserService.*(..))

// 匹配指定包中的所有 public 方法
execution(public * com.xys.service.*(..))

// 匹配指定包中的所有 public 方法, 并且返回值是 int 类型的方法
execution(public int com.xys.service.*(..))

// 匹配指定包中的所有 public 方法, 并且第一个参数是 String, 返回值是 int 类型的方法
execution(public int com.xys.service.*(String name, ..))
匹配类型签名

// 匹配指定包中的所有的方法, 但不包括子包
within(com.xys.service.*)

// 匹配指定包中的所有的方法, 包括子包
within(com.xys.service..*)

// 匹配当前包中的指定类中的方法
within(UserService)


// 匹配一个接口的所有实现类中的实现的方法
within(UserDao+)
匹配 Bean 名字

// 匹配以指定名字结尾的 Bean 中的所有方法
bean(*Service)
切点表达式组合

// 匹配以 Service 或 ServiceImpl 结尾的 bean
bean(*Service || *ServiceImpl)

// 匹配名字以 Service 开头, 并且在包 com.xys.service 中的 bean
bean(*Service) && within(com.xys.service.*)

声明 Advice
Advice是和一个 Pointcut 表达式关联在一起的, 并且会在匹配的 Join Point 的方法执行的前/后/周围 运行. Pointcut 表达式可以是简单的一个 Pointcut 名字的引用, 或者是完整的 Pointcut 表达式.
下面我们以几个简单的 Advice 为例子, 来看一下一个 Advice 是如何声明的.

Before advice

/**
 * @author xiongyongshun
 * @version 1.0
 * @created 16/9/9 13:13
 */
    @Component
    @Aspect
    public class BeforeAspectTest {
    // 定义一个 Pointcut, 使用 切点表达式函数 来描述对哪些 Join point 使用 advise.
    @Pointcut("execution(* com.xys.service.UserService.*(..))")
    public void dataAccessOperation() {
    }
    }
    @Component
    @Aspect
    public class AdviseDefine {
    // 定义 advise
    @Before("com.xys.aspect.PointcutDefine.dataAccessOperation()")
    public void doBeforeAccessCheck(JoinPoint joinPoint) {
        System.out.println("*****Before advise, method: " + joinPoint.getSignature().toShortString() + " *****");
    }
    }

这里, @Before 引用了一个 Pointcut , 即 “com.xys.aspect.PointcutDefine.dataAccessOperation()” 是一个 Pointcut 的名字.


如果我们在 Advice 在内置 Pointcut , 则可以:

@Component
@Aspect
public class AdviseDefine {
    // 将 pointcut 和 advice 同时定义
    @Before("within(com.xys.service..*)") 
    publicvoiddoAccessCheck(JoinPoint joinPoint) 
    { 
        System.out.println("*****doAccessCheck, Before advise, method: " + joinPoint.getSignature().toShortString() + " *****");
    } 
}
around advice

around Advice 比较特别, 它可以在一个方法的之前之前和之后添加不同的操作, 并且甚至可以决定何时, 如何, 是否调用匹配到的方法.

在看环绕增强时我们需要先了解两个对象 joinpoint(连接点) 和 proceedingjoinpoint(进行连接点)

因为我们前面了解到 joinpoint 是连接点的意思 所以 JointPoint对象则包含了和切入相关的很多信息。比如切入点的对象，方法，属性等。我们可以通过反射的方式获取这些点的状态和信息，用于追踪tracing和记录logging应用信息。

而proceedingjoinpoint 继承了 JoinPoint。是在JoinPoint的基础上暴露出 proceed 这个方法。proceed很重要，这个是aop代理链执行的方法。

JointPoint和ProceedingJoinPoint区别？
JointPoint

通过JpointPoint对象可以获取到下面信息

返回目标对象，即被代理的对象

Object getTarget();

返回切入点的参数

Object[] getArgs();

返回切入点的Signature

Signature getSignature();

返回切入的类型，比如method-call，field-get等等，感觉不重要 

 String getKind();
ProceedingJoinPoint

Proceedingjoinpoint 继承了 JoinPoint。是在JoinPoint的基础上暴露出 proceed 这个方法。proceed很重要，这个是aop代理链执行的方法。

环绕通知=前置+目标方法执行+后置通知，proceed方法就是用于启动目标方法执行的

暴露出这个方法，就能支持 aop:around 这种切面（而其他的几种切面只需要用到JoinPoint，，这也是环绕通知和前置、后置通知方法的一个最大区别。这跟切面类型有关）， 能决定是否走代理链还是走自己拦截的其他逻辑。建议看一下 JdkDynamicAopProxy的invoke方法，了解一下代理链的执行原理。

JointPoint使用详解
这里详细介绍JointPoint的方法，这部分很重要是coding核心参考部分。开始之前我们思考一下，我们到底需要获取切入点的那些信息。我的思考如下

切入点的方法名字及其参数
切入点方法标注的注解对象（通过该对象可以获取注解信息）
切入点目标对象（可以通过反射获取对象的类名，属性和方法名）
注：有一点非常重要，Spring的AOP只能支持到方法级别的切入。换句话说，切入点只能是某个方法。

针对以上的需求JDK提供了如下API



1 获取切入点所在目标对象

// 获取切入点所在目标对象
Object targetObj =joinPoint.getTarget();

//可以发挥反射的功能获取关于类的任何信息，例如获取类名如下
  String className = joinPoint.getTarget().getClass().getName();
因为一个类有很多方法，为了获取具体切入点所在的方法可以通过如下API

2.获取切入点方法的名字

getSignature());是获取到这样的信息 :修饰符+ 包名+组件名(类名) +方法名

这里我只需要方法名

String methodName = joinPoint.getSignature().getName()
3. 获取方法上的注解

方法1：xxxxxx是注解名字

Signature signature = joinPoint.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();

        if (method != null)
        {
            xxxxxx annoObj= method.getAnnotation(xxxxxx.class);
        }
        return null;
方法2：上面我们已经知道了方法名和类的对象，通过反射可以获取类的内部任何信息。

// 切面所在类
        Object target = joinPoint.getTarget();

        String methodName = joinPoint.getSignature().getName();
     
        Method method = null;
        for (Method m : target.getClass().getMethods()) {
            if (m.getName().equals(methodName)) {
                method = m;
               //  xxxxxx annoObj= method.getAnnotation(xxxxxx.class);同上
                break;
            }
        }
4. 获取方法的参数

这里返回的是切入点方法的参数列表

Object[] args = joinPoint.getArgs();
测试
眼见为实，测试一遍可以理解更深刻

注解类

@Target({ ElementType.PARAMETER, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ApiLog
{
    /**
     * 模块 
     */
    public String title() default "";

    /**
     * 日志记录service实现
     * @return
     */
    public String logService() default "operLogServiceImpl";

 








    /**
     * 是否保存请求的参数
     */
    public boolean isSaveRequestData() default true;
     
    /**
     * 是否追踪用户操作
     * @return
     */
    public boolean isTrack() default true;
}

切面类

package com.kouryoushine.aop.test;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.stereotype.Component;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

@Aspect
@Component
public class DemoAspect {

    //切入点：aopdemo报下所有对象的save方法
    @Pointcut("execution(public * com.kouryoushine.aop.test.*.save*(..))")
    public void save(){
     
    }
    /**
     * 需要在update操作前后分别获取更新前后的值
     * @param
     * @return
     */
     
    @AfterReturning("save()")
    public void afterReturn(JoinPoint joinPoint) throws IllegalAccessException, NoSuchMethodException, InvocationTargetException {
     
        //1.获取切入点所在目标对象
        Object targetObj =joinPoint.getTarget();
        System.out.println(targetObj.getClass().getName());
        // 2.获取切入点方法的名字
        String methodName = joinPoint.getSignature().getName();
        System.out.println("切入方法名字："+methodName);
        // 3. 获取方法上的注解
        Signature signature = joinPoint.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();
     
        if (method != null)
        {
           ApiLog apiLog=  method.getAnnotation(ApiLog.class);
            System.out.println("切入方法注解的title:"+apiLog.title());
        }
     
        //4. 获取方法的参数
        Object[] args = joinPoint.getArgs();
        for(Object o :args){
            System.out.println("切入方法的参数："+o);
        }

 








    }

 









}

服务类

@Service
public class TestServcie {

    @ApiLog(title = "注解的标题",isSaveRequestData = false)
    public void save(String parm1,int parm2){
        System.out.println("执行目标对象的方法"+parm1+parm2);
    }

 








    public void  update(){
        System.out.println("没有注解的方法，不会被拦截");
    }
}
测试方法

  @Autowired
    TestServcie testServcie;
    @Test
    void  test6() throws Exception{

        testServcie.save("参数1字符串",33);
    }
测试结果

com.kouryoushine.aop.test.TestServcie

切入方法名字：save

切入方法注解的title:注解的标题

切入方法的参数：参数1字符串

切入方法的参数：33

所以现在我们知道了joinpoint 对象的作用 可以获取切入点的消息

而proceedingjoinpoint 仅支持环绕增强建议

因为proceedingjoinpoint和joinpoint不同的暴露出 proceed 这个方法。proceed很重要，这个是aop代理链执行的方法。

@Component
@Aspect
public class AdviseDefine {
    // 定义 advise
    @Around("com.xys.aspect.PointcutDefine.dataAccessOperation()")
    public Object doAroundAccessCheck(ProceedingJoinPoint pjp) throws Throwable {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        // 开始
        Object retVal = pjp.proceed();
        stopWatch.stop();
        // 结束
        System.out.println("invoke method: " + pjp.getSignature().getName() + ", elapsed time: " + stopWatch.getTotalTimeMillis());
        return retVal;
    }
}

Advice(通知、切面)： 某个连接点所采用的处理逻辑，也就是向连接点注入的代码， AOP在特定的切入点上执行的增强处理。意思就是Advice是一个动作, 即一段 Java 代码, 这段 Java 代码是作用于 Point Cut 所限定的那些 Join Point 上的

相关注解介绍：

@Around：环绕增强，相当于MethodInterceptor

@AfterReturning：后置增强，相当于AfterReturningAdvice，方法正常退出时执行

@Before：标识一个前置增强方法，相当于BeforeAdvice的功能，相似功能的还有

@AfterThrowing：异常抛出增强，相当于ThrowsAdvice

@After: final增强，不管是抛出异常或者正常退出都会执行

1.@Before（目标方法执行之前）

在Advice中内容是int，div调用之前输出的，因而用到了@Before

创建新的java类ComputerAop，并添加@Aspect与@Component注释。

public class ComputerAop{
	
	//目标方法执行之前
	@Before("execution(public int xxx.xx.xxxxx.xxxxx.ComputerService.*(..))")
	//xxx ComputerService前的路径。它会自动寻找ComputerService中所有public int的方法。
	public void before(JoinPoint jp) {
		Object[] args=jp.getArgs();
	//获取目标方法对应参数
		Signature sg=jp.getSignature();
	//获取目标方法
		String name=sg.getName();
		System.out.println(this.getClass().getName()+":the "+name+" method begins.");
		System.out.println(this.getClass().getName()+":parameters of the "+name+" method["+args[0]+","+args[1]+"].");
	}
}
执行结果如下



2.@After（目标方法执行完，无论是否出现错误异常，都会执行）

@After("execution(public int xxx.xx.xxxxx.xxxxx.ComputerService.*(..))")
	public void after(JoinPoint jp){
		Signature sg=jp.getSignature();
		String name=sg.getName();
		System.out.println(this.getClass().getName()+":The "+name+"method ends");
	}
执行结果如下



3.@AfterReturning（目标方法返回结果时，出现错误异常，不会执行）

@AfterReturning(value="("execution(public int xxx.xx.xxxxx.xxxxx.ComputerService.*(..))")",returning="result")
	public void afterReturning(JoinPoint jp,Object result) {
		Signature sg=jp.getSignature();
		String name=sg.getName();
		System.out.println(this.getClass().getName()+"the result of :"+name+" is "+result);
	}
当div方法传参数（1,1）时，return结果无措，正常执行。



当div方法传参数（1,0）时，return结果有措，不执行。

4.@AfterThrowing（目标方法出现错误异常执行）

@AfterThrowing(value="execution(public int xxx.xx.xxxxx.xxxxx.ComputerService.*(..))",throwing="e")
	public void afterThrowing(JoinPoint jp,Exception e) {
		System.out.println(e.getMessage());
	}
结果如下，打印出了错误。



5.@Around（可以实现以上所有功能）

@Around(value = "execution(public int xxx.xx.xxxxx.xxxxx.ComputerService.*(..))")
public object around(ProceedingJoinPoint pjp) {
    object [] args = pjp.getArgs();//传入目标方法的参数
    signature sg = pjp.getSignature();
    String name = sg.getName();
    System.out .print1n( this. getClass().getName()+": The "+name+" method begins.");
    System.out.println(this.getClass().getName()+": Parameters of the "+name+" method: [" +args[0]+"," +args[1]+"]");
try {
    try {
        object object = pjip. getTarget();//目标类创建的对象
        System.out.print1n("*******" +object. getClass() . getName());
        object result =pjp. proceed();//调用目标方法，并且返回目标方法的结果
        System.out.println(this.getClass() .getName()+": Result of the "+name+" method: " +result);
        return result;
}finally {
    System.out.println(this.getClass().getName()+": The "+name+" method ends.");
} catch (Throwable e) {
    System.out.println(e.getMessage();|
    return -1;
}

我们为什么要使用AOP?
举个例子，你想给你的网站加上鉴权，

对某些url，你认为不需要鉴权就可以访问，

对于某些url，你认为需要有特定权限的用户才能访问

如果你依然使用OOP，面向对象，

那你只能在那些url对应的Controller代码里面，一个一个写上鉴权的代码而如果你使用了AOP呢？

那就像使用Spring Security进行安全管理一样简单（更新：Spring Security的拦截是基于Servlet的Filter的，不是aop，不过两者在使用方式上类似）：

protected void configure(HttpSecurity http) throws Exception {     
    http         
        .authorizeRequests()            
        .antMatchers("/static","/register").permitAll() 
        .antMatchers("/user/**").hasRoles("USER", "ADMIN")
这样的做法，对原有代码毫无入侵性，这就是AOP的好处了，把和主业务无关的事情，放到代码外面去做。


所以当你下次发现某一行代码经常在你的Controller里出现，比如方法入口日志打印，那就要考虑使用AOP来精简你的代码了。


AOP是OOP的有益补充


基于Java语言的web开发，本质是用面向对象的组织，面向过程的逻辑，来解决问题。应用实践中灵活具体，不拘泥，不教条。

总结： Spring实现的AOP是代理模式，给调用者使用的实际是已经过加工的对象，你编程时方法体里只写了A，但调用者拿到的对象的方法体却是xAy。x和y总还是需要你来写的，这就是增强。x和y具体在什么时候被调用总还是需要你来规定的，虽然是基于约定的声明这种简单的规定，这就是切点。
