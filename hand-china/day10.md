## SpringBoot 注解

> [链接](https://mp.weixin.qq.com/s?__biz=MzI4Njc5NjM1NQ==&mid=2247488481&idx=1&sn=af83dc6907ff0dead4dff0a105405f2f&chksm=ebd62ccddca1a5db20871cf88fa75ccdce5d1f3fd0ed288b3c104604c2234a5ae5602cf521f0&scene=21#wechat_redirect)

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

