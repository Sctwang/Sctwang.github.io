## 代码规范

- 《Java 开发手册》
- 版本号：v1.5.0

## Git

- git init

- git add .

- git commit -m "xxxxx..."

- git push

- git diff ...<查看不同之处>

  - ~~~
    $ git diff hand-china/
    diff --git a/hand-china/day04.md b/hand-china/day04.md
    index 518a3a6..816edfa 100644
    --- a/hand-china/day04.md
    +++ b/hand-china/day04.md
    @@ -1 +1,5 @@
    -day04
    \ No newline at end of file
    +# Java 基础
    +
    +- 代码规范
    +  - 《Java 开发手册》
    +  - 版本号：v1.5.0
    \ No newline at end of file
    ~~~

- git log <显示从近到远的提交日志>

  - git log --pretty=oneline <可以格式化查看>

- git reset <回滚>

   - ~~~
     回滚到上个版本：git reset --hard HEAD^
     回滚到上上个版本：git reset --hard HEAD^^
     ...
     ~~~

   - 

     ~~~
     回滚到指定版本：git reset --hard [对应 commit-ID]
     ~~~

### .git 隐藏文件夹

> 这个不算工作区，而是 Git 的版本库。

- 文件夹中的 stage（或者叫 index）：暂存区

### 撤销更改

- 如果已经 commit ，使用 `git checkout -- [file]`





## Java

### String

~~~java
String str = "hello";
String str1 = new String("hello");
System.out.println(str == str1);

输出：false


String str = "hello";
String str1 = new String("hello");
System.out.println(str.equals(str1));

输出：true
~~~

- == 比较的是地址，equals 比较的内容
- 字符串内容不可更改 <字符串相加改变的是堆内存地址的指向>

### StringBuffer

- 可更改
- 是一个操作类，必须通过实例化进行操作
- 常用方法
  - append()
  - insert()
  - replace()
  - indexOf()

### StringBuilder

- 可变的字符序列
- 速度比 StringBuffer 快
- StringBuffer 线程安全
- 常用方法
  - append()
  - insert()

### 重载

> 方法名称相同，参数类型和个数不同。

### 多态

1、多态的体现：

- 方法的重载和重写
- 对象的多态性

2、对象的多态性：

- 向上转型：自动完成
  - 父类	父类对象	=	子类实例
- 向下转型：强制类型转换
  - 子类	子类对象	=	（子类）父类实例

### Collection 接口

- 集合特点：性能高，易扩展，易修改
- Collection 常用子类：
  - List
  - Set
  - Queue

### List 接口

- 可存任意数据，内容可修改
- 常用子类：
  - ArrayList：异步处理方式，性能高；非线程安全
  - Vector：同步处理方式，性能低；线程安全
- 常用操作：
  - 判断集合是否为空：boolean isEmpty();
  - 查找指定对象是否存在：int indexOf(Object o)

### Set 接口

- 不重复，可排序
- 常用子类：
  - 散列存放：HashSet
  - 有序存放：TreeSet

### Iterator 接口

- 输出的标准操作：使用 Iterator 接口
- 操作原理：Iterator 是专门的迭代输出接口，对元素进行判断，若有内容，则输出

~~~java
	public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("A");
        list.add("B");
        list.add("C");
        java.util.Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
~~~

### Map 接口

- 保存方式：key —> value
- 常用子类：
  - HashMap：无序存放，key 不能重复
  - HashTable：无序存放，key 不能重复 

## File 类

- 创建文件：createNewFile();

~~~java
createNewFile() 源码：

    public boolean createNewFile() throws IOException {
        SecurityManager security = System.getSecurityManager();
        if (security != null) security.checkWrite(path);
        if (isInvalid()) {
            throw new IOException("Invalid file path");
        }
        return fs.createFileExclusively(path);
	}
~~~

- 删除：delete();

~~~java
delete() 源码：

	public boolean delete() {
        SecurityManager security = System.getSecurityManager();
        if (security != null) {
            security.checkDelete(path);
        }
        if (isInvalid()) {
            return false;
        }
        return fs.delete(this);
    }
~~~

- 删除：deleteOnExit();

~~~java
 deleteOnExit() 源码：
 
 public void deleteOnExit() {
        SecurityManager security = System.getSecurityManager();
        if (security != null) {
            security.checkDelete(path);
        }
        if (isInvalid()) {
            return;
        }
        DeleteOnExitHook.add(path);
    }
~~~

### 读取文件属性：

- getName();
- getPath();
- getAbsolutePath()
- getParent();
- length();    <读取文件大小，单位为字节>
- ......

### 遍历文件夹

~~~java
	public static void main(String[] args) {
        prinfFiles(new File("F:/Java_IDEA/hand_practise/src"), 1);
    }

    public static void prinfFiles(File file, int tab) {
        if (file.isAbsolute()) {
            File next[] = file.listFiles();
            for (int i = 0; i < next.length; i++) {
                for (int j = 0; j < tab; j++) {
                    System.out.println("---");
                }
                System.out.println(next[i].getName());
                if (next[i].isDirectory()) {
                    prinfFiles(next[i], tab +1);
                }
            }
        }
    }
~~~

- 文件输入输出流



## I/O

### 字节流与字符流

#### 字节流

- 字节流可处理所有类型数据；读取时，读到一个字节就返回一个字节
- 在 Java 中对应的类以 “Stream” 结尾

#### 字节流读取数据

#### 字符流

- 只能处理纯文本数据；例如 .txt 等；读取时，读取到一个或多个字节，先查找指定的编码表，然后将查到的字符返回。
- 在 Java 中对应的类以 “Reader” 或者 “Writer” 结尾

### 输入流与输出流



## 线程

1、在 Java 中，线程的实现有 2 种方式：

- 继承 Thread 类（）必须重写 run（）方法

  - ~~~java
    class className extends Thread {
    	run(){
    	
    	};
    }
    ~~~

- 实现 Runnable 接口

2、线程状态

- 创建状态：准备好一个多线程对象
- 就绪状态：调用 start() 方法，等待 CPU 进行调度
- 运行状态：执行 run() 方法
- 阻塞状态：暂时停止执行，可能将资源交给其他线程使用
- 阻止状态（死亡）：线程销毁

3、线程优先级

- 优先级顺序：
  - 1-MIN_PRIORITY
  - 10-MAX_PRIORITY
  - 5-NORM_PRIORITY（缺省值）



### 查询 MySQL 事务隔离级别

~~~sql
1.查看当前会话隔离级别

 select @@tx_isolation;

 

2.查看系统当前隔离级别

select @@global.tx_isolation;

 

3.设置当前会话隔离级别

set session transaction isolatin level repeatable read;

 

4.设置系统当前隔离级别

set global transaction isolation level repeatable read;

 

5.命令行，开始事务时

 set autocommit=off 或者 start transaction

 

1.read uncommitted

可以看到未提交的数据（脏读），举个例子：别人说的话你都相信了，但是可能他只是说说，并不实际做。

 

2.read committed

读取提交的数据。但是，可能多次读取的数据结果不一致（不可重复读，幻读）。用读写的观点就是：读取的行数据，可以写。



3.repeatable read(MySQL默认隔离级别)

可以重复读取，但有幻读。读写观点：读取的数据行不可写，但是可以往表中新增数据。在MySQL中，其他事务新增的数据，看不到，不会产生幻读。采用多版本并发控制（MVCC）机制解决幻读问题。

 

4.serializable

可读，不可写。像java中的锁，写数据必须等待另一个事务结束。
~~~



