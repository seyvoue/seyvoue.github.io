---
title: '@JavaWeb: 从零开始写 Java Web 框架'
comments: true
categories: manual
tags: java
abbrlink: fa02af02
date: 2018-01-19 18:33:10
updated:
keywords:
description:
---

目标： 搭建一个轻量级的 MVC 框架。

1. 如何快速搭建开发框架
2. 如何加载并读取配置文件
3. 如何实现一个简单的 IOC 容器
4. 如何加载指定的类
5. 如何初始化框架

Controller 是 MVC 的核心，一个Controller类包含了多个Action 方法，可返回 View 对象或 Data 对象，分别对应 JSP 页面和 JSON 数据。在普通请求的情况下，可返回 JSP 页面，在 Ajax 请求的情况下，可返回 JSON 数据。

## 搭建开发环境

- 既然是一款 JavaWeb 框架，需要添加依赖 Servlet、JSP、JSTL
- 在框架中会大量使用日志输出，需要添加`SLF4J`的日志框架
- 由于涉及到mysql数据库，需要添加一个 mysql 的 Java驱动程序所对应的依赖
- 由于在 Controller 的 Action方法返回值中是可以返回 JSON 数据的，需要选择一款JSON 序列化工具
- 还需要添加一些常用的工具类，诸如数据类型转换、集合类判空等
- 每次对数据库的操作，都需要面对PreparedStatement 和ResultSet，为了减少这部分代码的编写，需要添加一个 DBUTILS
- 每次对数据库的操作，都需要创建一个connection，并在操作完成后，关闭数据库连接，频繁的这种操作，必定会造成大量的系统开销，而且数据库的连接数是有限的，需要添加数据库连接池框架 DBCP
- Service 编写好后，要进行单元测试，需要添加 JUNIT 依赖

上面提到的所有依赖，见下：

```xml
<dependencies>
        <!-- Servlet -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>${servlet.version}</version>
            <scope>provided</scope>
        </dependency>

        <!-- JSP -->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>

        <!-- JSTL -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
            <scope>runtime</scope>
        </dependency>

        <!--Junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

        <!--SLF4J-->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.7</version>
        </dependency>

        <!--MySQL-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.33</version>
            <scope>runtime</scope>
        </dependency>

        <!--Apache Commons Lang-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.3.2</version>
        </dependency>
        <!--Apache Commons Collections-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-collections4</artifactId>
            <version>4.0</version>
        </dependency>

        <!--Apache Commons DbUtils-->
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.6</version>
        </dependency>

        <!--Apache Commons Dbcp2-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-dbcp2</artifactId>
            <version>2.0.1</version>
        </dependency>

    </dependencies>
```

## 如何加载并读取配置文件？

如何加载并读取在`/src/main/resources` 目录下的诸如`*.properties`的配置文件，配置文件中的内容一般如下：

```
rt.framework.jdbc.driver=com.mysql.jdbc.Driver
smart.framework.jdbc.url=jdbc:mysql://localhost:3306/demo
smart.framework.jdbc.username=root
smart.framework.jdbc.password=30131234

smart.framework.app.base_package=com.seyvoue.demo.chapter3
smart.framework.app.jsp_path=/WEB-INF/view/
smart.framework.app.asset_path=/asset/
```

*注：`*.properties` 文件中的内容是 key=value 的形式*

step1：首先创建一个 [ConfigConstant](https://github.com/seyvoue/smart-web/blob/master/smart-framework/src/main/java/com/seyvoue/demo/framework/ConfigConstant.java) 的常量类，用来维护`*.properties`文件中相关配置项的名称（即 `key`）

step2：然后创建一个 [PropsUtil](https://github.com/seyvoue/smart-web/blob/master/smart-framework/src/main/java/com/seyvoue/demo/framework/util/PropsUtil.java) 工具类用来读取该配置文件（这个工具类依赖 `java.util.Properties`）

step3：创建 [ConfigHelper](https://github.com/seyvoue/smart-web/blob/master/smart-framework/src/main/java/com/seyvoue/demo/framework/util/PropsUtil.java) 通过 PropsUtil 工具类加载配置文件

## 如何开发一个类加载器

如何开发一个“类加载器”加载某基础包下的所有类，比如使用了某注解的类、或者实现了某接口的类等。故需要创建一个 [ClassUtil](https://github.com/seyvoue/smart-web/blob/master/smart-framework/src/main/java/com/seyvoue/demo/framework/util/ClassUtil.java) 工具类，提供与类操作相关的方法。

step1: 获取类加载器
step2：加载类
step3：获取指定包名下的所有类


