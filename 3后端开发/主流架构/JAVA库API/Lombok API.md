# Lombok	(lombok)

Lombok 是一个 Java 库，能通过注解的方式减少 Java 代码中的样板代码（如 getter、setter、构造函数等），从而让代码更加简洁易读。以下为你详细介绍 Lombok 的相关内容：

### 主要特性和常用注解

#### 1. `@Getter` 和 `@Setter`

为类的字段自动生成 getter 和 setter 方法。

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class Person {
    private String name;
    private int age;
}
```

上述代码使用了 `@Getter` 和 `@Setter` 注解，编译后 `Person` 类会自动拥有 `name` 和 `age` 字段的 getter 和 setter 方法。

#### 2. `@ToString`

自动生成 `toString()` 方法。

```java
import lombok.ToString;

@ToString
public class Book {
    private String title;
    private String author;
}
```



使用 `@ToString` 注解后，`Book` 类会自动生成包含 `title` 和 `author` 信息的 `toString()` 方法。

#### 3. `@EqualsAndHashCode`

自动生成 `equals()` 和 `hashCode()` 方法。

```java
import lombok.EqualsAndHashCode;

@EqualsAndHashCode
public class Product {
    private String id;
    private String name;
}
```



`@EqualsAndHashCode` 注解会让 `Product` 类根据 `id` 和 `name` 字段生成 `equals()` 和 `hashCode()` 方法。

#### 4. `@NoArgsConstructor`、`@RequiredArgsConstructor` 和 `@AllArgsConstructor`

* `@NoArgsConstructor`：生成无参构造函数。
* `@RequiredArgsConstructor`：为类中所有带有 `final` 修饰符或 `@NonNull` 注解的字段生成构造函数。
* `@AllArgsConstructor`：生成包含所有字段的构造函数。

```java
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;
import lombok.RequiredArgsConstructor;

@NoArgsConstructor
@RequiredArgsConstructor
@AllArgsConstructor
public class Employee {
    private final String employeeId;
    private String name;
    private int age;
}
```



这个 `Employee` 类会有一个无参构造函数、一个包含 `employeeId` 的构造函数以及一个包含所有字段的构造函数。

#### 5. `@Data`

这是一个组合注解，相当于同时使用了 `@Getter`、`@Setter`、`@ToString`、`@EqualsAndHashCode` 和 `@RequiredArgsConstructor`。

```java
import lombok.Data;

@Data
public class Customer {
    private String customerId;
    private String name;
    private String email;
}
```



`@Data` 注解让 `Customer` 类自动拥有了 getter、setter、`toString()`、`equals()`、`hashCode()` 方法以及一个包含必需字段的构造函数。

#### 6. `@Builder`

用于为类生成建造者模式的代码。

```java
import lombok.Builder;

@Builder
public class Order {
    private String orderId;
    private String productName;
    private int quantity;
}
```



使用 `@Builder` 注解后，可以通过以下方式创建 `Order` 对象：

```java
Order order = Order.builder()
    .orderId("123")
    .productName("Phone")
    .quantity(2)
    .build();
```

### 使用步骤

#### 1. 添加依赖

如果你使用的是 Maven 项目，在 `pom.xml` 中添加以下依赖：

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.26</version>
    <scope>provided</scope>
</dependency>
```



若使用 Gradle 项目，则在 `build.gradle` 中添加：

```groovy
compileOnly 'org.projectlombok:lombok:1.18.26'
annotationProcessor 'org.projectlombok:lombok:1.18.26'
```

#### 2. 配置 IDE

为了让 IDE 能够识别 Lombok 注解并正确显示生成的代码，需要安装 Lombok 插件。以 IntelliJ IDEA 为例，在 `File` -> `Settings` -> `Plugins` 中搜索 `Lombok` 并安装。

### 优点和缺点

#### 优点

* **代码简洁**：减少了大量样板代码，使代码更易阅读和维护。
* **提高开发效率**：开发者无需手动编写 getter、setter 等方法，节省了时间。

#### 缺点

* **学习成本**：对于不熟悉 Lombok 的开发者来说，理解注解背后的生成逻辑可能需要一定时间。
* **调试难度**：由于部分代码是编译时生成的，调试时可能会增加一些难度。