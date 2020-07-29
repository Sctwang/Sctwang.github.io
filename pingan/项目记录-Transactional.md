## 项目记录 - 事务回滚 - Transactional




### Propagation 属性

> 事务的传播行为，默认值为 Propagation.REQUIRED。 

可选的值有：

- Propagation.REQUIRED

  如果当前存在事务，则加入该事务，如果当前不存在事务，则创建一个新的事务。

  例如：`@Transactional（rollbackFor = Exception.class）` ，当抛出的异常类型是 Exception 时，就出发机制，进行回滚

- Propagation.SUPPORTS

  如果当前存在事务，则加入该事务；如果当前不存在事务，则以非事务的方式继续运行。

- Propagation.MANDATORY

  如果当前存在事务，则加入该事务；如果当前不存在事务，则抛出异常。

- Propagation.REQUIRES_NEW

  重新创建一个新的事务，如果当前存在事务，暂停当前的事务。

- Propagation.NOT_SUPPORTED

  以非事务的方式运行，如果当前存在事务，暂停当前的事务。

- Propagation.NEVER

  以非事务的方式运行，如果当前存在事务，则抛出异常。

- Propagation.NESTED 和 Propagation.REQUIRED 效果一样。

#### 参考

- [CSDN](https://blog.csdn.net/nextyu/article/details/78669997)
- [spring.io](https://docs.spring.io/spring/docs/4.3.13.RELEASE/spring-framework-reference/htmlsingle/#transaction-declarative-annotations)





















