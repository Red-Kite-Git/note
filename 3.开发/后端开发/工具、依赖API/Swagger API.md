# Swagger	(io.swagger.annotations)

Swagger 是一个用于设计、构建、文档化和使用 RESTful Web 服务的工具集，在 API 开发领域应用广泛，以下是对它的详细介绍：

## 一、功能特性

* **API 文档生成**：Swagger 可以根据代码中的注释或特定的配置，自动生成详细的 API 文档。这些文档不仅包含了 API 的端点、请求方法、参数等基本信息，还能提供示例请求和响应，方便开发人员和其他相关人员快速了解 API 的使用方法。
* **API 设计与验证**：它提供了一种基于 YAML 或 JSON 格式的 API 描述语言（OpenAPI 规范），允许开发人员在编写实际代码之前，先定义好 API 的结构和规范。这样可以在开发早期发现设计上的问题，避免后期因为 API 设计的变更而导致大量的代码修改。同时，Swagger 还能对 API 的实现进行验证，确保实际的 API 符合预先定义的规范。
* **交互性测试**：Swagger UI 是 Swagger 的一个重要组件，它提供了一个可视化的界面，让用户可以直接在浏览器中查看和测试 API。用户可以在界面上输入请求参数，发送请求，并查看响应结果，无需使用额外的工具如 Postman 等，方便快捷地进行 API 的调试和测试。
* **代码生成**：Swagger 能够根据 API 定义生成多种编程语言的客户端代码和服务器端代码框架。这大大减少了开发人员编写样板代码的工作量，提高了开发效率，使得开发人员可以将更多的精力放在业务逻辑的实现上。

## 二、工作原理

* Swagger 使用 OpenAPI 规范来描述 API。OpenAPI 规范是一种基于 JSON 或 YAML 格式的文件，它定义了 API 的各个方面，包括路径、操作、参数、请求和响应体等。开发人员通过在代码中添加 Swagger 注释或直接编写 OpenAPI 文件来描述 API。Swagger 工具集根据这些描述信息生成文档、UI 界面和代码等。

## 三、应用场景

* **团队协作**：在一个大型的软件开发项目中，不同的团队可能负责不同的模块，而 API 是各个模块之间进行交互的重要方式。Swagger 可以让各个团队更好地理解彼此提供的 API，减少沟通成本，提高协作效率。例如，前端团队可以通过 Swagger 文档快速了解后端提供的 API 接口，从而更高效地进行前端页面的开发。
* **API 发布与共享**：当公司对外发布 API 供其他开发者使用时，Swagger 可以提供一个清晰、直观的文档和测试界面，方便外部开发者快速上手使用 API。这有助于提高公司 API 的易用性和吸引力，促进更多的开发者基于公司的 API 进行创新和开发。
* **API 版本管理**：随着业务的发展，API 可能会不断更新和迭代。Swagger 可以很好地支持 API 的版本管理，开发人员可以为不同版本的 API 分别定义规范，并通过 Swagger 进行文档化和测试，确保各个版本的 API 都能得到有效的管理和维护。

## 四、在SpringBoot中使用

在 Spring Boot 项目中使用 Swagger 可以方便地生成和展示 API 文档，下面将详细介绍具体的使用步骤。

### 步骤 1：添加依赖

在`pom.xml`文件中添加 Swagger 相关依赖

```xml
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
```

### 步骤 2：配置 Swagger

