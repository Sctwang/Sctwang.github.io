### 单例模式

- 通过单例模式可以保证系统中，应用该模式的一个类只有一个实例。即一个类只有一个对象实例。

#### 懒汉式-线程不安全

- 对象是在方法被调用的时候进行初始化；这也叫做对象的延时加载或者延迟实例化，当 Singleton 类进入内存，对象还没有存在，只有在调用 getUniqueInstance() 方法的时候，才会建立对象。


>
> 优点：
> 节约资源，在使用的时候在进行对象实例化

缺点：
> 在多线程环境下是不安全的，如果多个线程能够同时进入 <code>if (singleton == null)</code> ，并且此时 <code>singleton</code> 为 
>   <code>null</code>，那么会有多个线程执行 <code>singleton = new Singleton();</code> 语句，这将导致实例化多次 <code>singleton</code>。


~~~java
public class Singleton {
    private static Singleton singleton;
    
    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
        if (singleton == null) {
            singleton = new Singleton();
        }
        return singleton;
    }

}
~~~



#### 懒汉式-线程安全

- 在原来的基础上加锁，确保线程安全，同一时刻只能有一个线程进入该方法

优点：
> 线程安全

缺点：
> 降低了性能，每个因为加锁导致每一个时刻只能有一个线程进入

~~~java
public static synchronized Singleton getUniqueInstance() {
    if (singleton == null) {
        singleton = new Singleton();
    }
    return singleton;
}
~~~

#### 饿汉式-线程安全
- 饿汉式：提前进行对象实例化

优点：
> 线程安全

缺点：
> 占用资源，提前实例化了对象

~~~java
private static Singleton singleton = new Singleton();
~~~