# Spring

## 一、Spring简介

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

## 二、搭建开发环境

### 1.开发环境

IDEA

### 2.创建项目

加入Jar包

### 3.创建Spring的配置文件

Spring分模块开发的配置

+ 加载配置文件时，直接加载多个配置文件

```java
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext1.xml", "applicationContext2.xml");
```

+ 在一个配置文件中引入多个配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--引入配置文件-->
	<import resource="applicationContext2.xml"/>
    
</beans>
```



## 三、控制反转IoC/依赖注入DI

### 1. IoC（Inversion of Controll）

控制反转是一种思想

将对象的创建权利交出去，交给第三方容器负责
将对象和对象之间的关系维护权交出去，交给第三方容器负责

**思想是反转资源获取的方向**，传统的资源查找方式要求组件向容器发起请求查找资源。作为回应，容器适时的返回资源。而应用了IOC之后，则是**容器主动的将资源推送给它所管理的组件，组件所要做的仅是选择一种合适的方式来接收资源**

### 2. DI(Dependency Injection)

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

Spring的属性注入方式

#### 构造方法方式的属性注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--构造方法方式的属性注入-->
    <bean id="car" class="learningspring.ioc.examples.demo3.Car">
        <constructor-arg name="name" value="BWM"/>
        <constructor-arg name="price" value="800000"/>
    </bean>
</beans>
```

#### Set方法方式的属性注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--Set方法方式的属性注入-->
    <bean id="dog" class="learningspring.ioc.examples.demo3.Dog">
        <property name="name" value="Golden"/>
        <property name="length" value="100"/>
    </bean>
</beans>
```

#### 为Bean注入引用类型的数据

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--构造方法方式的属性注入-->
    <bean id="car" class="learningspring.ioc.examples.demo3.Car">
        <constructor-arg name="name" value="BWM"/>
        <constructor-arg name="price" value="800000"/>
    </bean>

    <!--Set方法方式的属性注入-->
    <bean id="dog" class="learningspring.ioc.examples.demo3.Dog">
        <property name="name" value="Golden"/>
        <property name="length" value="100"/>
    </bean>

    <!--为Bean注入对象属性-->
    <!--构造方法方式一样可行-->
    <bean id="employee" class="learningspring.ioc.examples.demo3.Employee">
        <property name="name" value="Chen"/>
        <property name="car" ref="car"/>
        <property name="dog" ref="dog"/>
    </bean>
</beans>
```

#### P名称空间的属性注入（Spring2.5）

+ 通过引入p名称空间完成属性注入
  + 普通属性：p:属性名=“值”
  + 对象属性：p:属性名-ref=“值”

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--P名称空间的属性注入-->
    <bean id="cat" class="learningspring.ioc.examples.demo3.Cat" p:name="Orange" p:length="100"/>
    
    <!--为Bean注入对象属性-->
    <bean id="employee" class="learningspring.ioc.examples.demo3.Employee" p:cat-ref="cat">
        <property name="name" value="Chen"/>
        <property name="car" ref="car"/>
        <property name="dog" ref="dog"/>
    </bean>
</beans>
```

#### 不同类型数据的注入

### 注入集合类型的数据

```java
/**
 * 注入集合类型的数据测试
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class CollectionBean {

    private String[] strs;
    private List<String> list;
    private Set<String> set;
    private Map<String, String> map;

    public void setStrs(String[] strs) {
        this.strs = strs;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setSet(Set<String> set) {
        this.set = set;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }

    @Override
    public String toString() {
        return "CollectionBean{" +
                "strs=" + Arrays.toString(strs) +
                ", list=" + list +
                ", set=" + set +
                ", map=" + map +
                '}';
    }
}

```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--Spring的集合属性的注入-->
    <!--注入数组类型-->
    <bean id="collectionBean" class="learningspring.ioc.examples.demo4.CollectionBean">
        <!-- 注入数组类型 -->
        <property name="strs">
            <list>
                <value>Tom</value>
                <value>Jack</value>
            </list>
        </property>

        <!-- 注入List集合 -->
        <property name="list">
            <list>
                <value>Lucy</value>
                <value>Lily</value>
            </list>
        </property>

        <!-- 注入Set集合 -->
        <property name="set">
            <set>
                <value>aaa</value>
                <value>bbb</value>
                <value>ccc</value>
            </set>
        </property>

        <!-- 注入Map集合 -->
        <property name="map">
            <map>
                <entry key="a" value="1"/>
                <entry key="b" value="2"/>
            </map>
        </property>
    </bean>
</beans>
```

### 5.基于注解管理bean

Spring开发中的常用注解

#### @Component

该注解在类上使用，使用该注解就相当于在配置文件中配置了一个Bean，例如：

```java
@Component("userDao")
public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("UserDaoImpl.save");
    }
}
```

相当于以下内容：

```xml
<bean id="userDao" class="learningspring.ioc.examplesannotation.demo1.UserDaoImpl"></bean>
```

该注解有3个衍生注解：

+ **@Controller：修饰Web 层类**
+ **@Service：修饰Service层类**
+ **@Repository：修饰Dao层类**

#### @Value

该注解用于给属性注入值，有2种用法

+ 如果有set方法，则需要将该注解添加到set方法上
+ 如果没有set方法，则需要将该注解添加到属性上

```java
/**
 * Value 注解用于属性注入
 * 当类有提供set方法时添加在set方法上
 * 如果没有，则添加到属性上
 *
 * @author Chen Rui
 * @version 1.0
 **/

@Component("dog")
public class Dog {
    private String name;

    @Value("100") // 注入属性值
    private Double length;

    public Dog() {
    }

    public Dog(String name, Double length) {
        this.name = name;
        this.length = length;
    }

    public String getName() {
        return name;
    }

    @Value("Golden") // 注入属性值
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", length=" + length +
                '}';
    }
}

```

#### @Autowired

`@Value` 通常用于普通属性的注入。

`@Autowired` 通常用于为对象类型的属性注入值，但是按照**类型**完成属性注入

习惯是按照**名称**完成属性注入，所以必须让`@Autowired`注解和`@Qualifier`注解**一同使用**。

（如果没有`@Qualifier`注解，修改以下例子中`@Repository`注解的值，也能编译成功）

```java
@Service("userService")
public class UserServiceImpl implements UserService {

    @Autowired
    @Qualifier("userDao")
    private UserDao userDao;

