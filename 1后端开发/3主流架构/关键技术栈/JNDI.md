# JNDI技术

JNDI（Java Naming and Directory Interface）是 Java 平台提供的一套用于访问命名和目录服务的 API。它允许开发者通过统一的接口查找和访问各种资源（如数据库连接、EJB、配置文件等），而无需关心底层具体实现。

### **核心功能**

1. **命名服务**
   * 将名称与对象关联，类似 “电话簿” 功能（例如：通过`jdbc/datasource`名称查找数据库连接池）。
   * 支持分层命名空间（如 LDAP、DNS、RMI 等）。
2. **目录服务**
   * 存储和检索对象的属性信息（如用户信息、设备配置）。
   * 支持复杂查询（如按属性过滤）。

### **典型应用场景**

1. **Java EE 容器**
   * 查找 EJB、JMS 队列、数据源等资源。
   * 示例：在 Tomcat 中配置数据源后，通过 JNDI 获取`DataSource`对象。
2. **资源管理**
   * 集中管理配置（如数据库 URL、邮件服务器地址）。
   * 解耦应用与底层实现（更换数据库时只需修改 JNDI 配置）。
3. **分布式系统**
   * 注册和发现分布式服务（如 RMI 远程对象）。

### **基本使用步骤**

1. **初始化上下文**

   ```java
   import javax.naming.InitialContext;
   import javax.naming.Context;
   
   Context ctx = new InitialContext();
   ```
   
2. **查找资源**

   ```java
   DataSource dataSource = (DataSource) ctx.lookup("java:comp/env/jdbc/mydb");
   Connection conn = dataSource.getConnection();
   ```
   
3. **绑定资源（可选）**

   ```java
   ctx.bind("ejb/MyService", myEjbInstance);
   ```

### **常见实现与配置**

* **应用服务器**：Tomcat、JBoss、WebSphere 等均内置 JNDI 服务。

* **独立服务**：可通过 LDAP（如 Apache Directory Server）或 RMI 实现。

* 配置示例

  （Tomcat 的`context.xml`）：
  ```xml
  <Resource name="jdbc/mydb" 
            auth="Container" 
            type="javax.sql.DataSource" 
            driverClassName="com.mysql.cj.jdbc.Driver"
            url="jdbc:mysql://localhost:3306/mydb"
            username="root" 
            password="password"/>
  ```

### **注意事项**

1. **异常处理**：JNDI 操作可能抛出`NamingException`，需捕获并处理。
2. **安全性**：避免 JNDI 注入攻击（如限制 JNDI 上下文权限、使用 JDK 6u132 + 的安全修复）。
3. **性能**：频繁查找可能影响性能，建议缓存结果或使用连接池。

### **总结**

JNDI 是 Java 生态中资源管理的核心工具，通过统一接口简化了复杂环境下的资源访问，尤其适用于企业级应用和分布式系统。