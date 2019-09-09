<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190909151554.png"/></div>

**原创文章，转载请注明出处**

**2019 年 05 月 06 日：请勿以任何方式转载！**

## 《垃圾回收的算法与实现》笔记

## GC 复制算法

- 只把某个空间里的活动对象复制到其他空间，把原空间里的所有对象都回收掉。

名词解释：

`From 空间` ：复制对象的原空间。

`To 空间` ：粘贴活动对象的新空间。

当 From 空间被完全占满时，GC 会将活动对象全部复制到 To 空间。当复制对象全部完成后，此算法会把 From 空间和 To 空间互换，GC 也就结束了。 From 空间必须和 To 空间大小完全一致。

GC 复制算法的 copying () 函数

```java
copying() {
	//$free 分块开头的变量，并设置在 To 空间的开头
    $free = $to_start
    //复制能从根引用的对象
    for (r : $roots) {
        *r = copy(*r)
    }
    
    //swap : 交换两个对象
    swap($from_start, $to_start)
}
```

GC 复制算法的 copy () 函数（copy () 函数将作为参数给出的对象复制，再递归复制其子对象）

```
copy(obj) {
	//检查 obj 的复制是否已经完成，obj.tag 是一个域，表示 obj 的复制是否完成。obj.tag 是obj.field1 的别名
	if (obj.tag !=COPIED) {
        copy_data($free, obj, obj.size)
        obj.tag = COPIED
        obj.forwarding = $free
        $free += obj.size
	}
	
	for (child : children(obj.forwarding)) {
        *child = copy(*child)
	}
	
	return obj.forwarding
}
```

### GC 复制算法的优缺点

#### 优点

- 优秀的吞吐量

  > 它只搜索并复制活动对象，堆越大，差距越明显。

- 可实现高速分配

- 不会发生碎片化

- 与缓存兼容

#### 缺点

- 堆使用效率低下
- 不兼容保守式 GC 算法
- 递归调用函数