    @Override
    public void save() {
        System.out.println("UserServiceImpl.save");
        userDao.save();
    }
}
```

```java
@Repository("userDao")
public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("UserDaoImpl.save");
    }
}
```

#### @Resource

该注解也可以用于属性注入，通常情况下使用**@Resource注解**，默认按照**名称**完成属性注入。

该注解由J2EE提供，需要导入包`javax.annotation.Resource`。

`@Resource`有两个重要的属性：`name`和`type`，而Spring将`@Resource`注解的`name`属性解析为bean的名字，而`type`属性则解析为bean的类型。所以，如果使用`name`属性，则使用byName的自动注入策略，而使用`type`属性时则使用byType自动注入策略。如果既不制定`name`也不制定`type`属性，这时将通过反射机制使用byName自动注入策略。

```java
/**
 * UserController
 *
 * @author Chen Rui
 * @version 1.0
 **/
@Controller("userController")
public class UserController {
    
    @Resource(name = "userService")
    private UserService userService; 
    
}
```

```java
/**
 * UserService实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/

@Service("userService")
public class UserServiceImpl implements UserService {

	@Resource(name = "userDao")
    private UserDao userDao;

    @Override
    public void save() {
        System.out.println("UserServiceImpl.save");
        userDao.save();
    }
}

```

```java
/**
 * UserDao实现类
 * @author Chen Rui
 * @version 1.0
 **/

@Component("userDao")
public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("UserDaoImpl.save");
    }
}

```

#### @PostConstruct 和 @PreDestroy

`@PostConstruct`和`@PreDestroy`注解，前者为Bean生命周期相关的注解，在配置文件中，使用到了i`nit-method`参数来配置Bean初始化之前要执行的方法，该注解也是同样的作用。将该注解添加到想在初始化之前执行的目标方法上，就可以实现该功能，而后者则是Bean生命周期中`destroy-method`目标方法，使用该注解在指定方法上，即可实现在Bean销毁之前执行该方法。

```java
/**
 * UserDao实现类
 * @author Chen Rui
 * @version 1.0
 **/

@Component("userDao")
public class UserDaoImpl implements UserDao {
    
    @PostConstruct
    public void init(){
        System.out.println("UserDaoImpl.init");
    }
    
    @Override
    public void save() {
        System.out.println("UserDaoImpl.save");
    }
    
    @PreDestroy
    public void destroy(){
        System.out.println("UserDaoImpl.destroy");
    }
}
```

#### @Scope

Bean的作用范围的注解，默认为singleton（单例）

可选值：

+ singleton
+ prototype
+ request
+ session
+ globalsession

```java
/**
 * UserDao实现类
 * @author Chen Rui
 * @version 1.0
 **/

@Component("userDao")
@Scope // 默认为singleton
public class UserDaoImpl implements UserDao {

    @PostConstruct
    public void init(){
        System.out.println("UserDaoImpl.init");
    }

    @Override
    public void save() {
        System.out.println("UserDaoImpl.save");
    }

    @PreDestroy
    public void destroy(){
        System.out.println("UserDaoImpl.destroy");
    }
}
```

### 6.XML管理与注解管理对比

基于XML配置和基于注解配置的对比

|                | 基于XML的配置                                                | 基于注解的配置                                               |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Bean的定义     | \<bean id="Bean的id" class="类的全路径"/>                    | @Component或衍生注解@Controller，@Service和@Repository       |
| Bean的名称     | 通过id或name指定                                             | @Component(“Bean的id”)                                       |
| Bean的属性注入 | \<property>或者通过p命名空间                                 | 通过注解@Autowired 按类型注入<br />通过@Qualifier按名称注入  |
| Bean的生命周期 | init-method指定Bean初始化前执行的方法，destroy-method指定Bean销毁前执行的方法 | @PostConstruct 对应于int-method<br />@PreDestroy 对应于destroy-method |
| Bean的作用域   | 在bean标签中配置scope属性                                    | @Scope, 默认是singleton<br />配置多例可以在目标类上使用@Scope(“prototype”) |
| 使用场景       | Bean来自第三方，可以使用在任何场景                           | Bean的实现类由自己维护                                       |

XML可以适用于任何场景，就算Bean来自第三方也可以适用XML方式来管理。而注解方式就无法在此场景下使用，注解方式可以让开发的过程更加方便，但前提是Bean由自己维护，这样才能在源码中添加注解。

所以可以使用**两者混合**的方式来开发项目，使用**XML配置文件来管理Bean，使用注解来进行属性注入**

## 四、面向切面AOP

### 1.介绍

即**面向切面编程**，通过**预编译**方式和运行期动态代理实现程序功能的统一维护的一种技术。利用AOP可以对业务逻辑的各个部分进行**隔离**，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序的**可重用性**，同时提高了开发的效率。

### 2.相关术语

#### joinpoint(连接点) 
 可以被拦截到的点。save(), query(),update(),delete()方法都可以增强，这些方法就可以称为连接点。

#### pointcut(切入点)
真正被拦截到的点。在实际开发中，可以只对save()方法进行增强，那么save()方法就是切入点。

#### advice(增强)
方法层面的增强，现在可以对save()方法进行权限校验，权限校验(checkPri())的方法称为增强。

#### introduction(引介)
类层面的增强。

#### target(目标)
被增强的对象。

#### weaving(织入)
将增强(advice)应用到目标(target)的过程

#### proxy(代理)
代理对象，被增强以后的代理对象

#### aspect(切面)
多个增强(advice)和多个切入点(pointcut)的组合

### 3.基于XML配置实现

#### AspectJ的XML配置案例

首先创建一个接口`ProductDao`，在里面定义添加商品，查询商品，修改商品，删除商品方法。

```java
/**
 * ProductDao
 *
 * @author Chen Rui
 * @version 1.0
 **/
public interface ProductDao {

    /**
     * 添加商品
     */
    void save();

    /**
     * 删除商品
     */
    void delete();

    /**
     * 修改商品
     */
    void modify();

    /**
     * 查询商品
     */
    void query();
}

