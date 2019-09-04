转载自 [JavaGuide](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485576&idx=1&sn=f993349f12650a68904e1d99d2465131&chksm=cea24743f9d5ce55ffe543a0feaf2c566382024b625b59283482da6ab0cbcf2a3c9dc5b64a53&scene=0&xtrack=1#rd)



## Spring

- 核心技术 ：依赖注入 (DI)，AOP，事件 (events)，资源，i18n，验证，数据绑定，类型转换，SpEL。

- 测试 ：模拟对象，TestContext 框架，Spring MVC 测试，WebTestClient。

- 数据访问 ：事务，DAO 支持，JDBC，ORM，编组 XML。

- Web 支持 : Spring MVC 和 Spring WebFlux Web 框架。

- 集成 ：远程处理，JMS，JCA，JMX，电子邮件，任务，调度，缓存。

- 语言 ：Kotlin，Groovy，动态语言。

### Spring 模块
- Spring Core： 基础，可以说 Spring 其他所有的功能都需要依赖于该类库。主要提供 IoC 依赖注入功能。

- Spring Aspects ：该模块为与 AspectJ 的集成提供支持。

- Spring AOP ：提供了面向切面的编程实现。

- Spring JDBC : Java 数据库连接。

- Spring JMS ：Java 消息服务。

- Spring ORM : 用于支持 Hibernate 等 ORM 工具。

- Spring Web : 为创建 Web 应用程序提供支持。

- Spring Test : 提供了对 JUnit 和 TestNG 测试的支持。

#### @RestController 和 @Controller

`Controller` 返回一个页面，单独使用 `@Controller` 不加 `@ResponseBody` 的话一般使用在要返回一个视图的情况，这种情况属于比较传统的 Spring MVC 的应用，对应于前后端不分离的情况。

#### @RestController 返回 JSON 或 XML 形式数据

`@RestController`只返回对象，对象数据直接以 JSON 或 XML 形式写入 HTTP 响应（Response）中，这种情况属于 RESTful Web 服务，这也是目前日常开发所接触的最常用的情况（前后端分离）。

#### @Controller +@ResponseBody 返回 JSON 或 XML 形式数据

如果你需要在 Spring4 之前开发 RESTful Web 服务的话，你需要使用 `@Controller` 并结合 `@ResponseBody` 注解，也就是说 `@Controller` +`@ResponseBody`= `@RestController`（Spring 4 之后新加的注解）。


