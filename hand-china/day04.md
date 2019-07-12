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

~~~
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













