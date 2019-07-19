## SpringBoot 注解

> [微信公众号文章](https://mp.weixin.qq.com/s?__biz=MzI4Njc5NjM1NQ==&mid=2247488481&idx=1&sn=af83dc6907ff0dead4dff0a105405f2f&chksm=ebd62ccddca1a5db20871cf88fa75ccdce5d1f3fd0ed288b3c104604c2234a5ae5602cf521f0&scene=21#wechat_redirect)
>
> 作者：SnailClimb 链接：https://juejin.im/post/5ce69379e51d455d877e0ca0来源：掘金著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

1.核心注解 `@SpringBootApplication`

**重点的三个注解：**`@SpringBootConfiguration，@EnableAutoConfiguration，@ComponentScan`

- Ctrl + 左键 点进去可以看到源码如下

~~~java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
	
}
~~~

- `@SpringBootConfiguration` 源码如下：

~~~java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {

}
~~~

- `@Configuration` 源码如下：

~~~java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
~~~

**解释**：`@Configuration` 能让我们去注册一些额外的`Bean`，并且导入一些额外的配置；`@Configuration` 还有一个作用就是把该类变成一个配置类，不需要额外的 XML 进行配置。所以 `@SpringBootConfiguration` 就相当于 `@Configuration`。





- 控制反转：控制权的转换
  - "对象a 依赖了对象 b，当对象 a 需要使用 对象 b的时候必须自己去创建。但是当系统引入了 IOC 容器后， 对象a 和对象 b 之前就失去了直接的联系。这个时候，当对象 a 需要使用 对象 b的时候， 我们可以指定 IOC 容器去创建一个对象b注入到对象 a 中"。 对象 a 获得依赖对象 b 的过程,由主动行为变为了被动行为，控制权翻转，这就是控制反转名字的由来。
- **DI(Dependecy Inject,依赖注入)是实现控制反转的一种设计模式，依赖注入就是将实例变量传入到一个对象中去。**



- AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

  **Spring AOP 就是基于动态代理的**，如果要代理的对象，实现了某个接口，那么Spring AOP会使用**JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用**Cglib** ，这时候Spring AOP会使用 **Cglib** 生成一个被代理对象的子类来作为代理。

  