创建一个SwaggerConfig.java配置类来配置Swagger

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                // 是否开启
                .enable(true).select()
                // 如果类上使用@Api注解  扫描这个类上的Swagger注解信息
                .apis(RequestHandlerSelectors.withClassAnnotation(Api.class))
                // 指定路径处理PathSelectors.any()代表所有的路径
                .paths(PathSelectors.any()).build().pathMapping("/")
                .securitySchemes(security());
    }

    private List<ApiKey> security() {
        List<ApiKey> list = new ArrayList<>();
        ApiKey apiKey = new ApiKey("token", "token", "header");
        list.add(apiKey);
        return list;
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder().title("springBoot03项目的接口文档")
                .contact(new Contact("kgc", "www.kgc.cn", "kgc@qq.com"))
                .description("接口文档")
                .version("v1.0")
                .build();
    }
    
}
```

### 步骤 3：在 Controller 中添加注解

在控制器类和方法上添加 `@Swagger` 注解，以便生成详细的 API 文档

```java
/**
 * @author YC
 * RequestMapping("/user")在类上注解时，进入该控制器的请求全都需要先 /user 再进入对应方法的路径
 * RestController注解 表示该类中的方法返回值会自动转换为JSON格式数据
 * Api注解 表示该类是一个接口（tags表示该接口的描述）
 */
@RestController
@RequestMapping("/user")
@Api(tags = "用户接口")
public class SmbmsUserController {

    @Resource
    private SmbmsUserService smbmsUserService;

    /**
     * PostMapping("/insert")注解 表示进入该方法的请求是POST请求 路径为/insert
     * RequestBody注解 表示传入的对象会变为JSON格式数据
     */
    @PostMapping("/insert")
    @ApiOperation(value = "添加用户")
    public boolean insertOne(@RequestBody InsertUserForm form) {
        log.debug("添加用户：{}", form);
        SmbmsUser smbmsUser = new SmbmsUser();
        BeanUtil.copyProperties(form, smbmsUser);
        return smbmsUserService.save(smbmsUser);
    }

    /**
     * GetMapping("/list")注解 表示进入该方法的请求是GET请求 路径为/list
     * ApiOperation注解 表示该方法是接口中的方法(value表示该方法的描述)
     */
    @GetMapping("/list")
    @ApiOperation(value = "查询所有用户")
    public List<SmbmsUser> getList() {
        return smbmsUserService.list();
    }

    /**
     * PostMapping("/login")注解 表示进入该方法的请求是POST请求 路径为/login
     * ApiImplicitParams注解 表示该方法的多个参数描述（数组）
     * ApiImplicitParam注解 表示该方法的单个参数描述（name参数名 value描述信息 required是否必填 dataType数据类型 paramType参数类型）
     */
    @PostMapping("/getOne")
    @ApiOperation(value = "查询一个用户")
    @ApiImplicitParam(name = "id", value = "用户ID", required = true,
            dataType = "long", paramType = "query")
    public SmbmsUser getOne(@RequestParam("id") Long id) {
        return smbmsUserService.getById(id);
    }

    @DeleteMapping("/delete/{id}")
    @ApiOperation(value = "删除用户")
    @ApiImplicitParam(name = "id", value = "用户id", required = true,
            dataType = "long", paramType = "path")
    public boolean deleteOne(@PathVariable("id") Long id) {
        return smbmsUserService.removeById(id);
    }

    /**
     * PutMapping("/update")注解 表示进入该方法的请求是PUT请求 路径为/update
     */
    @PutMapping("/update")
    @ApiOperation(value = "更新用户")
    public boolean update(@RequestBody UpdateUserForm form) {
        SmbmsUser smbmsUser = new SmbmsUser();
        BeanUtil.copyProperties(form, smbmsUser);
        smbmsUser.setModifyDate(new Date());
        return smbmsUserService.updateById(smbmsUser);
    }

    /**
     * MyBatisPlus查询
     * PostMapping("/login")注解 表示进入该方法的请求是POST请求 路径为/login
     * ApiImplicitParams注解 表示该方法的多个参数描述（数组）
     * ApiImplicitParam注解 表示该方法的单个参数描述（name参数名 value描述信息 required是否必填 dataType数据类型 paramType参数类型）
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

### 步骤 4：访问 Swagger UI

启动 Spring Boot 应用程序，在浏览器中访问以下 URL
http://localhost/doc.html
http://localhost/swagger-ui.html