```

接着创建一个类`ProductDaoImpl`来实现该接口

```java
/**
 * ProductDao的实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductDaoImpl implements ProductDao {

    @Override
    public void save() {
        System.out.println("添加商品");
    }

    @Override
    public void delete() {
        System.out.println("删除商品");
    }
    
    @Override
    public void modify() {
        System.out.println("修改商品");
    }
    
    @Override
    public void query() {
        System.out.println("查询商品");
    }
    
}

```

现在目的就是给`save()`方法进行增强，使得在调用`save()`方法前进行权限校验。

要实现此功能，先创建一个**增强类**，或者叫**切面类**。里面编写要增强的功能，例如权限校验。

创建增强类`ProductEnhancer`

```java
/**
 * ProductDao的增强类(切面类)
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductEnhancer {

    public void checkPri(){
        System.out.println("【前置增强】权限校验");
    }

}

```

然后创建配置文件`aspectj-xml.xml`来配置，该文件名此案例仅用于演示，实际开发中不要采取此名，依据实际需求编写。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置目标对象，即被增强的对象 -->
    <bean id="productDao" class="learningspring.aop.aspectj.xml.demo2.ProductDaoImpl"/>

    <!-- 将增强类(切面类)交给Spring管理 -->
    <bean id="productEnhancer" class="learningspring.aop.aspectj.xml.demo2.ProductEnhancer"/>
    
    <!-- 通过对AOP的配置完成对目标对象产生代理 -->
    <aop:config>
        <!-- 表达式配置哪些类的哪些方法需要进行增强 -->
        <!-- 对ProductDaoImpl类中的save方法进行增强 -->
        <!--
        “*” 表示任意返回值类型
        “..” 表示任意参数
        -->
        <aop:pointcut id="pointcut1" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.save(..))"/>

        <!-- 配置切面 -->
        <aop:aspect ref="productEnhancer">
            <!-- 前置增强 -->
            <!-- 实现在调用save方法之前调用checkPri方法来进行权限校验-->
            <aop:before method="checkPri" pointcut-ref="pointcut1"/>
        </aop:aspect>
    </aop:config>
    
</beans>
```

至此切入点及切面都已配置完成，编写测试类和方法

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

/**
 * AspectJ的XML方式配置测试类
 *
 * @author Chen Rui
 * @version 1.0
 **/

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:aspectj-xml.xml")
public class AppTest {

    @Resource(name = "productDao")
    private ProductDao productDao;

    @Test
    public void test(){
        // 对save方法进行增强
        productDao.save();

        productDao.delete();
        
        productDao.modify();
        
        productDao.query();
    }
}

```

运行`test()`方法，控制台打印结果如下：

```
【前置增强】权限校验
添加商品
删除商品
修改商品
查询商品
```

至此就实现了在不修改`ProductDaoImpl`类的情况下，对其中的`save()`方法进行增强。

#### Spring中常用的增强类型

##### 前置增强

在目标方法执行之前执行，可以获得切入点的信息

修改之前的`ProductEnhancer`类的`checkPri()`方法的参数。

```java
import org.aspectj.lang.JoinPoint;

/**
 * ProductDao的增强类(切面类)
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductEnhancer {

    public void checkPri(JoinPoint joinPoint){
        System.out.println("【前置增强】权限校验" + joinPoint);
    }

}
```

执行测试方法，控制台输出

```
【前置增强】权限校验execution(void learningspring.aop.aspectj.xml.demo2.ProductDao.save())
添加商品
删除商品
修改商品
查询商品
```



##### 后置增强

在目标方法执行之后执行，可以获得方法的返回值

首先修改`ProductDao`中的`delete()`方法的返回值类型，改成String

```java
/**
 * ProductDao
 *
 * @author Chen Rui
 * @version 1.0
 **/
public interface ProductDao {

    /**
     * 添加商品
     */
    void save();

    /**
     * 删除商品
     */
    String delete();

    /**
     * 修改商品
     */
    void modify();

    /**
     * 查询商品
     */
    void query();
}

```

再修改`ProductDaoImpl`中的`delete()`方法

```java
/**
 * ProductDao的实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductDaoImpl implements ProductDao {

    @Override
    public void save() {
        System.out.println("添加商品");
    }

    @Override
    public String delete() {
        System.out.println("删除商品");
        return new Date().toString();
    }

    @Override
    public void modify() {
        System.out.println("修改商品");
    }

    @Override
    public void query() {
        System.out.println("查询商品");
    }
}

```

修改`ProductEnhancer`类，添加`writeLog()`方法，实现写日志功能

```java
/**
 * ProductDao的增强类(切面类)
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductEnhancer {

    /**
     * 前置增强案例
     * 在调用save方法之前进行权限校验
     * @param joinPoint 切入点对象
     */
    public void checkPri(JoinPoint joinPoint){
        System.out.println("【前置增强】权限校验" + joinPoint);
    }

    /**
     * 后置增强案例
     * 在调用delete方法之后，写入日志记录操作时间
     * @param result 目标方法返回的对象
     */
    public void writeLog(Object result){
        System.out.println("【后置增强】写入日志 操作时间：" + result.toString());
    }
}
```

然后修改`aspectj.xml`配置文件，配置新的**切入点**和**切面**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置目标对象，即被增强的对象 -->
    <bean id="productDao" class="learningspring.aop.aspectj.xml.demo2.ProductDaoImpl"/>

    <!-- 将增强类(切面类)交给Spring管理 -->
    <bean id="productEnhancer" class="learningspring.aop.aspectj.xml.demo2.ProductEnhancer"/>
    
    <!-- 通过对AOP的配置完成对目标对象产生代理 -->
    <aop:config>
        <!-- 表达式配置哪些类的哪些方法需要进行增强 -->
        <!-- 对ProductDaoImpl类中的save方法进行增强 -->
        <!--
        “*” 表示任意返回值类型
        “..” 表示任意参数
        -->
        <!-- 前置增强的切入点配置 -->
        <aop:pointcut id="pointcut1" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.save(..))"/>
        
        <!-- 后置增强的切入点配置 -->
        <aop:pointcut id="pointcut2" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.delete(..))"/>

        <!-- 配置切面 -->
        <aop:aspect ref="productEnhancer">
            <!-- 前置增强 -->
            <!-- 实现在调用save方法之前调用checkPri方法来进行权限校验-->
            <aop:before method="checkPri" pointcut-ref="pointcut1"/>
            
            <!-- 后置增强 -->
            <!-- returning里面的值必须和writeLog()方法里的参数名相同，本案例为result-->
            <aop:after-returning method="writeLog" returning="result" pointcut-ref="pointcut2"/>
        </aop:aspect>
    </aop:config>

</beans>
```

