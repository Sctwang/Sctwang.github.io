# JConsole

1、介绍：JConsole 是 Java 自带的一款性能分析工具

2、如果你的电脑配置了 Java 环境，可以在控制台中键入 jconsole 打开；否则，需要在完整路径下运行可执行文件。如果 Java 安装位置为默认位置，路径为：`C:\Program Files\Java\jdk1.8.0_191\bin\jconsole.exe`

3、官方使用教程：[Using jconsole](https://docs.oracle.com/javase/1.5.0/docs/guide/management/jconsole.html)

------


官方文档为全英文，以下为自己阅读过程中的笔记（部分采用谷歌翻译）：

`Jconsole` 是一个兼容 JMX（Java 管理扩展） 的监控工具，它使用 Java 虚拟机的广泛 JMX 工具来提供有关 Java 平台上运行的应用程序的性能和资源消耗的信息。

- [Starting jconsole](Starting jconsole)
- [Command Syntax](## Command Syntax)



## Starting jconsole

当你安装了 JDK，并且 `JDK_HOME`是一个目录时， jconsole 是位于 `JDK_HOME/bin`下的一个可执行文件，如果这个目录在你的系统路径下的话，你可以通过简单的命令来启动这个工具。否则，你必须通过完整的路径来运行这个可执行文件。

### Command Syntax（命令语法）

你可以使用 `jconsole` 来监控你的本地应用（运行在同一个系统）和远程应用（运行在另一个系统）。

笔记：对于开发和原型来说使用 `jconsole`来监控一个本地应用是有用的，但不建议在生产线上使用，因为`jconsole` 自身会消耗重要的系统资源。推荐在远程使用时，隔离`jconsole`应用与受监控的应用。

关于 `jconsole`的命令，请参考 [jconsole - Java Monitoring and Management Console](https://docs.oracle.com/javase/1.5.0/docs/tooldocs/share/jconsole.html).

#### *Local Monitoring*（本地监控）

要监视一个本地应用， 它必须以同样的用户 ID 运行在 `jconsole`中，启动`jconsole`本地监控的命令语法是： `jconsole [processID]`

当 processID 是应用的进程 ID（PID）时，要确定一个应用的 PID：

- 在 Unix 或者 Linux 系统，使用 `ps` 命令来找到 **java** 的 PID
- 在 Windows 系统中，使用任务管理器来找 **java** 或者 **javaw** 的 PID

你也可以使用 [jps](https://docs.oracle.com/javase/1.5.0/docs/tooldocs/share/jps.html) 命令行来确定 PIDs。

例如，如果你确定 *Notepad* 应用的进程 ID 为 2956，然后你可以通过 `jconsole 2956` 来启动 jconsole

`jconsole` 和 应用程序都必须通过同一用户来执行。管理和监视系统使用操作系统的文件权限。如果你没有特殊的进程 ID， jconsole 将会自动检测所有的本地 Java 应用，并且显示并显示一个对话框，您可以选择要监视的对话框（请参阅下一节）。

关于更多的信息，请看[Local JMX Monitoring and Management](https://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html#local).

#### *Remote Monitoring*（远程监控）

要启动 jconsole 来远程监控， 使用这个命令语法：

`jconsole [hostName:portNum]` 当 *hostName* 是系统正在运行的应用和端口号 *portNum* ，您在启动 JVM 时启用 JMX 代理时指定的。更多信息，请看[Remote JMX Monitoring and Management](https://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html#remote).

如果你没有指定主机名/端口号组合，jconsole 将显示一个链接对话框（详见下一节）使你能够输入主机名和端口号。

### Connecting to a JMX Agent（连接 JMX 代理）

如果使用指定要连接的 JMX 代理的参数启动 `jconsole`，它将自动启动来监控指定 JVM。你可以通过选择 **Connection | New Connection** 在任何时候来连接一个不同的主机，并且输入必要的信息。

否则，如果在你不提供任何参数的情况下启动`jconsole`，你看到的第一件事就是连接的对话框。这个对话框有三个标签：

- Local（本地）
- Remote（远程）
- Advanced（高级）

#### *Local Tab* （本地选项卡）

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/localtab.jpg"/></div>
本地选项卡列出在本地系统上运行的所有 JVM，它们使用与 jconsole 相同的用户 ID 启动，以及它们的进程 ID 和类/参数信息。选择你想监控的应用，然后点击 Connection。

#### *Remote Tab*（远程选项卡）

<div align=center><img /src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/remotetab.jpg"/></div>
要监控一个远程 JVM，输入：

- 主机名字：运行 JVM 的机器名称。
- 端口号：启动 JVM 时指定的 JMX 代理端口号。
- 用户名称和密码：要使用的用户名和密码（仅在通过需要密码身份验证的 JMX 代理监视 JVM 时才需要）。

更多信息详见设置端口号的 JMX 代理，请看 [Enabling the JMX Management Agent](https://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html#jmxagent). 更多信息在用户名称和密码，详见 [Using Password and Access Files](https://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html#PasswordAccessFiles).

要运行 jconsole 监控 JVM，简单的电机 Connect，使用主机 localhost 和端口 0.

#### *Advanced Tab*（高级选项卡）

<div align="center"><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/Advanced Tab.jpg"/></div>
高级选项卡使你能够通过指定 JMX URL 连接 JMX 代理（MBean 服务），还有用户名称和密码。

[javax.management.remote.JMXServiceURL](https://docs.oracle.com/javase/1.5.0/docs/api/javax/management/remote/JMXServiceURL.html) 的 API 文档中描述了 JMX URL 的语法。

笔记：如果 JMX 代理程序在未包含在 Java 平台中的连接器中使用，则需要在运行 jconsole 时将连接器类添加到类路径中，如下所示：

`jconsole -J-Djava.class.path=JAVA_HOME/lib/jconsole.jar:JAVA_HOME/lib/tools.jar:connector-path`

## The jconsole interface（jconsole 接口）

jconsole 界面由 6 个选项卡组成：

- 摘要选项卡：显示有关JVM和受监视值的摘要信息
- 内存选项卡：显示有关内存使用的信息
- 线程选项卡：显示有关线程使用的信息
- MBeans选项卡：显示有关MBean的信息
- VM选项卡：显示有关JVM的信息

以下部分提供有关每个选项卡的信息。

## Viewing Summary Information（查看摘要信息）

摘要选项卡显示有关线程使用情况，内存消耗和类加载的一些关键监视信息，以及有关JVM和操作系统的信息。

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/summarytab.jpg" /></div>
### Summary（摘要）

- **正常运行时间**： JVM 运行了多久
- **总编译时间**：在即时（JIT）编译中花费的时间量
- **进程 CPU 时间**：JVM 消耗的 CPU 总时间

### Threads（线程）

- **实时线程**：当前活动守护程序线程数和非守护程序线程数 
- **峰值**：自 JVM 启动以来最多的活动线程数。
- **守护程序线程**：当前活动守护程序线程数 。
- **总计开始的**: 自 JVM 启动以来启动的线程总数（包括守护进程，非守护进程）。

### Memory（内存）

- **当前堆大小**：堆当前占用的 Kbytes 数。
- **提交的内存**：分配给堆使用的内存总量。 
- **最大堆大小**：堆占用的最大 Kbytes 数。 
- **待完成的对象**：待完成的对象数。 
- **垃圾收集器信息**：有关 GC 的信息，包括垃圾收集器名称，执行的收集数以及执行 GC 所花费的总时间。

### Classes（内存）

- **当前加载的类**：当前加载到内存中的类的数量。 
- **加载的类总数**：自 JVM 启动以来加载到内存中的类的总数，包括随后卸载的类。 
- **卸载的总类数**：自 JVM 启动以来从内存中卸载的类数。

### Operating System（操作系统）

- **物理内存总量**：操作系统具有的随机存取内存（RAM）的数量。
- **自由物理内存**：操作系统具有的可用 RAM 量。 
- **提交的虚拟内存**：保证可供正在运行的进程使用的虚拟内存量。

## Monitoring Memory Consumption（监控内存消耗）

内存选项卡提供有关内存消耗和内存池的信息。

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/memtab.jpg" /></div>
该图表显示了 JVM 的内存使用与时间，堆和非堆内存以及特定内存池的关系。可用的内存池取决于所使用的 JVM。对于 HotSpot JVM，池是：

- Eden Space (heap): pool from which memory is initially allocated for most objects.
- Survivor Space (heap): pool containing objects that have survived GC of eden space.
- Tenured Generation (heap): pool containing objects that have existed for some time in the survivor space.
- Permanent Generation (non-heap): holds all the reflective data of the virtual machine itself, such as class and method objects. With JVMs that use [class data sharing](https://docs.oracle.com/javase/1.5.0/docs/guide/vm/class-data-sharing.html), this generation is divided into read-only and read-write areas.
- Code Cache (non-heap): HotSpot JVM also includes a "code cache" containing memory used for compilation and storage of native code.

关于内存池更多的信息，请看：[Garbage Collection.](https://docs.oracle.com/javase/1.5.0/docs/guide/management/jconsole.html#gc)

**详细**信息区域显示了几个当前内存指标：

- **Used**: the amount of memory currently used. Memory used includes the memory occupied by all objects including both reachable and unreachable objects.
- **Committed**: the amount of memory guaranteed to be available for use by the JVM. The amount of committed memory may change over time. The Java virtual machine may release memory to the system and committed could be less than the amount of memory initially allocated at startup. Committed will always be greater than or equal to used.
- **Max**: the maximum amount of memory that can be used for memory management. Its value may change or be undefined. A memory allocation may fail if the JVM attempts to increase the used memory to be greater than committed memory, even if the amount used is less than or equal to max (for example, when the system is low on virtual memory).

右下方的条形图显示了堆和非堆内存中内存池消耗的内存。当使用的内存超过内存使用量阈值时，栏将变为红色。您可以通过 MemoryMXBean 的属性设置内存使用量阈值。

### Heap and Non-heap Memory（堆和非堆内存）

JVM 管理两种内存：堆和非堆内存，两者都在启动时创建。

*堆内存*是运行时数据区，JVM 从中为所有类实例和数组分配内存。堆可以是固定的或可变的大小。垃圾收集器是一个自动内存管理系统，可回收对象的堆内存。 

*非堆内存*包括在 JVM 的内部处理或优化所需的所有线程和内存之间共享的方法区域。它存储每类结构，例如运行时常量池，字段和方法数据，以及方法和构造函数的代码。方法区域在逻辑上是堆的一部分，但是根据实现，JVM 可能不会垃圾收集或压缩它。与堆一样，方法区域可以是固定的或可变的大小。方法区域的内存不需要是连续的。 

除了方法区域之外，JVM 实现可能还需要用于内部处理或优化的内存，该内存也属于非堆内存。例如，JIT 编译器需要用于存储从 JVM 代码转换的本机机器代码的内存以获得高性能。

### Memory Pools and Memory Managers（内存池和内存管理器）

内存池和内存管理器是 JVM 内存系统的关键方面。

*内存池*表示 JVM 管理的内存区域。 JVM 至少有一个内存池，它可以在执行期间创建或删除内存池。内存池可以属于堆内存或非堆内存。

*内存管理器*管理一个或多个内存池。垃圾收集器是一种内存管理器，负责回收无法访问的对象使用的内存。 JVM 可能有一个或多个内存管理器。它可以在执行期间添加或删除内存管理器。内存池可以由多个内存管理器管理。

### Garbage Collection（垃圾收集）

Garbage collection (GC) is how the JVM frees memory occupied by objects that are no longer referenced. It is common to think of objects that have active references as being "alive" and un-referenced (or unreachable) objects as "dead." Garbage collection is the process of releasing memory used by the dead objects. The algorithms and parameters used by GC can have dramatic effects on performance.

The HotSpot VM garbage collector uses *generational garbage collection*. Generational GC takes advantage of the observation that, in practice, most programs create:

- many objects that have short lives (for example, iterators and local variables).
- some objects that have very long lifetimes (for example, high level persistent objects)

So, generational GC divides memory into several *generations*, and assigns each a memory pool. When a generation uses up its allotted memory, the VM performs a partial garbage collection (also called a *minor collection*) on that memory pool to reclaim memory used by dead objects. This partial GC is usually much faster than a full GC.

The HotSpot VM defines two generations: the **young generation** (sometimes called the "nursery") and the **old generation**. The young generation consists of an "eden space" and two "survivor spaces." The VM initially assigns all objects to the eden space, and most objects die there. When it performs a minor GC, the VM moves any remaining objects from the eden space to one of the survivor spaces. The VM moves objects that live long enough in the survivor spaces to the "tenured" space in the old generation. When the tenured generation fills up, there is a full GC that is often much slower because it involves all live objects. The permanent generation holds all the reflective data of the virtual machine itself, such as class and method objects.

The default arrangement of generations looks something like this:

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/generations.gif"/></div>
As explained in the following documents, if the garbage collector has become a bottleneck, you can improve performance by customizing the generation sizes. Using jconsole, explore the sensitivity of your performance metric to the garbage collector parameters. For more information, see:

- [Tuning Garbage collection with the 5.0 HotSpot VM](http://java.sun.com/docs/hotspot/gc/index.html)

## Monitoring Thread Use(监视线程使用)

The Threads tab provides information on thread use.

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/threadtab.jpg"/></div>
The graph plots the number of classes loaded versus time:

- Red line is the total number of classes loaded (including those subsequently unloaded).
- Blue line is the current number of classes loaded.

The Details section at the bottom of the tab displays the total number of classes loaded since the JVM started, the number currently loaded and the number unloaded.

## Monitoring and Managing MBeans(监控和管理 MBeans)

The MBean tab displays information on all the MBeans registered with the platform MBean server.

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190909142055.png"/></div>
The tree on the left shows all the MBeans, organized according to their objectNames. When you select an MBean in the tree, its attributes, operations, notifications and other information is displayed on the right.

You can set the value of attributes, if they are writeable (the value will be displayed in blue).  You can also invoke operations displayed in the Operations tab.

### Displaying a Chart(显示图表)

You can display a chart of an attribute's value versus time by double-clicking on the attribute value.  For example, if you click on the value of the CollectionTime property of java.lang.GarbageCollector.Copy MBean, you will see a chart that looks something like this:

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190909142141.png"/></div>
## Viewing VM Information(查看 VM 信息)

The VM tab provides information on the JVM.

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190909142226.png"/></div>
The information includes:

- **Uptime**: Total amount of time since the JVM was started.
- **Process CPU Time**: Total amount of CPU time that the JVM has consumed since it was started.
- **Total Compile Time**: Total accumulated time spent in just-in-time (JIT) compilation. The JVM implementation determines when JIT compilation occurs. The Hotspot VM uses *adaptive compilation*, in which the VM launches an application using a standard interpreter, but then analyzes the code as it runs to detect performance bottlenecks, or "hot spots".