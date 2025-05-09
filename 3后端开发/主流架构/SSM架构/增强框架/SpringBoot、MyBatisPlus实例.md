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



## 五、XXX



## 六、统一结果封装

接口的返回值类型不同、运行时异常等，导致前端获取的数据不统一难以处理

思路：后端接口返回数据,一般都是在控制器中直接返回一个JSON格式的数据，所以我们只需要在控制器返回数据的时候,将结果封装成Result类型的数据即可

### 1.正常结果封装

#### 创建自定义注解

如果控制器中的方法上存在该注解,表示此方法返回

的JSON格式的数据不需要在统一的封装到Result对象中。

```java
@Target({ElementType.METHOD}) 
@Retention(RetentionPolicy.RUNTIME) 
public @interface IgnoreResult{

}
```

#### 创建Result类

该Result类中存放服务器端返回的数据。

定义结果常量,包含正常的信息和错误的信息。

```java
@AllArgsConstructor
@Getter
public enum ResultConstant{
	ERROR(500,"抱歉服务器繁忙,请重试!!!"),
	SUCCESS(200,"SUCCESS"); int code;
	String message;
}
```

#### 定义结果封装对象

```
public class Result<T> implements Serializable {

 /**

 \* 返回的编码

 */

 private int code;

 /**

 \* 返回的信息

 */

 private String msg;

 /**

 \* 返回的数据

 */

 private T data;

 /**

 \* 处理成功

 \* @param data 返回的数据

 \* @return 返回Result对象

 */

 public static Result success(Object data) {

 int code = ResultConstant.SUCCESS.getCode();

 String message = ResultConstant.SUCCESS.getMessage();

 return Result.builder().code(code).msg(message).data(data).build();

 }

 /**

 \* 处理成功

 \* @return 返回Result对象

 */

 public static Result success() {

 return success(null);

 }

}
```

#### 创建ResultAdvice类

对服务器端返回的结果进行封装处理。判断控制器的方法上是否有IgnoreResult注解,如果有不将结果分装成

Result对象。如果没有将结果封装成Result对象。

> @ControllerAdvice 注解,它是一个Controller增强器,可对
>
> Controller进行增强处理。
>
> @ControllerAdvice("cn.kgc.controller")

```
public class ResultAdvice implements ResponseBodyAdvice<Object>{

 /**

 \* 如果supports方法的返回值是true,就会执行beforeBodyWrite方法。

 \* @param methodParameter 目标方法参数 * @param aClass 类型

 \* @return 是否支持

 */

 @Override

 public boolean supports(MethodParameter methodParameter,

 Class<? extends HttpMessageConverter<?>> aClass) {

 //获得方法的信息

 Method method = methodParameter.getMethod();

 //判断方法上是否有IgnoreResult注解

 if(method.isAnnotationPresent(IgnoreResult.class)){

 //如果方法上使用了IgnoreResult注解,则不进行封装

 return false;

 }

 return true;

 }

 /**

 \* 将控制器方法的返回值封装到Result对象中

 \* @param o 控制器中方法的返回值

 */

 @Override

 public Object beforeBodyWrite(Object o, MethodParameter

methodParameter, MediaType mediaType, Class<? extends

HttpMessageConverter<?>> aClass, ServerHttpRequest

serverHttpRequest, ServerHttpResponse serverHttpResponse) {

 if(o==null){

 return Result.success();

 }else if(o instanceof Result){

 return o;

 }else if (o instanceof String){

 return JSONUtil.toJsonStr(Result.success(o));

 }else{

 return Result.success(o);

 }

 }

}
```

### 2.统一异常处理

系统中如果出现了异常,我们不能直接将异常信息返回给前端。因为

这样的异常信息不利于前端进行处理,同时会暴露出一些系统信息。

我们要对系统中的异常进行统一的捕获处理,返回封装好的json格式

的错误信息。为了统一返回的结果,我们可以将系统中的异常信息封

装到Result对象中。

统一异常处理的思路

#### 使用@ControllerAdvice注解与@ExceptionHandler注解 捕获控制器中的异常信息,将异常信息封装到Result对象中,返回Result对象即可。创建自定义异常信息,需要注意的是HttpException异常是运行时异常。

