作者：特立独行的猪手
链接：https://juejin.im/post/58f2f47061ff4b0058f4b7cc
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



# 源码分析之 HashMap

### 概念

`HashMap`是`Java Collections Framework`中`Map`集合的一种实现。`HashMap`提供了一种简单实用的数据存储和读取方式。`Map`接口不同于`List`接口，属于集合框架的另一条支线，`Map`提供了键值对`K-V`数据存储模型，底层则是通过`Hash`表存储。



本文分析基于`JDK1.8`。

### 类结构



![hashmap-structure](https://mortre-picgo.oss-cn-beijing.aliyuncs.com/hashmap-structures.jpg)



`HashMap`实现了`Map`接口，`Map`接口设置一系列操作`Map`集合的方法，如：`put`、`get`、`remove`...等方法，而`HashMap`也针对此有其自身对应的实现。

`HashMap`继承`AbstractMap`类。`AbstractMap`类对于`Map`接口做了基础的实现，实现了`containsKey`、`containsValue`...等方法。

### 类成员

#### 构造函数

`HashMap`提供四种构造函数。最为基础是如下这种：

##### HashMap(int initialCapacity, float loadFactor)

```
   public HashMap(int initialCapacity, float loadFactor) {

        ...        

        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }复制代码
```

`HashMap(int initialCapacity, float loadFactor)`是最基础的构造函数。该构造函数提供两个参数`initialCapacity(初始大小)`、`loadFactor(加载因子)`。

- `initialCapacity`默认值是`16 (1 << 4)`,最大值是 `1073 741 824(1 << 30)`，且大小必须是小于最大值的2的幂次方；
- `loadFactor`默认值是`0.75`，作用是扩容时使用；

初始化的过程中将传入的参数`loadFactor`赋值给`this.loadFactor`，然后调用`tableSizeFor(initialCapacity)`方法将处理的结果值赋值给`this.threshold`;

`threshold`是`HashMap`判断`size`是否需要扩容的阈值。这里调用`tableSizeFor(initialCapacity)`来设置`threshold`;

##### tableSizeFor有啥用？

先抛出答案：`tableSizeFor`方法保证函数返回值是大于等于给定参数`initialCapacity`最小的2的幂次方的数值。

如何实现？

```java
 static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }复制代码
```

可以看出该方法是一系列的二进制位操作。先说明 `|=`的作用：`a |= b 等同于 a = a|b`。逐行分析`tableSizeFor`方法：

- `int n = cap - 1`

  - 给定的`cap`减1，是为了避免参数`cap`本来就是`2`的幂次方，这样一来，经过后续的未操作的，`cap`将会变成`2 * cap`,是不符合我们预期的。

- `n |= n >>> 1`

  - `n >>> 1`，`n`无符号右移`1`位，即`n`二进制最高位的`1`右移一位；

  - `n | (n >>> 1)`，导致的结果是`n`二进制的高`2`位值为`1`;

    目前`n`的高`1~2`位均为`1`。

- `n |= n >>> 2`

  - `n`继续无符号右移`2`位。

  - `n | (n >>> 2)`，导致`n`二进制表示高`3~4`位经过运算值均为`1`；

    目前`n`的高`1~4`位均为`1`。

- `n |= n >>> 4`

  - `n`继续无符号右移`4`位。

  - `n | (n >>> 4)`，导致`n`二进制表示高`5~8`位经过运算值均为`1`；

    目前`n`的高`1~8`位均为`1`。

- `n |= n >>> 8`

  - `n`继续无符号右移`8`位。

  - `n | (n >>> 8)`，导致`n`二进制表示高`9~16`位经过运算值均为`1`；

    目前`n`的高`1~16`位均为`1`。

- `n |= n >>> 16`

  - `n`继续无符号右移`16`位。

  - `n | (n >>> 16)`，导致`n`二进制表示高`17~32`位经过运算值均为`1`；

    目前`n`的高`1~32`位均为`1`。

可以看出，无论给定`cap(cap < MAXIMUM_CAPACITY )`的值是多少，经过以上运算，其值的二进制所有位都会是`1`。再将其加`1`，这时候这个值一定是`2`的幂次方。当然如果经过运算值大于`MAXIMUM_CAPACITY`，直接选用`MAXIMUM_CAPACITY`。

这里可以举个栗子，假设给定的`cap`的值为`20`。

- `int n = cap - 1;` —> `n = 19(二进制表示：0001 0011)`
- `n |= n >>> 1;`

```
    n             ->  0001 0011
    n >>> 1       ->  0000 1001
    n |= n >>> 1  ->  0001 1011复制代码
```

- `n |= n >>> 2;`

```
    n             ->  0001 1011
    n >>> 2       ->  0000 1101
    n |= n >>> 2  ->  0001 1111复制代码
```

此时`n`所有位均为1，后续的位操作均不再改变`n`的值。

...

```
    n + 1        ->  0010 0000 (32)复制代码
```

最终，`tableSizeFor(20)`的结果为`32(2^5)`。

至此`tableSizeFor`如何保证`cap`为`2`的幂次方已经显而易见了。那么问题来了，为什么`cap`要保持为`2`的幂次方？

##### 为什么cap要保持为2的幂次方？

`cap`要保持为2的幂次方主要原因是`HashMap`中数据存储有关。

在`JDK1.8`中，`HashMap`中`key`的`Hash`值由`Hash(key)`方法（后面会详细分析）计算得来。

`HashMap`中存储数据`table`的`index`是由`key`的`Hash`值决定的。在`HashMap`存储数据的时候，我们期望数据能够均匀分布，以避免哈希冲突。自然而然我们就会想到去用`%`取余的操作来实现我们这一构想。

这里要了解到一个知识：**取余(%)操作中如果除数是2的幂次方则等同于与其除数减一的与(&)操作**。

这也就解释了为啥一定要求`cap`要为`2`的幂次方。再来看看`table`的`index`的计算规则：

```java
 index = e.hash & (newCap - 1) 

 等同于：

 index = e.hash % newCap复制代码
```

采用二进制位操作`&`，相对于`%`，能够提高运算效率，这就是要求`cap`的值被要求为`2`幂次方的原因。

#### Node 类

```java
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }复制代码
```

`Node<K,V> 类`是`HashMap`中的静态内部类，实现`Map.Entry<K,V>`接口。 定义了`key`键、`value`值、`next`节点，也就是说元素之间构成了单向链表。

#### Node[] table

`Node<K,V>[] table`是`HashMap`底层存储的数据结构，是一个`Node`数组。上面得知`Node`类为元素维护了一个单向链表。

至此，`HashMap`存储的数据结构也就很清晰了：维护了一个数组，每个数组又维护了一个单向链表。之所以这么设计，考虑到遇到哈希冲突的时候，同`index`的`value`值就用单向链表来维护。

#### hash(K,V) 方法

`HashMap`中`table`的`index`是由`Key`的哈希值决定的。`HashMap`并没有直接使用`key`的`hashcode()`，而是经过如下的运算：

```java
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }复制代码
```

而上面我们提到`index`的运算规则是`e.hash & (newCap - 1)`。由于`newCap`是`2`的幂次方，那么`newCap - 1`的高位应该全部为`0`。如果`e.hash`值只用自身的`hashcode`的话，那么`index`只会和`e.hash`低位做`&`操作。这样一来，`index`的值就只有低位参与运算，高位毫无存在感，从而会带来哈希冲突的风险。所以在计算`key`的哈希值的时候，用其自身`hashcode`值与其低`16`位做异或操作。这也就让高位参与到`index`的计算中来了，即降低了哈希冲突的风险又不会带来太大的性能问题。

#### put(K,V) 方法

```java
  public V put(K key, V value) {
      return putVal(hash(key), key, value, false,  true);
  }

  final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            // tab 为空,调用resize()初始化tab。
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            // key没有被占用的情况下,将value封装为Node并赋值
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                // 如果key相同,p赋值给e
                e = p;
            else if (p instanceof TreeNode)
                // 如果p是红黑树类型，调用putTreeVal方式赋值
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                // index 相同的情况下
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        // 如果p的next为空，将新的value值添加至链表后面
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            // 如果链表长度大于8，链表转化为红黑树，执行插入
                            treeifyBin(tab, hash);
                        break;
                    }
                    // key相同则跳出循环
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                //根据规则选择是否覆盖value
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            // size大于加载因子,扩容
            resize();
        afterNodeInsertion(evict);
        return null;
    }复制代码
```

在构造函数中最多也只是设置了`initialCapacity`、`loadFactor`的值，并没有初始化`table`，`table`的初始化工作是在`put`方法中进行的。

#### resize() 方法

```java
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            // table已存在
            if (oldCap >= MAXIMUM_CAPACITY) {
                // oldCap大于MAXIMUM_CAPACITY,threshold设置为int的最大值
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                //newCap设置为oldCap的2倍并小于MAXIMUM_CAPACITY，且大于默认值, 新的threshold增加为原来的2倍
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            // threshold>0, 将threshold设置为newCap,所以要用tableSizeFor方法保证threshold是2的幂次方
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            // 默认初始化，cap为16，threshold为12。
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            // newThr为0，newThr = newCap * 0.75
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
            // 新生成一个table数组
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            // oldTab 复制到 newTab
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                       // 链表只有一个节点，直接赋值
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        // e为红黑树的情况
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }复制代码
```

#### remove(key) 方法 & remove(key, value) 方法

`remove(key)` 方法 和 `remove(key, value)` 方法都是通过调用`removeNode`的方法来实现删除元素的。

```java
     final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        Node<K,V>[] tab; Node<K,V> p; int n, index;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (p = tab[index = (n - 1) & hash]) != null) {
            Node<K,V> node = null, e; K k; V v;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                // index 元素只有一个元素
                node = p;
            else if ((e = p.next) != null) {
                if (p instanceof TreeNode)
                    // index处是一个红黑树
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    // index处是一个链表，遍历链表返回node
                    do {
                        if (e.hash == hash &&
                            ((k = e.key) == key ||
                             (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
            // 分不同情形删除节点
            if (node != null && (!matchValue || (v = node.value) == value ||
                                 (value != null && value.equals(v)))) {
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                else if (node == p)
                    tab[index] = node.next;
                else
                    p.next = node.next;
                ++modCount;
                --size;
                afterNodeRemoval(node);
                return node;
            }
        }
        return null;
    }
```