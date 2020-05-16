### Tomcat

#### Tomcat 在没有图形化界面的系统的启动方式

方式一：

在catalina.sh中添加参数

 ```shell
JAVA_OPTS="${JAVA_OPTS} -Djava.awt.headless=true
 ```





方式二：

在idea中配置

编辑Tomcat配置环境，选择Server，在VM OPtions中添加`-Djava.awt.headless=true`


#### 解决tomcat访问html文件乱码问题
在catalina.sh JAVA_OPTS或者idea中VM options中添加参数
`-DFile.encoding=UTF-8`



#### 解决mac idea启动弹出boostrap的问题
在VM Options中添加参数
`-Dapple.awt.UIElement=true`



### Spring

#### IOC （Inverse Of Control:控制反转）容器

控制反转：把创建对象的权利交给框架，是框架的重要特征，并非面向对象编程的专用术语，它包括依赖注入和依赖查找。

使用Spring的IOC解决程序的耦合

#### AOP（面向切面编程）



bean对象：在计算机语言中有可重用组件的含义。

javabean：用java语言编写的可重用组件。

​				javabean 并不等于 `实体类`，而是大于`实体类`

一个创建bean对象的工厂

​			它就是创建我们的service和dao对象

如何创建：

​		第一个：需要一个配置文件来配置我们的service和dao

​					配置的内容：唯一标识=全限定类名（key=value）

​		第二个：通过读取配置文件中配置的内容，通过反射创建对象

​		配置文件可以是xml，也可以是properties。

连接点：所有的方法都可以是连接点
切入点：被增强的方法时切入点


### spring framework组成

#### org.springframework

##### spring-context

###### 1. spring-aop

###### 2. spring-beans

###### 3. spring-core

###### 4. spring-expression



spring bean对象管理

在resources文件下创建一个xml文件来使用bean标签来有spring管理对象的创建（降低耦合）

标准配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">   
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```





### 问题总结

#### 1. org.springframework.web.bind.annotation.RequestMapping无法导入

查看是否缺少依赖，如果是maven 可以使用以下代码至pom.xml

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.1.5.RELEASE</version>
</dependency>
```

#### 2. Mac中使用FileSystemXmlApplicationContext加载绝对路径，出现找不到文件，但路径是对的

路径应该使用`//`开头，而不是`/`,使用`/`是相对于项目根目录，都不用是相对于当前文件的路径





### SpringMVC

 SpringMVC是Spring框架的一个模块。

SpringMVC是一个基于MVC的web框架。

MVC是一个设计模式（模型、视图、控制器）

1. 前端控制器（DiapatcherServlet）

   作用：接收请求，响应结果，相当于转发器

2. 处理映射器（HandlerMapping）

   作用：根据请求的url查找Handler

3. 处理适配器（去执行HandlerAdapter）

   作用：按照特定的规则去执行Handler

4. 处理器（Handler）

   注意：编写Handler要按照HandlerAdapter的要求去做，这样适配器才能正确执行Handler。

5. 视图解析器（View resolver）

   作用：进行视图解析，根据逻辑视图名解析成真正的视图（view）。

6. 视图View

   View是一个接口，实现支持不同的View类型（jsp, freemaker, pdf….）。

   

SpringMVC流程

1. 用户发送请求至前端控制器DispatcherServlet；
2. DispatcherServlet收到请求后，调用HandlerMapping处理器映射器，请求获取Handler；
3. 处理器映射器根据url找到具体的处理器，生成处理器对象以及处理器拦截器（如果有则生成）一并返回给DispatcherServlet；
4. DispatcherServlet调用HandlerAdapter处理器适配器；
5. HandlerAdapter经过适配调用具体处理器（Handler, 也叫后端处理器）
6. Handler执行完成并返回ModelAndView给HandlerAdapter；
7. HandlerAdapter将Handler执行的结果ModelAndView返回给DispatcherServlet；
8. DispatcherServlet将ModelAndView传给ViewResolver视图解析器进行解析
9. ViewResolver解析后返回具体的View；
10. DispatcherServlet对view进行渲染视图（即将模型数据填充至视图中）；
11. DispatcherServlet响应用户请求。











用IDEA编辑器创建Maven+SpringMVC项目：

1. idea创建新项目->选择Maven->选择org.apache.maven.archetypes:maven-archetype-webapp(需要将`create from archetype`打勾才能选择)

2. 在main下新建文件夹`java.<groupid>`