执行测试方法，控制台打印结果

```
【前置增强】权限校验execution(void learningspring.aop.aspectj.xml.demo2.ProductDao.save())
添加商品
删除商品
【后置增强】写入日志 操作时间：Tue Mar 19 15:59:48 CST 2019
修改商品
查询商品
```

##### 环绕增强

在目标方法执行之前和之后都执行

利用环绕增强来实现在调用`modify()`方法前后进行性能监控

首先修改`ProductEnhancer`类，添加`monitor()`方法

```java
/**
 * ProductDao的增强类(切面类)
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductEnhancer {

    /**
     * 前置增强案例
     * 在调用save方法之前进行权限校验
     * @param joinPoint 切入点对象
     */
    public void checkPri(JoinPoint joinPoint){
        System.out.println("【前置增强】权限校验" + joinPoint);
    }

    /**
     * 后置增强案例
     * 在调用delete方法之后，写入日志记录操作时间
     * @param result 目标方法返回的对象
     */
    public void writeLog(Object result){
        System.out.println("【后置增强】写入日志 操作时间：" + result.toString());
    }

    /**
     * 环绕增强
     * 在调用modify方法前后，显示性能参数
     * @param joinPoint 切入点对象
     * @throws Throwable 可抛出的异常
     */
    public Object monitor(ProceedingJoinPoint joinPoint) throws Throwable{
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        Object obj = joinPoint.proceed();
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        return obj;
    }
}

```

然后再修改`aspectj.xml`配置文件，添加新的**切入点**和**切面**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置目标对象，即被增强的对象 -->
    <bean id="productDao" class="learningspring.aop.aspectj.xml.demo2.ProductDaoImpl"/>

    <!-- 将增强类(切面类)交给Spring管理 -->
    <bean id="productEnhancer" class="learningspring.aop.aspectj.xml.demo2.ProductEnhancer"/>
    
    <!-- 通过对AOP的配置完成对目标对象产生代理 -->
    <aop:config>
        <!-- 表达式配置哪些类的哪些方法需要进行增强 -->
        <!-- 对ProductDaoImpl类中的save方法进行增强 -->
        <!--
        “*” 表示任意返回值类型
        “..” 表示任意参数
        -->
        <!-- 前置增强的切入点配置 -->
        <aop:pointcut id="pointcut1" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.save(..))"/>

        <!-- 后置增强的切入点配置 -->
        <aop:pointcut id="pointcut2" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.delete(..))"/>

        <!-- 环绕增强的切入点配置 -->
        <aop:pointcut id="pointcut3" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.modify(..))"/>

        <!-- 配置切面 -->
        <aop:aspect ref="productEnhancer">
            <!-- 前置增强 -->
            <!-- 实现在调用save方法之前调用checkPri方法来进行权限校验-->
            <aop:before method="checkPri" pointcut-ref="pointcut1"/>

            <!-- 后置增强 -->
            <!-- returning里面的值必须和writeLog()方法里的参数名相同，本案例为result-->
            <aop:after-returning method="writeLog" returning="result" pointcut-ref="pointcut2"/>

            <!-- 环绕增强 -->
            <aop:around method="monitor" pointcut-ref="pointcut3"/>

        </aop:aspect>
    </aop:config>

</beans>
```

运行测试方法，控制台打印结果：

```
【前置增强】权限校验execution(void learningspring.aop.aspectj.xml.demo2.ProductDao.save())
添加商品
删除商品
【后置增强】写入日志 操作时间：Tue Mar 19 15:58:49 CST 2019
【环绕增强】当前空闲内存185MB
修改商品
【环绕增强】当前空闲内存185MB
查询商品
```

##### 异常抛出增强

在程序出现异常时执行

利用异常抛出增强来实现获取异常信息的功能

首先修改`ProductDaoImpl`中的`query()`方法，使该方法抛出异常

```java
/**
 * ProductDao的实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductDaoImpl implements ProductDao {

    @Override
    public void save() {
        System.out.println("添加商品");
    }

    @Override
    public void query() {
        System.out.println("查询商品");
        int a = 1/0;
    }

    @Override
    public void modify() {
        System.out.println("修改商品");
    }

    @Override
    public String delete() {
        System.out.println("删除商品");
        return new Date().toString();
    }
}
```

接着修改`ProductEnhancer`类，添加`exception()`方法

```java
/**
 * ProductDao的增强类(切面类)
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductEnhancer {

    /**
     * 前置增强案例
     * 在调用save方法之前进行权限校验
     * @param joinPoint 切入点对象
     */
    public void checkPri(JoinPoint joinPoint){
        System.out.println("【前置增强】权限校验" + joinPoint);
    }

    /**
     * 后置增强案例
     * 在调用delete方法之后，写入日志记录操作时间
     * @param result 目标方法返回的对象
     */
    public void writeLog(Object result){
        System.out.println("【后置增强】写入日志 操作时间：" + result.toString());
    }

    /**
     * 环绕增强
     * 在调用modify方法前后，显示性能参数
     * @param joinPoint 切入点对象
     * @throws Throwable 可抛出的异常
     */
    public Object monitor(ProceedingJoinPoint joinPoint) throws Throwable{
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        Object obj = joinPoint.proceed();
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        return obj;
    }

    /**
     * 异常抛出增强
     * 在调用query时若抛出异常则打印异常信息
     * @param ex 异常对象
     */
    public void exception(Throwable ex){
        System.out.println("【异常抛出增强】" + "异常信息：" +ex.getMessage());
    }
}

