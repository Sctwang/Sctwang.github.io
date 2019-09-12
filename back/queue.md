链接地址：[GitHub](https://github.com/xingshaocheng/architect-awesome)



## 队列

### Queue

> 队列： 先进先出 （FIFO）

```java
java.util.Queue;

Queue 接口继承 Collection;
```

- 源码

```java
	public interface Queue<E> extends Collection<E> {
      // Inserts the specified element into this queue if it is possible to do so immediately without violating capacity restrictions, returning {@code true} upon success and throwing an {@code IllegalStateException} if no space is currently available.
      boolean add(E e);
      
      // Inserts the specified element into this queue if it is possible to do so immediately without violating capacity restrictions. When using a capacity-restricted queue, this method is generally preferable to {@link #add}, which can fail to insert an element only by throwing an exception.
      boolean offer(E e);
      
      // Retrieves and removes the head of this queue.  This method differs from {@link #poll poll} only in that it throws an exception if this queue is empty.
      E remove();
      
      // Retrieves and removes the head of this queue, or returns {@code null} if this queue is empty.
      E poll();
      
      // Retrieves, but does not remove, the head of this queue.  This method differs from {@link #peek peek} only in that it throws an exception if this queue is empty.
      E element();
      
      // Retrieves, but does not remove, the head of this queue, or returns {@code null} if this queue is empty.
      E peek();
  }
```

- 注解：

~~~java
	add			增加一个元索				如果队列已满，则抛出一个 IIIegaISlabEepeplian 异常
	remove   	移除并返回队列头部的元素	如果队列为空，则抛出一个 NoSuchElementException 异常
	element		返回队列头部的元素 		  如果队列为空，则抛出一个 NoSuchElementException 异常
	offer       添加一个元素并返回true     如果队列已满，则返回 false
    poll        移除并返问队列头部的元素    如果队列为空，则返回 null
	peek        返回队列头部的元素         如果队列为空，则返回 null
    put         添加一个元素              如果队列满，则阻塞
    take        移除并返回队列头部的元素   	如果队列为空，则阻塞
~~~





### LinkedList、ConcurrentLinkedQueue、LinkedBlockingQueue



关于 ConcurrentLinkedQueue 和 LinkedBlockingQueue：

- LinkedBlockingQueue 是使用锁机制，ConcurrentLinkedQueue 是使用 CAS 算法，虽然LinkedBlockingQueue 的底层获取锁也是使用的 CAS 算法

- 关于取元素，ConcurrentLinkedQueue 不支持阻塞去取元素，LinkedBlockingQueue 支持阻塞的 take() 方法，如若大家需要 ConcurrentLinkedQueue 的消费者产生阻塞效果，需要自行实现

- 关于插入元素的性能，从字面上和代码简单的分析来看 ConcurrentLinkedQueue 肯定是最快的，但是这个也要看具体的测试场景，我做了两个简单的demo做测试，测试的结果如下，两个的性能差不多，但在实际的使用过程中，尤其在多 CPU 的服务器上，有锁和无锁的差距便体现出来了，ConcurrentLinkedQueue 会比LinkedBlockingQueue 快很多。
