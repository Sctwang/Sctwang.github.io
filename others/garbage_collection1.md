<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190909151008.png"/></div>

**原创文章，转载请注明出处**

## 《垃圾回收的算法与实现》笔记

### `***` GC 算法的性能评价标准

- 吞吐量

  吞吐量（throughput）指的是 “在单位时间内的处理能力”。人们通常喜欢吞吐量高的 GC 算法

- 最大暂停时间

  本书介绍的所有 GC 算法，都会在 GC 执行过程中令 mutator 暂停执行。最大暂停时间指的是 “因执行 GC 而暂停执行 mutator 的最长时间”。

- 堆使用效率

  影响堆使用效率的因素有两个：头的大小；堆的用法。

  头的大小：堆中存放的信息越多， GC 的效率越高；头越小越好，因此为了执行 GC ，需要把在头中对方的信息控制在最小限度。

  堆的用法：所使用的回收算法；例如标记 - 清除算法和引用计数法的效率差异。

- 访问的局部性

  PC 上有 4 种存储器：寄存器、缓存、内存（主存储器）、辅助存储器（硬盘等）。

### 名词解释

`mutator` ： 有 “改变某物” 的意思，即改变 GC 对象之间的引用关系。

`allocation`：分配

`allocator`：分配器

## GC 标记 - 清除算法

> 由标记阶段和清除阶段组成；标记阶段是把所有活动对象都做上标记的阶段，清除阶段是把那些没有标记的对象，也就是非活动对象回收的阶段。

- 标记 - 清除算法伪代码：mark_sweep () 函数

  ```java
  mark_sweep() {
      mark_phase()
      sweep_phase()
  }
  ```

  - 标记阶段 mark_phase () 函数：给堆里所有的活动对象进行标记

    ```java
    mark_phase() {
        for (r : $roots){
            mark(*r)
        }
    }
    
    //mark 函数
    mark(obj) {
    //检查作为实参的 obj 是否已经被标记
        if (obj.amrk == FALSE) {
            obj.mark == TRUE
            for (child : children) {
                mark(*children)
            }
        }
    }
    ```

  - 清除阶段 sweep_phase () 函数：collector 遍历整个堆，回收没有打上标记的对象（即垃圾），使其能再次得到利用

    ```java
    sweep_phase() {
    	//$heap_start 是堆首地址，使用变量 sweeping 遍历堆
        sweeping = $heap_start
        while (seeeping < $heap_end) {
            if (sweeping.mark == TRUE) {
            	//如果为 TRUE，说明这个对象是活动对象，不能回收；然后准备下一次回收
                sweeping.mark = FALSE
            } else {
            	//重写 sweeping 的域;生成、使用空闲链表的时候进行这一步；next 表示对象（分块）最初的域，即 filed1
                sweeping.next = $free_list
                $free_list = sweeping
            }
            //size 域是存储对象大小（字节数）的域，和 mark 域一样，事先在头中定义
            sweeping += sweeping.size
        }
    }
    ```

    把未标记的对象（要回收的对象）作为分块，连接到被称为 “空闲链表” 的单项链表，之后遍历这个链表，就可以找到分块了。

##### 深度优先搜索与广度优先搜索

- 比较内存使用量（已存储的对象数量）可以知道，深度优先搜索比广度优先搜索更能 压低内存使用量，因此，标记阶段常使用深度优先搜索。

我们在搜索对象病进行标记的时候使用的是 深度优先搜索 （depth-first search），这是尽可能从深度上搜索树形结构的算法；另一方面，还有 广度优先搜索（breath-first search），这是尽可能从广度上搜索树形结构的方法。

### 分配

> 分配：对回收的垃圾进行再利用

- 标记 - 清除阶段，已经把回收的垃圾（分块）放入了空闲链表；搜索空闲链表并寻找大小合适的分块就是分配操作。

执行分配函数伪代码： new_obj ()

```java
new_obj(size) {
	//pickup_chunk() 函数来遍历 $free_list,寻找大于 size 的分块，返回大于等于 size 的分块
    chunk = pickup_chunk(size, $free_list)
    if (chunk != NULL) {
        return
    } else {
        allocation_fail()
    }
}
```

- 当找到比 size 大的分块，会分割为 size 大小的分块和去掉 size 后剩下的分块，将剩下的返回空闲链表。
- 如果没找到合适大小的分块，返回 NULL，此时不进行分配，进行 allocation_fail () 函数。

### 合并

> 合并：将分配策略下产生的大量的连续的小分块进行合并；合并在清除阶段进行

执行合并函数伪代码：sweep_phase () 函数

```java
sweep_phase() {
    sweeping = $heap_start
    while(sweeping < $heap_end) {
        if (sweeping.amrk == TRUE) {
            sweeping.mark = FALSE;
        } else {
        	//用来检查这次发现的分块和上次发现的分块是否连续，如果连续，则合并
            if (sweeping == $free_list + $free_list.size) {
                $free.list.size += $free_list.size
            } else {
                sweeping.next = $free_list
                $free_list = sweeping
            }
            sweeping += sweeping.size
        }
    }
}
```