3. 添加SpringMVC依赖

   ```shell
   <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-webmvc</artifactId>
     <version>4.3.3.RELEASE</version>
   </dependency>
   ```

   版本可以自由选择，查询网站：https://mvnrepository.com/



SpringMVC 配置文件：位置同`web.xml`,名称为：`mvc-dispatcher-servlet.xml`



1. 启动服务器，加载一些配置文件
   - DispatcherServlet
   - springMVC.xml 被加载了
   - HelloController创建成对象

2. 发送请求，后台处理请求



#### RequestMapping

参数：

- `path`/`vaue`：表示请求的路径，值为字符串
- `method`：表示请求的方法，参数值为RequestMethod枚举类型；默认为空
- `params`：表示请求url中必须包含的参数,参数值为字符串类型，如果是key=value 类型的字符串表示必须匹配key=value的参数才能访问，如果是`key!value`类型的字符串表示

SpringMVC 可以对请求参数进行封装，只需在请求的方法中增加即可（注意：URL请求的参数名必须和方法中的变量名一致）

SpringMVC 还可以对请求参数进行封装，封装到一个JavaBean对象。可以在方法中传入bean对象（同样url参数名必须和bean对象中的变量名一致）

如果bean对象中还有引用对象，则url参数名称可以使用多级引用的方法。

例：

Account.java

```java
package com.smilejack.domain;

import java.io.Serializable;

public class Account implements Serializable {
    private String username;
    private String password;
    private Float money;
    private User user;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Float getMoney() {
        return money;
    }

    public void setMoney(Float money) {
        this.money = money;
    }

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    @Override
    public String toString() {
        return "Account{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", money=" + money +
                ", user=" + user +
                '}';
    }
}
```

User.java

```java
package com.smilejack.domain;

import java.io.Serializable;

public class User implements Serializable {
    private String accountName;
    private Integer age;

    public String getAccountName() {
        return accountName;
    }

    public void setAccountName(String accountName) {
        this.accountName = accountName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "accountName='" + accountName + '\'' +
                ", age=" + age +
                '}';
    }
}

```

ParamController.java

```java
package com.smilejack.controller;


import com.smilejack.domain.Account;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * 请求参数绑定
 */
@Controller
@RequestMapping("/param")
public class ParamController {
    /**
     * 将请求参数封装到JavaBean的类中
     * @param account
     * @return
     */
    @RequestMapping(path = "/saveAccount", method = {RequestMethod.POST})
    public String saveAccount(Account account){

        System.out.println("执行了请求参数绑定。。。。");
        System.out.println(account);
        return "success";
    }
}

```

jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: smilejack
  Date: 7/26/19
  Time: 17:43
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>请求参数绑定</title>
</head>
<body>
<form action="/param/saveAccount" method="post">
    姓名：<input type="text" name="username" /><br/>
    密码：<input type="text" name="password" /><br/>
    金额：<input type="text" name="money" /><br/>
    姓名：<input type="text" name="user.accountName"><br/>
    年龄：<input type="text" name="user.age"><br/>
    <input type="submit" value="提交" /><br/>
</form>
</body>
</html>

```

List集合(其中list对应List集合的参数名)

```html
账户姓名：<input type="text" name="list[0].accountName"><br/>
账户年龄：<input type="text" name="list[0].age"><br/>
```

Map集合(其中map对应Map集合的参数名)

```html
账户姓名：<input type="text" name="map['one'].accountName"><br/>
账户年龄：<input type="text" name="map['one'].age"><br/>
```

自定义类型转换

编写类继承org.springframework.core.convert.converter.Converter方法

并在xml了中配置自定义类型转换

```XML
<beans>
		<!--配置自定义类型转换器-->
    <!--配置完后还必须在配置值SpringMVC注解支持中添加conversion-service属性-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.smilejack.utils.StringToDateConverter"></bean>
            </set>
        </property>
    </bean>
    <!--开启SpringMVC框架注解支持-->
    <mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