```
@Getter

public class HttpException extends RuntimeException {

 /**

 \* 异常码

 */

 private final int code;

 public HttpException(ResultConstant resultConstant){

 super(resultConstant.getMessage());

 this.code=resultConstant.getCode();

 }

}

在Result类中添加将异常信息封装成Result对象的方法

 /**

 \* 处理失败

 \* @param httpException 异常

 \* @return Result对象

 */

 public static Result error(HttpException httpException){

 int code=httpException.getCode();

 String message=httpException.getMessage();

 return Result.builder().code(code).msg(message).data(null).build();

 }

 /**

 \* 处理失败

 \* @param exception 异常

 \* @return Result对象

 */

 public static Result error(Exception exception){

 return error(exception,null);

 }

 /**

 \* 处理失败

 \* @param exception 异常

 \* @param data 异常数据

 \* @return 异常对象

 */

 public static Result error(Exception exception,Object data){

 int code=ResultConstant.ERROR.getCode();

 String message=ResultConstant.ERROR.getMessage();

 return Result.builder().code(code).msg(message).data(data).build(); }
```

#### 在程序中使用ExcpetionAdvice类处理系统的异常信息,并将异常信息统一封装成Result对象。

```
@ControllerAdvice("cn.kgc.controller")

public class ExceptionAdvice {

 @ExceptionHandler(value = HttpException.class)

 public ResponseEntity handleHttpException(HttpException

httpException){

 //将异常信息封装到 Result对象中

 Result result= Result.error(httpException);

 return getResponseEntity(result);

 }

 @ExceptionHandler(value = Exception.class)

 public ResponseEntity handleException(Exception exception){

 Result result = Result.error(exception);

 return getResponseEntity(result);

 }

 private ResponseEntity getResponseEntity(Result result) {

 HttpStatus httpStatus=HttpStatus.OK;

 HttpHeaders headers=new HttpHeaders();

 headers.setContentType(MediaType.APPLICATION_JSON);

 //封装返回的响应实体

 return new ResponseEntity(result,headers, httpStatus);

 }

}
```



## 七、参数验证（使用JSR303）

### 1.添加依赖

```xml
<!-- 添加服务端验证的依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
</dependency>
```

### 2.常见的JSR303验证注解

`@NotEmpty`: 字符串, Collection, Map 和 Array 对象不能是 null 并且相关

对象的 size 大于 0。

`@NotBlank`: 字符串不是 null 且去除两端空白字符后的长度大于 0。

`@NotNull`: 字符串, Collection, Map 和 Array 对象不能是 null, 但可以是空

集（size = 0）。

`@Future` 验证是否在当前系统时间之后

`@Past` 验证是否在当前系统时间之前

`@Max` 验证值是否小于等于最大指定整数值

`@Min` 验证值是否大于等于最小指定整数值

`@Pattern` 验证字符串是否匹配指定的正则表达式

`@Email`被注释的元素必须是电子邮箱地址

`@Length` 被注释的字符串的大小必须在指定的范围内

`@Range` 被注释的元素必须在合适的范围内

### 3.使用实体类接收参数

创建UserInfoDTO类,添加验证规则

```
@Data
@NoArgsConstructor
@AllArgsConstructor
@ApiModel("用户信息")
public class UserDTO {

    @ApiModelProperty(name = "username", value = "用户名", required = true)
    @NotBlank(message = "用户名不能为空")
    private String username;

    @Email(message = "邮箱格式不正确")
    @ApiModelProperty(name = "email", value = "邮箱", required = true)
    private String email;

    @ApiModelProperty(name = "birthday", value = "生日", required = true)
    @NotNull(message = "生日不能为空")
    @Past(message = "生日不能为未来时间")
    private Date birthday;

}
```

### 4.接收参数的实体类前使用@Validated注解

```
@PostMapping("/hello")
 public Map<String,Object>  hello(@Validated @RequestBody UserInfoDTO
 userInfo){
   Map<String,Object> map=new HashMap<>();
   map.put("hello","hello");
   return map;
 }
```