### GC 标记 - 清除算法 优缺点

#### 优点

- 算法简单，实现较容易

#### 缺点

- 碎片化（导致无数的小分块散布在堆的各处）

- 分配速度（由于分块不都是连续的，在每次分配时，都要进行遍历寻找合适的分块）

- 与写时复制技术不兼容（若没有重写对象，GC 也会设置所有活动对象的标志位，则会频繁发生复制，从而压迫到内存空间）

  > 写时复制技术（copy_on_write）时在 Linux 等中 用到的高速化方法；Linux 中，使用 fork () 函数时，大部分内存空间不会被复制，只是复制进程，实际上是将内存空间共享出去了。
  >
  > 复制后只访问这个私有空间，不访问共享内存。
  >
  > 在对共享内存空间进行写入时，不能直接重写，因为从其他程序访问时，会发生数据不一致，重写时，要复制自己私有空间的数据，对这个私有空间进行重写。

## 多个空闲链表

原来的方法只有一个空闲链表，每次分配都要遍历一遍，此方法是按照分块大小创建多个 空闲链表

创建只连接大分块的空闲链表和只连接小分块的空闲链表。

当需要的时候，只要按照 mutator 所申请的分块大小选择空闲链表就可以了。

利用多个空闲链表的时候，需要修正 函数和 sweep_phase () 函数。修改后的如下：

```java
new_obj(size) {
	index = size / (WORD_LENGTH / BYTE_LENGTH)
	if (index <= 100) {
        if ($free_list[index] != NULL) {
            chunk = $free_list[index]
            $free_list[index] = $free_list[index].next
            return chunk
        }
	} else {
        chunk = pickup_chunk(size, $free_list[101])
        if (chunk != NULL) {
            return chunk
        }
	}
	allocation_fail()
}
```

### BIBOP (Big Bag Of Pages) 方法

- 将大小相近的对象整理成固定大小的块进行管理的做法；把堆分割成固定大小的块，让每个块只能配置同样大小的对象。

### 位图标记

- 只收集各个对象的标志位并表格化，不跟对象一起管理。在标记的时候，不在对象的头里置位，而是在这个表格中的特定场所置位。像这样集合了用于标记的为的表格叫做 “位图表格”，利用这个表格进行标记的行为叫做 “位图标记”。

位图标记中的 mark () 函数

```java
mark(obj) {
	//WORD_LENGTH 是常量，表示各机器中 1 个字位宽（例如，32 位机器中对应的值是 32）;obj_num 是从位图表格前面数起，obj 的标志位在第几个
    obj_num = (obj - $heap_start) / WORD_LENGTH
    //表示位图表格的行编号
    index = obj_num / WORD_LENGTH
    //表示位图表格的列编号
    offset = obj_num % WORD_LENGTH
    
    if (($bitmap_tbl[index] & (1 << offset)) == 0) {
        $bitmap_tbl[index] |= (1 << offset)
        for (child : children(obj)) {
            mark(*child)
        }
    }
}
```

### 位图标记的优点

#### 优点

- 与写时复制技术兼容
- 清除操作更高效

### 延迟清除法

- 目的是缩减因清除操作而导致的 mutator 最大暂停时间的方法

在标记操作结束后，不一定进行清除操作，而是如其字面意思一样让它 “延迟”，通过延迟来防止 mutator 长时间暂停。

### 延迟清除算法

- 在分配时执行必要的遍历，并不是一下遍历整个堆，所以可压缩因清除操作而导致的 mutator 的暂停时间。

new_obj () 函数

```java
new_obj(size) {
    chunk = lazy_sweep(size)
    if (chunk != NULL) {
        return chunk
    }
    
    mark_phase()
    //调用 lazy_sweep() 函数，进行清除操作；若能用清除来分配块，则返回分块；若不能清除，则标记
    chunk = lazy_sweep(size)
    if (chunk != NULL) {
        return chunk
    }
    
    allocation_fail()
}
```

lazy_sweep () 函数

> 此函数会一直遍历堆，直到找到大于等于所申请大小的分块为止。再找到合适分块时会将其返回。
>
> $sweeping 变量是全局变量，也就是说，遍历的开始为止位于上一次清除操作中发现的分块右边。
>
> 若没有找到分块，返回 NULL。

```java
lazy_sweep(size) {
    while($sweeping < $heap_end) {
        if ($sweeping.mark == TRUE) {
            $sweeping.mark == FALSE
        } else if ($sweeping.size >= size) {
            chunk = $sweeping
            $sweeping += sweeping.size
            return chunk
        }
        $sweeping += $sweeping.size
        $sweeping = $heap_start
        return NULL
    }
}
```