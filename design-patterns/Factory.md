### 工厂模式
- 工厂模式作用：把对象的生产和使用过程分开
#### 简单工厂
- 简单工厂又叫静态工厂(Static Factory Method)

优点：
> 工厂进行生产操作，客户不需要知道对象是怎么被生产的，只需要使用就可以

缺点：
> 不易于扩展，当业务需求改变的时候，需要修改源代码
~~~java
public class SimpleFactory {

    public Product createProduct(int type) {
        if (type == 1) {
            return new ConcreteProduct1();
        } else if (type == 2) {
            return new ConcreteProduct2();
        }
        return new ConcreteProduct();
    }
}
~~~

~~~java
public class Client {

    public static void main(String[] args) {
        SimpleFactory simpleFactory = new SimpleFactory();
        Product product = simpleFactory.createProduct(1);
        // do something with the product
    }
}

~~~


#### 工厂方法



#### 抽象工厂