控制器中传入的参数并不是对象  对参数做验证 需要在控制器上添 加@Validated注解

```
@RestController @Validated @Api(tags = "参数验证") public class HelloController {        @RequestMapping("/hello/{id}")        
public Map hello(@PathVariable @Max(value = 3,message = "id的值不能超过3")int id,@RequestParam @Email String email    ){
        Map map=new HashMap<>();                  
        map.put("hello","hello");                   
        return map;        
    } 
}
```

当参数验证失败时,系统会产生异常。我们需要将这些异常信息进行 统一的处理,返回给前端统一的Result对象信息。

```java
@ExceptionHandler(value = MethodArgumentNotValidException.class) public ResponseEntity handleMethodArgumentNotValidException(MethodArgumentNotValidException  exception) {    
    //获得验证错误的字段信息
    BindingResult bindingResult = exception.getBindingResult();    List fieldErrors = bindingResult.getFieldErrors();
    //将错误信息封装到map中
    Map map=new HashMap<>();    for (FieldError fieldError : fieldErrors) {
        //获得错误信息的字段名
        String field = fieldError.getField();        
        //获得字段对应的错误信息        
        String defaultMessage = fieldError.getDefaultMessage();        map.put(field,defaultMessage);    }    
    //将错误信息封装到 Result对象中    
    Result result = Result.error(exception,map);    return getResponseEntity(result); } @ExceptionHandler(value = ConstraintViolationException.class) public ResponseEntity handlerConstraintViolationException(ConstraintViolationException exception){    String tempMessage=  exception.getMessage();    String[] errorMessages = tempMessage.split(",");    String defaultError="";    
    //将错误信息封装到map中    
    Map map=new HashMap<>();    for (String errorMessage : errorMessages) {        String[] split = errorMessage.split(":");        map.put(split[0].split("\\.")[1],split[1].trim());    }    Result result = Result.error(exception,map);    return getResponseEntity(result); } 
```

二 分组验证 我们可以使用一个实体类分别来接收用户传入的新增的数据和更新 的数据。 这就会产生一个问题,新增数据和更新数据时,验证的条件是不同的,所 以我们需要区分出新增和更新的验证条件,此时使用分组验证即可。

```
 public interface AddGroup { } public interface UpdateGroup { } @Data @ApiModel public class PmsBrandDTO {    @ApiModelProperty(name = "id", value = "主键", dataType = "int")    @NotNull(message = "主键不能为空",groups = {UpdateGroup.class})    private Long id;     @ApiModelProperty( value = "品牌名", dataType = "java.lang.String")    @NotBlank(message = "品牌名不能为空",groups = {AddGroup.class, UpdateGroup.class})    @Length(max = 50,message = "品牌名字符长度不能超过50",groups = {AddGroup.class, UpdateGroup.class})    private String name;     @ApiModelProperty(value = "品牌logo地址", dataType = "java.lang.String")    private String logo;       @ApiModelProperty(name = "descript", value = "介绍", dataType = "java.lang.String")    private String descript;    //介绍    @ApiModelProperty( value = "检索首字母", dataType = "java.lang.String")    @Pattern(regexp = "[a-zA-Z]{1}",message = "检索首字母必须是字母,且长 度必须是1",groups = {AddGroup.class,UpdateGroup.class})    private String firstLetter;    //检索首字母    @ApiModelProperty(value = "排序", dataType = "java.lang.Integer")    @Min(value = 0,message = "排序最小值是0",groups = {AddGroup.class,UpdateGroup.class})    @Max(value = 10,message = "排序最大值是10", groups = {AddGroup.class,UpdateGroup.class})    private Integer sort;    //排序    private int version;   //版本 } @PostMapping("/update2") @ApiOperation(value = "修改信息") public boolean update2(@Validated(value = {UpdateGroup.class}) @RequestBody  PmsBrandDTO pmsBrandDTO) {    PmsBrand pmsBrand = this.pmsBrandService.getById(pmsBrandDTO.getId());    BeanUtils.copyProperties(pmsBrandDTO, pmsBrand);    return this.pmsBrandService.updateById(pmsBrand); }
```

