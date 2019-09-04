## 注解

注解就是某种注解类型的一种实例，我们可以把它用在某个类上进行标注。



### 元注解

> 元注解的作用是负责注解其他注解

~~~java
@Target
@Retention
@Documented
@Inherited
~~~

- Target

  - @Target 说明了 Annotation 所修饰的对象范围：Annotation 可被用于 packages、types（类、接口、枚举、Annotation 类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch 参数）。在 Annotation 类型的声明中使用了 target 可更加明晰其修饰的目标。

  - ##### 作用：用于描述注解的使用范围（即：被描述的注解可以用在什么地方）

    ##### 取值 (ElementType) 有：

    1. CONSTRUCTOR: 用于描述构造器
    2. FIELD: 用于描述符
    3. LOCAL_VARIABLE: 用于描述局部变量
    4. METHOD: 用于描述方法
    5. PACKAGE: 用于描述包
    6. PARAMETER: 用于描述参数
    7. TYPE: 用于描述类、接口（包括注解类型）或者 enum 声明