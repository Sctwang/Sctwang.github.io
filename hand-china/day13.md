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

### MyBatis 的 .xml 配置

> [CSDN博客链接](https://blog.csdn.net/qq_39291929/article/details/80030434)

- 一对一
  - resultMap
  - association; 对应的标签属性：
    - property			对象属性的名称
    - javaType	        对象属性的类型	
    - column              所对应的外键字段名称
    - select                 使用另一个查询封装的结果

~~~java
<!--resultMap-->
    
<resultMap id="baseResultMap" type="com.hand.train.wm.domain.User">
    <result column="id" property="id" jdbcType="DECIMAL"/>
    <result column="name" property="name" jdbcType="VARCHAR"/>
    <result column="age" property="age" jdbcType="DECIMAL"/>
    <result column="sex" property="sex" jdbcType="VARCHAR"/>
</resultMap>
~~~


- 一对多

  - 主要是 clollection（例如一个客户对应多个订单）


~~~java
<!--resultMap-->
    
<resultMap id="baseResultMap" type="com.hand.train.wm.domain.User">
    <result column="id" property="id" jdbcType="DECIMAL"/>
    <result column="name" property="name" jdbcType="VARCHAR"/>
    <result column="age" property="age" jdbcType="DECIMAL"/>
    <result column="sex" property="sex" jdbcType="VARCHAR"/>
    <!--一对多-->
    <collection property="roleList" ofType="com.hand.train.wm.domain.Role">
        <id property="id" column="user_id"/>
        <result column="id" property="id" jdbcType="DECIMAL"/>
        <result column="name" property="name" jdbcType="VARCHAR"/>
        <result column="user_id" property="userId" jdbcType="DECIMAL"/>
        <result column="code" property="code" jdbcType="VARCHAR"/>
        <result column="description" property="description" jdbcType="VARCHAR"/>
    </collection>
</resultMap>
~~~



- 多对多
  - 例如用户和角色的关系

~~~java
<resultMap id="findManyToManyMap" type="user">
        <!--主表 user 表-->
        <id column="user_id" property="id"/>
        <result column="username" property="userName"/>
        <result column="sex" property="sex"/>
        <result column="address" property="address"/>
        <!--从表 order 表映射到 orderList-->
        <collection property="orderList" ofType="com.ghc.pojo.Orders">
            <id column="id" property="id"/>
            <result column="user_id" property="user_id"/>
            <result column="number" property="number"/>
            <result column="createtime" property="createtime"/>
            <result column="note" property="note"/>

            <!--从表 orderDetail 表映射到 orderDetailList-->
            <collection property="orderDetailList" ofType="com.ghc.pojo.OrderDetail">
                <id column="odtid" property="id"/>
                <result column="items_id" property="itemsId"/>
                <result column="items_num" property="itemsNum"/>
                <result column="orders_id" property="ordersId"/>
                <association property="item" javaType="com.ghc.pojo.Items">
                    <id column="item_id" property="id"/>
                    <result column="name" property="name"/>
                    <result column="price" property="price"/>
                    <result column="detail" property="detail"/>
                    <result column="pic" property="pic"/>
                    <result column="createtime" property="createtime"/>
                </association>
            </collection>
        </collection>
    </resultMap>

    <select id="findManyToMany" resultMap="findManyToManyMap">
        select o.id,
       o.user_id,
       o.number,
       o.createtime,
       o.note,
        u.username,
        u.sex,
        u.address,
        odt.id as odtid,
        odt.items_id,
        odt.items_num,
        odt.orders_id,
         i.id as item_id,
        i.name,
        i.price,
        i.detail,
        i.pic,
        i.createtime
       from user u join
                       orders o on u.id = o.user_id
                   join orderdetail odt on o.id = odt.orders_id
                   join items i on odt.items_id = i.id
    </select>
~~~



### Linux

- 通过less命令打开文件，通过Shift+G到达文件底部，再通过?+关键字的方式来根据关键来搜索信息。
- 通过grep的方式查关键字，具体用法是, grep 关键字 文件名，如果要两次在结果里查找的话，就用grep 关键字1 文件名 | 关键字2 --color。最后--color是高亮关键字。
- 通过vi来编辑文件。
- 通过chmod来设置文件的权限。

- 查看日志 
  - less  /var/log/messages
  - Ctrl+F向前翻页

  - Ctrl+B向后翻页



- 搜索关键字
  - less  +/intel /var/log/messages

  - n键向前继续显示搜索结果
  - Shift+n键向后复看搜索结果