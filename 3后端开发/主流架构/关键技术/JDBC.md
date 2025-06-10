# JDBC 技术

1. ## 概念

JDBC（Java Database Connectivity）是Java语言访问关系型数据库的标准API。

它允许Java程序员使用标准的SQL语句来访问和操作关系型数据库。JDBC提供了一种标准的方式来连接到不同数据库的驱动程序，并且是Java EE平台上进行数据访问的基础。它提供了许多接口和类，使Java应用程序可以通过它们来访问和管理关系型数据库。

2. ## 原理

加载数据库驱动：在Java应用程序中，首先需要加载适当的数据库驱动程序。

连接数据库：使用Java程序中的getConnection（）方法与数据库建立连接。

创建操作对象：使用Java程序中的Statement对象或者PreparedStatement对象来执行SQL语句。

执行SQL语句：使用Statement对象或者PreparedStatement对象来执行SQL语句，在执行SQL语句之前，需要对SQL语句进行预编译。

处理查询结果：使用ResultSet对象来处理从数据库返回的查询结果。

释放资源：使用Java程序中的close（）方法释放资源（ResultSet对象、Statement对象、Connection对象）。

```java
String url = "jdbc:mysql://localhost:3306/1127douyinDB";
String user = "root";
String password = "root";

try {
	// 加载MySQL驱动程序
	Class.forName("com.mysql.cj.jdbc.Driver");

            // 建立MySQL数据库连接
            Connection connection = DriverManager.getConnection(url, user, password);

            // 创建Statement对象
            Statement statement = connection.createStatement();

            // 执行SQL查询语句
            ResultSet resultSet = statement.executeQuery("SELECT * FROM douyin");

            // 处理查询结果
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String liveStreaming = resultSet.getString("liveStreaming");
                int looknumber = resultSet.getInt("looknumber");
                System.out.println("id: " + id +"\tName: " + liveStreaming + "\tlooknumber: " + looknumber);
            }

            // 关闭ResultSet、Statement和Connection对象
            resultSet.close();
	statement.close();
	connection.close();
} catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
}

```



> TIP：JDBC的执行过程是通过Java语言的标准接口实现的，具有跨平台的优点。同时，JDBC也支持连接池和事务管理等功能。