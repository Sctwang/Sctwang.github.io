## 20191225 - 项目记录 - Feign 与 RestTemplate

描述：项目中微服务之间使用 FeignClient 进行调用；oauth-server 中使用 RestTemplate 获取用户信息。

问题：两者有什么不同？

1. FeignClient

- FeignClient：pom 文件中加入 Feign 依赖

  ~~~yml
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
      <version>2.0.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>io.github.openfeign</groupId>
      <artifactId>feign-core</artifactId>
      <version>9.7.0</version>
  </dependency>
  <dependency>
      <groupId>io.github.openfeign</groupId>
      <artifactId>feign-slf4j</artifactId>
      <version>9.7.0</version>
  </dependency>
  ~~~

- 编写 FeignClient 代码

  ~~~java
  @FeignClient(name = "myFeignClient", url = "http://127.0.0.1:8001")
  public interface MyFeignClient {
      @RequestMapping(method = RequestMethod.GET, value = "/participate")
      String getCategorys(@RequestParam Map<String, Object> params);
  }
  ~~~

- 直接使用 FeignClient

  ~~~java
  @Autowired
  MyFeignClient myFeignClient;
  ~~~

  这时候， FeignClient 就可以直接调用其它微服务的接口了。

2. RestTemplate

   源码的解释为：`Synchronous client to perform HTTP requests, exposing a simple, template method API over underlying HTTP client libraries such as the JDK, Apache HttpComponents, and others.` 

   --->

   `同步客户端执行HTTP请求，在基础HTTP客户端库（如JDK，Apache HttpComponents等）上公开简单的模板方法API。`

- 使用方法：需要在启动类中加入

  ~~~java
  @Bean
  public RestTemplate restTemplate() {
    return new RestTemplate();
  }
  ~~~

  然后在 Controller 层中引入 RestTemplate

  ~~~java
  @Autowired
  private RestTemplate restTemplate;
  ~~~

  



### 参考

- [关于 FeignClient 使用大全](https://www.jianshu.com/p/0834508b7a6d)
- [RestTemplate 详解](https://zhuanlan.zhihu.com/p/31681913)