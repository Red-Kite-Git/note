# Jackson	(com.fasterxml.jackson.annotation)

`com.fasterxml.jackson.annotation` 包提供了一系列注解，这些注解可以用来对 Java 对象和 JSON 数据之间的序列化（将 Java 对象转换为 JSON 数据）和反序列化（将 JSON 数据转换为 Java 对象）过程进行精细控制。下面为你介绍其中一些常用的注解：

### 1. `@JsonProperty`

此注解用于指定 Java 对象属性和 JSON 字段之间的映射关系。当 Java 属性名与 JSON 字段名不一致时，就能使用这个注解来建立映射。

```java
import com.fasterxml.jackson.annotation.JsonProperty;

public class User {
    @JsonProperty("user_name")
    private String name;
    private int age;

    // Getters 和 Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```



在上述代码里，`@JsonProperty("user_name")` 表明 `name` 属性在 JSON 中对应的字段名为 `user_name`。

### 2. `@JsonIgnore`

该注解用于在序列化和反序列化时忽略某个 Java 属性，也就是该属性不会出现在生成的 JSON 数据中，同时在反序列化时也会被忽略。

```java
import com.fasterxml.jackson.annotation.JsonIgnore;

public class User {
    private String name;
    @JsonIgnore
    private String password;

    // Getters 和 Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```



在这个例子中，`password` 属性在序列化和反序列化时会被忽略。

### 3. `@JsonInclude`

此注解用于指定在序列化时，哪些属性值可以被包含在生成的 JSON 数据中。

```java
import com.fasterxml.jackson.annotation.JsonInclude;

@JsonInclude(JsonInclude.Include.NON_NULL)
public class User {
    private String name;
    private String email;

    // Getters 和 Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

在这个示例中，`@JsonInclude(JsonInclude.Include.NON_NULL)` 表明只有非 `null` 的属性才会被包含在生成的 JSON 数据中。

### 4. `@JsonFormat`

该注解用于指定日期、时间等类型的属性在序列化和反序列化时的格式。

```java
import com.fasterxml.jackson.annotation.JsonFormat;
import java.util.Date;

public class Event {
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
    private Date eventDate;

    // Getters 和 Setters
    public Date getEventDate() {
        return eventDate;
    }

    public void setEventDate(Date eventDate) {
        this.eventDate = eventDate;
    }
}
```

在这个例子中，`@JsonFormat` 注解指定了 `eventDate` 属性在序列化和反序列化时的日期时间格式以及时区。

### 5. `@JsonAutoDetect`

此注解用于控制哪些属性可以被自动检测并进行序列化和反序列化。

```java
import com.fasterxml.jackson.annotation.JsonAutoDetect;

@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.ANY)
public class User {
    private String name;
    private int age;

    // 不需要 Getters 和 Setters
}
```

在这个示例中，`@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.ANY)` 表示所有字段都可以被自动检测并进行序列化和反序列化，即使没有提供对应的 Getter 和 Setter 方法。



这些注解能够让你更加灵活地控制 Java 对象和 JSON 数据之间的转换，以满足不同的业务需求。