```

然后再修改`aspectj-xml.xml`配置文件，添加新的**切入点**和**切面**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置目标对象，即被增强的对象 -->
    <bean id="productDao" class="learningspring.aop.aspectj.xml.demo2.ProductDaoImpl"/>

    <!-- 将增强类(切面类)交给Spring管理 -->
    <bean id="productEnhancer" class="learningspring.aop.aspectj.xml.demo2.ProductEnhancer"/>
    
    <!-- 通过对AOP的配置完成对目标对象产生代理 -->
    <aop:config>
        <!-- 表达式配置哪些类的哪些方法需要进行增强 -->
        <!-- 对ProductDaoImpl类中的save方法进行增强 -->
        <!--
        “*” 表示任意返回值类型
        “..” 表示任意参数
        -->
        <!-- 前置增强的切入点配置 -->
        <aop:pointcut id="pointcut1" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.save(..))"/>

        <!-- 后置增强的切入点配置 -->
        <aop:pointcut id="pointcut2" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.delete(..))"/>

        <!-- 环绕增强的切入点配置 -->
        <aop:pointcut id="pointcut3" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.modify(..))"/>

        <!-- 异常抛出增强的切入点配置 -->
        <aop:pointcut id="pointcut4" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.query(..))"/>

        <!-- 配置切面 -->
        <aop:aspect ref="productEnhancer">
            <!-- 前置增强 -->
            <!-- 实现在调用save方法之前调用checkPri方法来进行权限校验-->
            <aop:before method="checkPri" pointcut-ref="pointcut1"/>

            <!-- 后置增强 -->
            <!-- returning里面的值必须和writeLog()方法里的参数名相同，本案例为result-->
            <aop:after-returning method="writeLog" returning="result" pointcut-ref="pointcut2"/>

            <!-- 环绕增强 -->
            <aop:around method="monitor" pointcut-ref="pointcut3"/>

            <!-- 异常抛出增强 -->
            <aop:after-throwing method="exception" throwing="ex" pointcut-ref="pointcut4"/>
        </aop:aspect>
    </aop:config>

</beans>
```

最后执行测试方法，控制台输出结果：

```
【前置增强】权限校验execution(void learningspring.aop.aspectj.xml.demo2.ProductDao.save())
添加商品
删除商品
【后置增强】写入日志 操作时间：Tue Mar 19 15:58:16 CST 2019
【环绕增强】当前空闲内存183MB
修改商品
【环绕增强】当前空闲内存183MB
查询商品
【异常抛出增强】异常信息：/ by zero
```

##### 最终增强

无论代码是否有异常最终都会执行

继续在异常抛出增强的代码修改，实现无论是否抛出异常都会打印当前时间信息

首先修改`ProductEnhancer`类，添加`finallyAdvice()`方法

```java
/**
 * ProductDao的增强类(切面类)
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductEnhancer {

    /**
     * 前置增强案例
     * 在调用save方法之前进行权限校验
     * @param joinPoint 切入点对象
     */
    public void checkPri(JoinPoint joinPoint){
        System.out.println("【前置增强】权限校验" + joinPoint);
    }

    /**
     * 后置增强案例
     * 在调用delete方法之后，写入日志记录操作时间
     * @param result 目标方法返回的对象
     */
    public void writeLog(Object result){
        System.out.println("【后置增强】写入日志 操作时间：" + result.toString());
    }

    /**
     * 环绕增强
     * 在调用modify方法前后，显示性能参数
     * @param joinPoint 切入点对象
     * @throws Throwable 可抛出的异常
     */
    public Object monitor(ProceedingJoinPoint joinPoint) throws Throwable{
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        Object obj = joinPoint.proceed();
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        return obj;
    }

    /**
     * 异常抛出增强
     * 在调用query时若抛出异常则打印异常信息
     * @param ex 异常对象
     */
    public void exception(Throwable ex){
        System.out.println("【异常抛出增强】" + "异常信息：" +ex.getMessage());
    }

    /**
     * 最终增强
     * 无论query方法是否抛出异常都打印当前时间
     */
    public void finallyAdvice(){
        System.out.println("【最终增强】" + new Date().toString());
    }
}

```

修改`aspectj.xml`配置文件，添加新的**切面**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置目标对象，即被增强的对象 -->
    <bean id="productDao" class="learningspring.aop.aspectj.xml.demo2.ProductDaoImpl"/>

    <!-- 将增强类(切面类)交给Spring管理 -->
    <bean id="productEnhancer" class="learningspring.aop.aspectj.xml.demo2.ProductEnhancer"/>
    
    <!-- 通过对AOP的配置完成对目标对象产生代理 -->
    <aop:config>
        <!-- 表达式配置哪些类的哪些方法需要进行增强 -->
        <!-- 对ProductDaoImpl类中的save方法进行增强 -->
        <!--
        “*” 表示任意返回值类型
        “..” 表示任意参数
        -->
        <!-- 前置增强的切入点配置 -->
        <aop:pointcut id="pointcut1" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.save(..))"/>

        <!-- 后置增强的切入点配置 -->
        <aop:pointcut id="pointcut2" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.delete(..))"/>

        <!-- 环绕增强的切入点配置 -->
        <aop:pointcut id="pointcut3" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.modify(..))"/>

        <!-- 异常抛出增强的切入点配置 -->
        <aop:pointcut id="pointcut4" expression="execution(* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.query(..))"/>

        <!-- 配置切面 -->
        <aop:aspect ref="productEnhancer">
            <!-- 前置增强 -->
            <!-- 实现在调用save方法之前调用checkPri方法来进行权限校验-->
            <aop:before method="checkPri" pointcut-ref="pointcut1"/>

            <!-- 后置增强 -->
            <!-- returning里面的值必须和writeLog()方法里的参数名相同，本案例为result-->
            <aop:after-returning method="writeLog" returning="result" pointcut-ref="pointcut2"/>

            <!-- 环绕增强 -->
            <aop:around method="monitor" pointcut-ref="pointcut3"/>

            <!-- 异常抛出增强 -->
            <aop:after-throwing method="exception" throwing="ex" pointcut-ref="pointcut4"/>

            <!-- 最终增强 -->
            <aop:after method="finallyAdvice" pointcut-ref="pointcut4"/>
        </aop:aspect>
    </aop:config>

</beans>
```

最后运行测试代码，控制台输出结果：

```
【前置增强】权限校验execution(void learningspring.aop.aspectj.xml.demo2.ProductDao.save())
添加商品
删除商品
【后置增强】写入日志 操作时间：Tue Mar 19 15:57:01 CST 2019
【环绕增强】当前空闲内存183MB
修改商品
【环绕增强】当前空闲内存183MB
查询商品
【最终增强】Tue Mar 19 15:57:01 CST 2019
【异常抛出增强】异常信息：/ by zero
```

#### AOP切入点表达式语法

AOP切入点表达式是基于execution的函数完成的

语法：**[访问修饰符] 方法返回值 包名.类名.方法名(参数)**

“*” 表示任意返回值类型
“..” 表示任意参数

+ `public void learningspring.aop.aspectj.xml.demo2.ProductDaoImpl.save(..) `：具体到某个增强的方法
+ `* *.*.*.*Dao.save(..) `：所有包下的所有以Dao结尾的类中的save方法都会被增强
+ `* learningspring.aop.aspectj.xml.demo2.ProductDaoImpl+.save(..) `：ProductDaoImpl及其子类的save方法都会被增强
+ `* learningspring.aop.aspectj.xml..*.*(..)`：xml包及其子包的所有类的方法都会被增强

#### AspectJ的注解配置案例

首先也是创建一个接口`ProductDao`

```java
/**
 * ProductDao接口
 *
 * @author Chen Rui
 * @version 1.0
 **/