</beans>
```

#### RequestParam
> **把请求中指定名称的参数给控制器中的形参赋值 **

** 属性： **
- `vlaue`:请求参数中的名称
- `required`:请求参数中是否必须提供此参数，默认值为true,表示必须提供

#### RequestBody
> **用于获取请求体内容，直接使用得到的是key=value@key=value....的形式**
> **get请求方式不适用，get没有请求体**

**属性：**
- required：是否必须有请求体，默认值为true，取true，get方法会报错，取false，get方法的到是null

#### PathVariable
> **用于绑定URL的占位符**



## Maven

### 配置

#### 配置自定义Maven 仓库下载位置

1. 创建一个目录用来保存Maven仓库
2. 修改maven 目录下conf/settings.xml文件添加或者修改以下代码

```xml
<localRepository>你想要保存的Maven的仓库的位置</localRepository>
```



3. 将上面的配置文件seetings.xml 复制到你的仓库根目录下

以下为idea可选的idea配置使用本地Maven：

1. 修改Maven_home directory 为 本地Maven家目录
2. 设置user settings file 为自定义仓库中的settings.xml 并选中Override。
3. 点击应用即可。

如果想让idea自动下载pom.xml 中的依赖,可以在提示Maven projects needed to be imported 中选择enable auto-import

`import changes`：表示导入包，但仅这一次

`enable auto-import`:表示自动导入包，以后更改pom.xml就会自动下载依赖包。

#### 配置国内镜像源（阿里云）

修改Maven仓库目录下conf/settings.xml，添加以下代码

```xml
<mirrors>
	<id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
  <mirrorOf>central</mirrorOf>
</mirrors>
```

第二步：pom.xml 文件添加

```xml
<repositories>  
        <repository>  
            <id>alimaven</id>  
            <name>aliyun maven</name>  
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
            <releases>  
                <enabled>true</enabled>  
            </releases>  
            <snapshots>  
                <enabled>false</enabled>  
            </snapshots>  
        </repository>  
</repositories>  
```









Maven的一个哲学是惯例优于配置(Convention Over Configuration), Maven默认的依赖配置项中，scope的默认值是compile，项目中经常傻傻的分不清，直接默认了。今天梳理一下maven的scope。

#### scope的分类

##### compile

**默认就是compile**，什么都不配置也就是意味着compile。compile表示被依赖项目需要参与当前项目的编译，当然后续的测试，运行周期也参与其中，是一个比较强的依赖。打包的时候通常需要包含进去。

##### test

scope为test表示依赖项目仅仅参与测试相关的工作，包括测试代码的编译，执行。比较典型的如junit。

##### runntime

runntime表示被依赖项目无需参与项目的编译，不过后期的测试和运行周期需要其参与。与compile相比，**跳过编译**而已，说实话在终端的项目（非开源，企业内部系统）中，和compile区别不是很大。比较常见的如JSR×××的实现，对应的API jar是compile的，具体实现是runtime的，compile只需要知道接口就足够了。oracle jdbc驱动架包就是一个很好的例子，一般scope为runntime。另外runntime的依赖通常和optional搭配使用，optional为true。我可以用A实现，也可以用B实现。

##### provided

provided意味着打包的时候可以不用包进去，别的设施(Web Container)会提供。事实上该依赖理论上可以参与编译，测试，运行等周期。相当于compile，但是在打包阶段做了exclude的动作。

##### system

从参与度来说，也provided相同，不过被依赖项不会从maven仓库抓，而是从本地文件系统拿，一定需**要配合systemPath属性使用**。

#### scope的依赖传递

A–>B–>C。当前项目为A，A依赖于B，B依赖于C。知道B在A项目中的scope，那么怎么知道C在A中的scope呢？答案是： 
当C是test或者provided时，C直接被丢弃，A不依赖C； 
否则A依赖C，C的scope继承于B的scope。



### packaging参数

程序的打包方式

jar：默认的打包方式,内部调用或者是作服务使用

pom: 父类型都为pom类型

war: 需要部署的项目





## Junit 单元测试

1. 应用程序的入口

   ​		main方法

2. Junit单元测试中，没有main方法也能执行

   ​		Junit集成了一个main方法

   ​		该方法就会判断当前测试类中哪些方法有@Test注解

   ​		Junit就会让有Test注解的方法执行

3. Junit不会管我们是否采用spring框架

   ​		在执行测试方法时，Junit根本不知道我们是不是使用了spring框架

   ​		所以就不会为我们读取配置文件/配置类创建spring核心容器

4. 由以上三点可知

   ​		当测试方法执行时，没有IOC容器，就算写了Autowire的的注解，也无法实现注入。

Spring 框架单元测试：

- 使用spring自动注入的时候必须在测试类添加注解
	- `@Runwith(SpringJUnit4ClassRunner.class)`
	- `@Configuration(value = {})`, `value`中的值spring配置文件的位置