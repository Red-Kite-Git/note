# MybatisPlus

## 一、概念

MyBatisPlus 是一个基于 [MyBatis](..\Mybatis.md) 的增强工具，它在 MyBatis 的基础上进行了扩展和优化，提供了更强大、更便捷的功能

## 二、创建项目（Maven项目）

### 1、引入依赖（pom.xml）

```xml
<!-- 添加mybatisPlus依赖 -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.2</version>
</dependency>
```

### 2、添加配置信息（application.yaml）

```yaml
# MyBatis-Plus配置
mybatis-plus:
  # Mapper文件位置
  mapper-locations: classpath:mapper/*.xml
  # 实体包类的别名
  type-aliases-package: cn.kgc.entity
  # MyBatis配置
  configuration:
    # 是否开启下划线转驼峰命名
    map-underscore-to-camel-case: true
    # 自动映射行为
    auto-mapping-behavior: full
    # 日志实现类
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  # 全局配置
  global-config:
    # 数据库配置
    db-config:
      # 逻辑删除字段名
      logic-delete-field: deleted
      # 逻辑删除值
      logic-delete-value: 1
      # 逻辑非删除值
      logic-not-delete-value: 0
```

### 3、添加注解(实体类)

>`@TableName(value = "表名")`：实体类与数据库中的哪个表关联
>
>`@TableId(value = "主键字段名",type = "IdType.ENUM")`： 实体类中的属性与表中主键映射
>		enum IdType：NONE[default无]	AUTO [自增]
>
>`@TableField(value = "字段名",exists = boleen,fill = "FieldFill.ENUM")`： 实体类中的属性与表中的非主键字段映射 
>		exists：true[default有]	false[无]
>		fill：指定什么时候让MyBatisPlus给字段赋值（在implements MetaObjectHandler的类中配置）
>		enum FieldFill：DEFAULT[默认]	INSERT[新增]	UPDATE[更新]	INSERT_UPDATE[新增与更新]
>
>`@TableLogic(value = "0", delval = "1")`：逻辑删除，在全局配置中配置后可以不使用此注解
>		value：逻辑未删除值（一般为0）
>		delval：逻辑已删除值（一般为1）
>
>`@Version`：表示该属性是数据库表的乐观锁字段
>
>`@Data`：loombook定义的注解，用于生成Getter、Setter方法

```java
@TableName("table")
@Data
public class Entity {
    
    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
    
    ......

    @TableField("createdBy")
    private Long createdBy;

    @TableField(value = "creationDate", fill = FieldFill.INSERT)
    @JsonFormat(pattern = "yyyy-MM-dd hh:mm:ss")
    private Date creationDate;

    @TableField("modifyBy")
    private Long modifyBy;

    @TableField(value = "modifyDate", fill = FieldFill.INSERT_UPDATE)
    @JsonFormat(pattern = "yyyy-MM-dd hh:mm:ss")
    private Date modifyDate;

    @TableField(exist = false)
    @Setter(AccessLevel.NONE)
    private Integer age;
 	public int getAge() {
        if (this.birthday != null) {
            return DateUtil.ageOfNow(this.birthday);
        }
        return 0;
    }

    @TableLogic(value = "0", delval = "1")
    @TableField(value = "deleted", fill = FieldFill.INSERT)
    private Integer deleted;
    
    @Version
    @TableField(value = "version",fill = FieldFill.INSERT)
    private Integer version;

}
```

### 4.创建MyBatisPlus的配置类

实现MetaObjectHander接口

此配置类非必要，需要在新增、更新数据时自动填充时创建（也可以让数据库进行默认值管理）

配合实体类的`@TableField(fill = "FieldFill.ENUM")`注解使用

>`@Configuration`：Spring定义的注解，表示这是一个配置类，Spring会扫描此类中的@Bean注解
>
>`@Bean`：Spring定义的注解，表示将该对象交给Spring管理，在Spring中对象名就是方法名

```java
@Configuration
public class MyBatisPlusConfig implements MetaObjectHandler {

    /**
     * 使用MyBatisPlus新增数据自动填充功能
     */
    @Override
    public void insertFill(MetaObject metaObject) {
        setFieldValByName("version", 1, metaObject);
        setFieldValByName("deleted", 0, metaObject);
        setFieldValByName("新增时需要自动填充的字段名", 填充内容, metaObject);
        ...
    }

    /**
     * 使用MyBatisPlus更新数据自动填充功能
     */
    @Override
    public void updateFill(MetaObject metaObject) {
        setFieldValByName("更新时需要自动填充的字段名", 填充内容, metaObject);
    }
    
    /**
     * 乐观锁拦截器
     * 更新数据时 数据的乐观锁字段自动+1
     */
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor(){
        return new OptimisticLockerInterceptor();
    }
    
    /**
     * 分页拦截器
     * 在进行分页查询时，自动处理分页逻辑，实现数据的分页展示
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
    
}
```

