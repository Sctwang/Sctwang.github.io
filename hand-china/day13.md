## 20190723-SpringBoot 系列

### 1.和数据相关的

- DataSource / JDBCTemplate
- JPA：
  - SpringBoot Data JPA
  - 数据校验
  - 返回数据处理
- MyBatis



- JPA

> JPA 是 Hibernate 的一个抽象

- 包含三个方面的技术
  - ORM
  - 持久化
  - 查询

属性配置中要注意的：

~~~java
spring.jpa.hibernate.ddl-auto 				数据库自动校验模式
spring.jpa.show-sql 						在日志中打印 Sql
mybatis.config-location MyBatis 			配置加载路径
mybatis.mapper-locations MyBatis 			Mapper加载路径
~~~



### SpringBoot - 功能扩展

**1.多种启动方式**

- IDEA启动：java [options] classname [args]
- JDK：java –jar target/{application-name}.jar
-  Maven：mvn spring-boot:run
- Gradle：gradle bootRun

**2.配置文件**

- 自定义属性：@Value
- 将属性赋值给Bean：@ConfigurationProperties
- 自定义配置文件：@PropertySource
- 多环境配置文件：application-{profile}.properties

**3.配置文件加载顺序**

- 命令行参数
- 当前环境变量或者系统变量中的 “ SPRING_APPLICATION_JSON ” 属性
- ServletConfig 初始化参数
- ServletContext 初始化参数
- “ java:comp/env ” 中的JNDI属性
- Java系统变量 “System.getProperties()”
- 操作系统环境变量
- Jar包外指定的 application-{profile}.properties
- Jar包内指定的 application-{profile}.properties
- Jar包外的 application.properties
- Jar 包内的 application.properties
-  @PropertySource 引入的配置
- 默认配置属性 “SpringApplication.setDefaultProperties”