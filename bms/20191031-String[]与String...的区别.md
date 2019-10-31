## 20191031 - String[] 与 String... 的区别

> 概述：记录入参为（String... foo）和 (String[] foo) 的区别



- 例如

~~~java
public static void test(String... foo) {
    System.out.println("foo");
}

public static void test(String[...] foo) {
    System.out.println("foo");
}
~~~



- 两种方法的区别（这两种入参在内部实现上是一样的）
  - 当入参为 String[] foo 时，调用这个方法的时候，入参只能为一个 String 数组
  - 当入参为 String... foo 时，调用这个方法的时候，入参可以为很多种



String... foo 入参示例：(对应调用的注释已经说明了运行结果)

~~~java
public void myMethod( String... foo ) {
    // do something
    // foo is an array (String[]) internally
    System.out.println( foo[0] );
}

myMethod( "a", "b", "c" );

// OR
myMethod( new String[]{ "a", "b", "c" } );

// OR without passing any args
myMethod();
~~~

String[] foo 入参示例：(对应调用的注释已经说明了运行结果)

~~~java
public void myMethod( String[] foo ) {
    // do something
    System.out.println( foo[0] );
}

// compilation error!!!
myMethod( "a", "b", "c" );

// compilation error too!!!
myMethod();

// now, just this works
myMethod( new String[]{ "a", "b", "c" } );
~~~



#### 参考链接：

- [Stackoverflow](https://stackoverflow.com/questions/11973505/what-is-the-difference-between-string-and-string-in-java)