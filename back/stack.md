## Stack 栈



~~~java

    public class Stack<E> extends Vector<E> {
        Stack();
        push(E); E
        pop(); E
        peek(); E
        empty(); boolean
        search(Object); int
    }

~~~



- push() : 把项压入堆栈顶部。
  - 返回：`item` 参数。

~~~java

    public E push(E item) {
        addElement(item);

        return item;
    }

~~~



- pop() : 移除堆栈顶部的对象，并作为此函数的值返回该对象。
  - 返回：堆栈顶部的对象（ `Vector` 对象中的最后一项）。

~~~java

    public synchronized E pop() {
        E obj;
        int len = size();

        obj = peek();
        removeElementAt(len - 1);

        return obj;
    }

~~~



- peek() : 查看堆栈顶部的对象，但不从堆栈中移除它。
  - 返回：当且仅当堆栈中不含任何项时返回 `true`；否则返回 `false`。

~~~java


    public synchronized E peek() {
        int len = size();

        if (len == 0)
            throw new EmptyStackException();
        return elementAt(len - 1);
    }

~~~



- empty() : 测试堆栈是否为空。
  - 返回：当且仅当堆栈中不含任何项时返回 `true`；否则返回 `false`。

~~~java
	
	public boolean empty() {
        return size() == 0;
    }

~~~



- search() : 返回对象在堆栈中的位置，以 1 为基数。如果对象 `o` 是堆栈中的一个项，此方法返回距堆栈顶部最近的出现位置到堆栈顶部的距离；堆栈中最顶部项的距离为 `1`。使用 `equals` 方法比较 `o` 与堆栈中的项。

~~~java

    public synchronized int search(Object o) {
        int i = lastIndexOf(o);

        if (i >= 0) {
            return size() - i;
        }
        return -1;
    }

~~~