public interface ProductDao {

    /**
     * 添加商品
     */
    void save();

    /**
     * 查询商品
     */
    void query();

    /**
     * 修改商品
     */
    void modify();

    /**
     * 删除商品
     */
    String delete();
}
```

然后创建一个Dao实现类`ProductDaoImpl`

```java
/**
 * ProductDao的实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class ProductDaoImpl implements ProductDao {

    @Override
    public void save() {
        System.out.println("添加商品");
    }

    @Override
    public String delete() {
        System.out.println("删除商品");
        return new Date().toString();
    }

    @Override
    public void modify() {
        System.out.println("修改商品");
    }

    @Override
    public void query() {
        System.out.println("查询商品");
        int a = 1/0;
    }
}
```

接着创建**增强类**`ProductEnhancer`，在该类里面使用注解

使用`@Pointcut`注解可以配置切入点信息，在较多的方法都要使用同一个增强时，就可以配置一个切入点让目标方法都去引用

`@Before`：前置增强

`@AfterReturning`：后置增强，其中的returning的值必须和方法传入的参数名相同

`@Around`：环绕增强

`@AfterThrowing`：异常抛出增强，其中的throwing的值必须和方法传入的参数名相同

`@After`：最终增强

```java
/**
 * ProductDao的增强类(切面类)
 *
 * @author Chen Rui
 * @version 1.0
 **/
@Aspect
public class ProductEnhancer {

    /**
     * 切入点配置
     * 对ProductDaoImpl里的方法都增强
     */
    @Pointcut(value = "execution(* learningspring.aop.aspectj.annotation.demo2.ProductDaoImpl.*(..))")
    private void pointcut1(){}

    /**
     * 前置增强案例
     * 在调用save方法之前进行权限校验
     * @param joinPoint 切入点对象
     */
    @Before(value = "execution(* learningspring.aop.aspectj.annotation.demo2.ProductDaoImpl.save())")
    public void checkPri(JoinPoint joinPoint){
        System.out.println("【前置增强】权限校验" + joinPoint);
    }

    /**
     * 后置增强案例
     * 在调用delete方法之后，写入日志记录操作时间
     * @param result 目标方法返回的对象
     */
    @AfterReturning(returning = "result", value = "execution(* learningspring.aop.aspectj.annotation.demo2.ProductDaoImpl.delete())")
    public void writeLog(Object result){
        System.out.println("【后置增强】写入日志 操作时间：" + result.toString());
    }

    /**
     * 环绕增强
     * 在调用modify方法前后，显示性能参数
     * @param joinPoint 切入点对象
     * @throws Throwable 可抛出的异常
     */
    @Around(value = "execution(* learningspring.aop.aspectj.annotation.demo2.ProductDaoImpl.modify())")
    public Object monitor(ProceedingJoinPoint joinPoint) throws Throwable{
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        Object obj = joinPoint.proceed();
        System.out.println("【环绕增强】当前空闲内存" + Runtime.getRuntime().freeMemory()/(1024 * 1024) + "MB");
        return obj;
    }

    /**
     * 异常抛出增强
     * 在调用query时若抛出异常则打印异常信息
     * @param ex 异常对象
     */
    @AfterThrowing(throwing = "ex", value = "execution(* learningspring.aop.aspectj.annotation.demo2.ProductDaoImpl.query())")
    public void exception(Throwable ex){
        System.out.println("【异常抛出增强】" + "异常信息：" +ex.getMessage());
    }

    /**
     * 最终增强
     * 无论ProductDaoImpl里的每个方法是否抛出异常都打印当前时间
     */
    @After(value = "pointcut1()")
    public void finallyAdvice(){
        System.out.println("【最终增强】" + new Date().toString());
    }
}

```

编写测试方法

```java
/**
 * AspectJ的注解方式配置测试类
 *
 * @author Chen Rui
 * @version 1.0
 **/

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:aspectj-annotation.xml")
public class AppTest {

    @Resource(name = "productDao")
    private ProductDao productDao;

