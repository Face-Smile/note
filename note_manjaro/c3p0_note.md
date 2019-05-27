# [c3p0数据库连接池的使用详解](https://www.cnblogs.com/fingerboy/p/5184398.html)



- 首先,什么是c3p0?下面是百度百科的解释:

> C3P0是一个开源的JDBC连接池，它实现了数据源和JNDI绑定，支持JDBC3规范和JDBC2的标准扩展。目前使用它的开源项目有*Hibernate，Spring*
>
>  

- 　　使用连接池和我们平时写的JDBC代码相比较有什么优点呢?

>  
>
> - - **资源重用：**
>
> ​     由于数据库连接得以重用，避免了频繁创建，释放连接引起的大量性能开销。在减少系统消耗的基础上，另一方面也增加了系统运行环境的平稳性。
>
> - - **更快的系统反应速度：**
>
> ​     数据库连接池在初始化过程中，往往已经创建了若干数据库连接置于连接池中备用。此时连接的初始化工作均已完成。对于业务请求处理而言，直接利用现有可用连接，避免了数据库连接初始化和释放过程的时间开销，从而减少了系统的响应时间。
>
> - - **新的资源分配手段：**
>
> ​     对于多应用共享同一数据库的系统而言，可在应用层通过数据库连接池的配置，实现某一应用最大可用数据库连接数的限制，避免某一应用独占所有的数据库资源。
>
> - - **统一的连接管理，避免数据库连接泄露：**
>
> ​     在较为完善的数据库连接池实现中，可根据预先的占用超时设定，强制回收被占用连接，从而避免了常规数据库连接操作中可能出现的资源泄露。
>
>  

如何在自己的项目中使用c3p0呢?

1. **导jar包:**

2. **两种方式可以建立c3p0连接,第一种方式是代码方式,Demo如下:**

   [![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

   ```java
   package com.wang.utils;
   
   import java.beans.PropertyVetoException;
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   import com.mchange.v2.c3p0.ComboPooledDataSource;
   
   public class JDBCutils {
   
       private static Connection conn;
       private static ComboPooledDataSource ds = new ComboPooledDataSource();
   
       public static Connection getConnection() {
           try {
               ds.setDriverClass("com.mysql.jdbc.Driver");
               ds.setJdbcUrl("jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF8&useServerPrepStmts=true&prepStmtCacheSqlLimit=256&cachePrepStmts=true&prepStmtCacheSize=256&rewriteBatchedStatements=true");
               ds.setUser("root");
               ds.setPassword("123");
               conn = ds.getConnection();
           } catch (SQLException e) {
               e.printStackTrace();
           } catch (PropertyVetoException e) {
               e.printStackTrace();
           }
           return conn;
       }
   ```



   第二种是使用读取配置文件的方式,要求是,**配置文件必须命名为c3p0-config.xml,并且放在src目录下**,配置文件如下:



   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <c3p0-config>
       <default-config> 
           <property name="jdbcUrl">
               <![CDATA[
                   jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF8&useServerPrepStmts=true&prepStmtCacheSqlLimit=256&cachePrepStmts=true&prepStmtCacheSize=256&rewriteBatchedStatements=true
               ]]>
           </property>
           <property name="driverClass">com.mysql.jdbc.Driver</property>
           <property name="user">root</property>
           <property name="password">123</property> 
   　　     <!--当连接池中的连接耗尽的时候c3p0一次同时获取的连接数。Default: 3 -->
           <property name="acquireIncrement">3</property>
   　　     <!-- 初始化数据库连接池时连接的数量 -->
           <property name="initialPoolSize">10</property>
           <!-- 数据库连接池中的最小的数据库连接数 -->
           <property name="minPoolSize">2</property>
           <!-- 数据库连接池中的最大的数据库连接数 -->
           <property name="maxPoolSize">10</property>
       </default-config>
   </c3p0-config>
   ```



   java代码部分:



   ```java
   package com.wang.utils;
   
   import java.beans.PropertyVetoException;
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   import com.mchange.v2.c3p0.ComboPooledDataSource;
   
   public class JDBCutils {
   
       private static Connection conn;
       private static ComboPooledDataSource ds = new ComboPooledDataSource();
   
       public static Connection getConnection() {
           try {
               conn = ds.getConnection();
           } catch (SQLException e) {
               e.printStackTrace();
           } catch (PropertyVetoException e) {
               e.printStackTrace();
           }
           return conn;
       }
   ```

    

   注意,配置文件里可以写多个数据库配置,上面的配置文件代码,我们是放在default-config标签下,可以再添加一个用<named-config name="mysqlConfig">标签修饰的配置,只需要在代码中,将name的值,放在new ComboPooledDataSource("mysqlConfig")中即可,

 