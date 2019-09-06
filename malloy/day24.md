```java
import com.hand.exam.enums.RoleEnums;

import java.lang.annotation.*;

/**
 * @author wyz
 * @date 2019/7/20 11:28
 *
 *
 * 自定义注解
 *
 * 官方链接:https://docs.spring.io/spring-boot/docs/1.5.2.RELEASE/reference/htmlsingle/#boot-features-bean-conditions
 *
 * @Documented  //注解表明制作javadoc时，是否将注解信息加入文档。如果注解在声明时使用了@Documented，则在制作javadoc时注解信息会加入javadoc。
 *
 * @Target：用于描述注解的使用范围
 *
 * @Target(ElementType.TYPE) //接口、类、枚举、注解
 * @Target(ElementType.FIELD) //字段、枚举的常量
 * @Target(ElementType.METHOD) //方法
 * @Target(ElementType.PARAMETER) //方法参数
 * @Target(ElementType.CONSTRUCTOR) //构造函数
 * @Target(ElementType.LOCAL_VARIABLE)//局部变量
 * @Target(ElementType.ANNOTATION_TYPE)//注解
 * @Target(ElementType.PACKAGE) ///包
 *
 * @Retention(RetentionPolicy.SOURCE) —— 这种类型的Annotations只在源代码级别保留,编译时就会被忽略
 *
 * @Retention(RetentionPolicy.CLASS) —— 这种类型的Annotations编译时被保留,在class文件中存在,但JVM将会忽略
 * @Retention(RetentionPolicy.RUNTIME) —— 这种类型的Annotations将被JVM保留,所以他们能在运行时被JVM或其他使用反射机制的代码所读取和使用.
 */

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
/*
    类继承关系中 @Inherited 的作用
    类继承关系中，子类会继承父类使用的注解中被 @Inherited 修饰的注解
    接口继承关系中 @Inherited 的作用
    接口继承关系中，子接口不会继承父接口中的任何注解，不管父接口中使用的注解有没有被 @Inherited 修饰
    类实现接口关系中 @Inherited 的作用
    类实现接口时不会继承任何接口中定义的注解
*/
@Inherited
public @interface AccessAnnotation {
    RoleEnums[] roles() default RoleEnums.WM_NORMAL;
}
```