    @Test
    public void test(){

        productDao.save();

        productDao.delete();

        productDao.modify();

        productDao.query();
    }
}
```

运行，控制台输出

```
【前置增强】权限校验execution(void learningspring.aop.aspectj.annotation.demo2.ProductDao.save())
添加商品
【最终增强】Tue Mar 19 16:01:06 CST 2019
删除商品
【最终增强】Tue Mar 19 16:01:06 CST 2019
【后置增强】写入日志 操作时间：Tue Mar 19 16:01:06 CST 2019
【环绕增强】当前空闲内存186MB
修改商品
【环绕增强】当前空闲内存186MB
【最终增强】Tue Mar 19 16:01:06 CST 2019
查询商品
【最终增强】Tue Mar 19 16:01:06 CST 2019
【异常抛出增强】异常信息：/ by zero
```





### 4.基于注解方式实现

## 五、Spring事务管理

什么是事务

事务：逻辑上的一组操作，组成这组操作的各个单元，要么全部成功，要么全部失败。

事务的特性

+ 原子性：事务不可分割
+ 一致性：事务执行前后数据完整性保持一致
+ 隔离性：一个事务的执行不应该受到其他事务的干扰
+ 持久性：一旦事务结束，数据就持久化到数据库

不考虑隔离性引发的安全性问题

+ 读问题
  + 脏读：A事务读到B事务未提交的数据
  + 不可重复读：B事务在A事务两次读取数据之间，修改了数据，导致A事务两次读取结果不一致
  + 幻读/虚读：B事务在A事务批量修改数据时，插入了一条新的数据，导致数据库中仍有一条数据未被修改。
+ 写问题
  + 丢失更新：

解决读问题

+ 设置事务的隔离级别
  + `Read uncommitted`：未提交读，任何读问题都解决不了
  + `Read committed`：已提交读，解决脏读，但是不可重复读和幻读有可能发生
  + `Repeatable read`：重复读，解决脏读和不可重复读，但是幻读有可能发生
  + `Serializable`：解决所有读问题，因为禁止并行执行

### Spring事务管理API

+ `PlatformTransactionManager`：平台事务管理器

  + `DataSourceTransactionManager`：底层使用JDBC管理事务

+ `TransactionDefinition`：事务定义信息

  ​	用于定义事务相关的信息，隔离级别，超时信息，传播行为，是否只读……

+ `TransactionStatus`：事务的状态

  ​	用于记录在事务管理过程中，事务的状态

API的关系：

Spring在进行事务管理的时候，首先**平台事务管理器**根据**事务定义信息**进行事务的管理，在事务管理过程中，产生各种状态，将这些状态信息记录到**事务状态对象**

Spring事务的传播行为

首先假设一个背景，Service1里的x()方法已经定义了一个事务，Service2里的y()方法也有一个事务，但现在新增一行代码在Service2的y()方法中要先调用Service1里的x()方法然后再执行本身的方法。这时就涉及到**事务的传播行为**。

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321110709.png)

Spring中提供了7种传播行为

**假设x()方法称为A，y()方法称为B**

+ 保证多个操作在同一个事务中
  + **`PROPAGATION_REQUIRED`**(\*)：Spring事务隔离级别的默认值。如果A中有事务，则使用A中的事务。如果没有，则创建一个新的事务，将操作包含进来。
  + `PROPAGATION_SUPPORTS`：支持事务。如果A中有事务，使用A中的事务。如果A没有事务，则不使用事务。
  + `PROPAGATION_MANDATORY`：如果A中有事务，使用A中的事务。如果没有事务，则抛出异常。
+ 保证多个事务不在同一个事务中
  + **`PROPAGATION_REQUIRES_NEW`**(\*)：如果A中有事务，将A的事务挂起，创建新事务，只包含自身操作。如果A中没有事务，创建一个新事物，包含自身操作。
  + `PROPAGATION_NOT_SUPPORTED`：如果A中有事务，将A的事务挂起，不使用事务。
  + `PROPAGATION_NEVER`：如果A中有事务，则抛出异常。
+ 嵌套式事务
  + **`PROPAGATION_NESTED`**(\*)：嵌套事务，如果A中有事务，则按照A的事务执行，执行完成后，设置一个保存点，再执行B中的操作，如果无异常，则执行通过，如果有异常，则可以选择回滚到初始位置或者保存点。

Spring事务管理案例——转账情景

转账情景实现

首先创建接口`AccountDao`，定义两个方法分别是`out`和`in`

```java
/**
 * AccountDao
 *
 * @author Chen Rui
 * @version 1.0
 **/

public interface AccountDao {

    /**
     * 转出
     *
     * @param from  转出账户
     * @param money 转出金额
     */
    void out(String from, double money);

    /**
     * 转入
     *
     * @param to    转入账户
     * @param money 转入金额
     */
    void in(String to, double money);
}
```

接着创建实现类`AccountDaoImpl`实现`out`和`in`方法并且继承`JdbcSupport`类。这样就可以直接使用父类的`JDBCTemplate`对象。

```java
/**
 * AccountDao实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao {

    @Override
    public void out(String from, double money) {
        this.getJdbcTemplate().update("UPDATE account SET money = money - ? WHERE name = ?", money, from);
    }

    @Override
    public void in(String to, double money) {
        this.getJdbcTemplate().update("UPDATE account SET money = money + ? WHERE name = ?", money, to);
    }
}

```

然后创建接口`AccountrService`，定义`transfer`方法

```java
/**
 * AccountService
 *
 * @author Chen Rui
 * @version 1.0
 **/
public interface AccountService {

    /**
     * 转账
     * @param from 转出账户
     * @param to 转入账户
     * @param money 交易金额
     */
    void transfer(String from, String to, Double money);
}

```

再创建类`AccountServiceImpl`实现该接口，并声明`AccountDao`引用并创建`set`方法

```java
/**
 * AccountService实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class AccountServiceImpl implements AccountService {

    private AccountDao accountDao;

    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }

    @Override
    public void transfer(String from, String to, Double money) {
        accountDao.out(from, money);
        accountDao.in(to, money);
    }
}
```

最后创建配置文件`spring-tx-programmatic.xml`，用来管理Bean。

引入数据库连接文件，配置数据源，创建Bean对象`accountDao`将数据源`dataSource`注入到`accountDao`中，再创建Bean对象`accountService`，将`accountDao`注入。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="                                             
            http://www.springframework.org/schema/beans  
            http://www.springframework.org/schema/beans/spring-beans.xsd  
            http://www.springframework.org/schema/context   
            http://www.springframework.org/schema/context/spring-context.xsd  
            http://www.springframework.org/schema/tx 
            http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 编程式事务管理配置文件 -->

    <!-- 配置Service -->
    <bean id="accountService" class="learningspring.transaction.programmatic.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>

    <!-- 配置Dao -->
    <bean id="accountDao" class="learningspring.transaction.programmatic.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 引入数据库配置文件 -->
    <context:property-placeholder location="db.properties"/>

    <!-- 配置C3P0连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driverClassName}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```

到此一个转账模拟业务就实现了，数据库表依然使用前面创建的`account`表，先查询当前数据库的数据。

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321124514.png)

编写测试方法，实现让姓名为Bob的账户向Jack转账1000元。

```java
/**
 * 编程式事务测试类
 *
 * @author Chen Rui
 * @version 1.0
 **/
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(value = "classpath:spring-tx-programmatic.xml")
public class AppTest {

    @Resource(name = "accountService")
    private AccountService accountService;