### 5.创建Mapper接口与映射Mapper.xml文件

> `@Mapper`：Spring定义的注解，表示这是一个Mapper接口

```java
@Mapper
public interface Mapper extends BaseMapper<SmbmsUser> {
	//单表增删改查语句不需要
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="cn.kgc.mapper.SmbmsUserMapper">
	<!-- 单表增删改查语句不需要 -->
</mapper>
```

### 6.创建Service接口与ServiceImpl实现类

Service接口 继承 `IService<T>`类
```java
public interface Service extends IService<T> {
	//单表增删改查语句不需要
}
```

ServiceImpl实现类 继承 `ServiceImpl<M extends BaseMapper<T>, T>`类

```java
@Service("service")
public class ServiceImpl extends ServiceImpl<M extends BaseMapper<T>, T> implements SmbmsUserService {
	//单表增删改查语句不需要
}
```

### 7.创建Controller类

调用业务层方法实现新增操作

```java
@RestController
@RequestMapping("/user")
public class Controller {

    @Resource
    private SmbmsUserService smbmsUserService;

    /**
     * PostMapping("/insert")注解 表示进入该方法的请求是POST请求 路径为/insert
     * RequestBody注解 表示传入的对象会变为JSON格式数据
     */
    @PostMapping("/insert")
    public boolean insertOne(@RequestBody InsertUserForm form) {
        SmbmsUser smbmsUser = new SmbmsUser();
        BeanUtil.copyProperties(form, smbmsUser);
        return smbmsUserService.save(smbmsUser);
    }

    /**
     * GetMapping("/list")注解 表示进入该方法的请求是GET请求 路径为/list
     */
    @GetMapping("/list")
    public List<SmbmsUser> getList() {
        return smbmsUserService.list();
    }

    /**
     * PostMapping("/login")注解 表示进入该方法的请求是POST请求 路径为/login
     */
    @PostMapping("/getOne")
    public SmbmsUser getOne(@RequestParam("id") Long id) {
        return smbmsUserService.getById(id);
    }

    @DeleteMapping("/delete/{id}")
    public boolean deleteOne(@PathVariable("id") Long id) {
        return smbmsUserService.removeById(id);
    }

    /**
     * PutMapping("/update")注解 表示进入该方法的请求是PUT请求 路径为/update
     */
    @PutMapping("/update")
    public boolean update(@RequestBody UpdateUserForm form) {
        SmbmsUser smbmsUser = new SmbmsUser();
        BeanUtil.copyProperties(form, smbmsUser);
        smbmsUser.setModifyDate(new Date());
        return smbmsUserService.updateById(smbmsUser);
    }

    /**
     * PostMapping("/login")注解 表示进入该方法的请求是POST请求 路径为/login
     */
    @PostMapping("/login")
    @ApiOperation(value = "登录（根据编号与密码查询）")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "userCode", value = "用户编号", required = true,
                    dataType = "string", paramType = "query"),
            @ApiImplicitParam(name = "userPassword", value = "用户密码", required = true,
                    dataType = "string", paramType = "query")
    })
    public SmbmsUser login(
            @RequestParam(value = "userCode", defaultValue = "") String userCode,
            @RequestParam(value = "userPassword", defaultValue = "") String userPassword
    ) {
        //封装查询条件
        QueryWrapper<SmbmsUser> queryWrapper = new QueryWrapper<>();
        //eq方法 等值判断
        queryWrapper.eq(!userCode.isBlank(), "userCode", userCode)
                .eq(!userPassword.isBlank(), "userPassword", userPassword);
        return smbmsUserService.getOne(queryWrapper);
    }

    @PostMapping("/selectMore")
    @ApiOperation(value = "根据编号与角色查询")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "userCode", value = "用户编号",
                    dataType = "string", paramType = "query"),
            @ApiImplicitParam(name = "userRole", value = "用户角色",
                    dataType = "long", paramType = "query")
    })
    public List<SmbmsUser> getMore(
            @RequestParam(value = "userCode", defaultValue = "") String userCode,
            @RequestParam(value = "userRole", defaultValue = "0") Long userRole
    ) {
        //封装查询条件
        QueryWrapper<SmbmsUser> queryWrapper = new QueryWrapper<>();
        //eq方法 等值判断
        queryWrapper.like(!userCode.isBlank(), "userCode", "%" + userCode + "%")
                .eq(userRole != 0, "userRole", userRole);
        return smbmsUserService.list(queryWrapper);
    }
}
```

## 三、MyBatisPlus管理乐观锁、逻辑删除