    @Test
    public void test(){
        accountService.transfer("Bob","Jack",1000d);
    }
}
```

运行结果

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321124630.png)

现在对类`AccountServiceImpl`里的`transfer`方法进行修改，让其发生异常，再观察结果

```java
/**
 * AccountService实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class AccountServiceImpl implements AccountService {

    private AccountDao accountDao;

    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }

    @Override
    public void transfer(String from, String to, Double money) {
        accountDao.out(from, money);
        // 抛出异常
        int i = 1/0;
        accountDao.in(to, money);
    }

}

```

查询数据库数据

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321125027.png)

这时Bob账户的钱就少了1000元，而Jack账户也没有增加1000元。

所以就需要事务来进行管理。

### 编程式事务

所谓编程式事务，就是要在源码中编写事务相关的代码。实现编程式事务，首先要在`AccountServiceImpl`中声明`TransactionTemplate`对象，并创建set方法。然后修改`transfer`参数列表所有参数都用`final`(因为使用了匿名内部类)修饰，并修改方法体内容。

```java
/**
 * AccountService实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class AccountServiceImpl implements AccountService {

    private AccountDao accountDao;

    private TransactionTemplate transactionTemplate;

    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }
    public void setTransactionTemplate(TransactionTemplate transactionTemplate) {
        this.transactionTemplate = transactionTemplate;
    }

    @Override
    public void transfer(final String from, final String to, final Double money) {
        transactionTemplate.execute(new TransactionCallbackWithoutResult() {
            @Override
            protected void doInTransactionWithoutResult(TransactionStatus status) {
                accountDao.out(from, money);
                // 抛出异常
                int i = 1/0;
                accountDao.in(to,money);
            }
        });
    }
}
```

然后修改`spring-tx-programmatic.xml`文件，创建Bean对象`transactionManager`和`transactionTemplate`，并将`transactionTemplate`注入到`accountService`中。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="                                             
            http://www.springframework.org/schema/beans  
            http://www.springframework.org/schema/beans/spring-beans.xsd  
            http://www.springframework.org/schema/context   
            http://www.springframework.org/schema/context/spring-context.xsd  
            http://www.springframework.org/schema/tx 
            http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 编程式事务管理配置文件 -->

    <!-- 配置Service -->
    <bean id="accountService" class="learningspring.transaction.programmatic.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="transactionTemplate" ref="transactionTemplate"/>
    </bean>

    <!-- 配置Dao -->
    <bean id="accountDao" class="learningspring.transaction.programmatic.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 引入数据库配置文件 -->
    <context:property-placeholder location="db.properties"/>

    <!-- 配置C3P0连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driverClassName}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 配置模板 -->
    <bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
        <property name="transactionManager" ref="transactionManager"/>
    </bean>
</beans>
```

此时异常依然存在，数据库数据仍然是上次执行的结果状态

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321125027.png)

再次运行测试方法，并查询结果，观察是否发生变化

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321130039.png)

现在就实现了编程式事务，当出现异常时，数据库的数据就不会被修改。

### 声明式事务

#### XML配置方式

声明式事务即通过配置文件实现，利用的就是Spring的AOP

修改类`AccountServiceImpl`，删除`TransactionTemplate`对象，并修改`transfer`方法，保留异常代码

```java
/**
 * AccountService实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
public class AccountServiceImpl implements AccountService{

    private AccountDao accountDao;

    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }

    @Override
    public void transfer(String from, String to, Double money) {
        accountDao.out(from, money);
        int i = 1/0;
        accountDao.in(to,money);

    }
}
```

然后创建配置文件`spring-tx-declarative.xml`，配置数据源即Bean对象，然后配置事务管理器。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/tx
            http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 声明式事务管理配置文件 -->

    <!-- 配置Service -->
    <bean id="accountService" class="learningspring.transaction.declarative.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>

    <!-- 配置Dao -->
    <bean id="accountDao" class="learningspring.transaction.declarative.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 引入数据库配置文件 -->
    <context:property-placeholder location="db.properties"/>

    <!-- 配置C3P0连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driverClassName}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

接着就配置事务的增强，配置文件中加入以下配置

```xml
<!-- 配置事务的增强 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!-- 配置事务的规则 根据实际业务修改-->
        <tx:method name="*" propagation="REQUIRED"/>
    </tx:attributes>
</tx:advice>

<!-- AOP的配置 -->
<aop:config>
    <aop:pointcut id="pointcut1" expression="execution(* learningspring.transaction.declarative.AccountServiceImpl.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut1"/>
</aop:config>
```

先查看当前数据库数据

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321130039.png)

编写测试方法

```java
/**
 * 声明式事务配置测试类
 *
 * @author Chen Rui
 * @version 1.0
 **/
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(value = "classpath:spring-tx-declarative.xml")
public class AppTest {

    @Resource(name = "accountService")
    private AccountService accountService;

    @Test
    public void test(){
        accountService.transfer("Bob","Jack",1000d);
    }
}
```

运行查看结果，是否变化

![](https://blogpictrue-1251547651.cos.ap-chengdu.myqcloud.com/blog/20190321132512.png)

至此就实现了声明式事务XML方式的配置。

#### 注解配置方式

Spring的事务配置仍然支持注解配置

继续沿用`spring-tx-declarative.xml`文件，把事务增强和AOP相关的配置注释，并开启注解事务。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/tx
            http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd">


    <!-- 声明式事务管理配置文件 -->

    <!-- 配置Service -->
    <bean id="accountService" class="learningspring.transaction.declarative.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>

    <!-- 配置Dao -->
    <bean id="accountDao" class="learningspring.transaction.declarative.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 引入数据库配置文件 -->
    <context:property-placeholder location="db.properties"/>

    <!-- 配置C3P0连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driverClassName}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    
    <!-- 配置事务的增强 -->
    <!--<tx:advice id="txAdvice" transaction-manager="transactionManager">-->
        <!--<tx:attributes>-->
            <!-- 配置事务的规则 -->
            <!--<tx:method name="*" propagation="REQUIRED"/>-->
        <!--</tx:attributes>-->
    <!--</tx:advice>-->

    <!-- AOP的配置 -->
    <!--<aop:config>-->
        <!--<aop:pointcut id="pointcut1" expression="execution(* learningspring.transaction.declarative.AccountServiceImpl.*(..))"/>-->
        <!--<aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut1"/>-->
    <!--</aop:config>-->
    
    <tx:annotation-driven transaction-manager="transactionManager"/>
</beans>
```

接下来就可以在业务层类上使用事务管理的注解了。修改`AccountServiceImpl`类，添加`@Transactional`注解

```java
/**
 * AccountService实现类
 *
 * @author Chen Rui
 * @version 1.0
 **/
@Transactional(rollbackFor = Exception.class)
public class AccountServiceImpl implements AccountService{

    private AccountDao accountDao;

    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }


    @Override
    public void transfer(String from, String to, Double money) {
        accountDao.out(from, money);
        int i = 1/0;
        accountDao.in(to,money);

    }
}
```

再次运行测试方法，数据库也不会发生改变。
