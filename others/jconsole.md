# JConsole 工具笔记

1、介绍：JConsole 是 Java 自带的一款性能分析工具

2、如果你的电脑配置了 Java 环境，可以在控制台中键入 jconsole 打开；否则，需要在完整路径下运行可执行文件。如果 Java 安装位置为默认位置，路径为：`C:\Program Files\Java\jdk1.8.0_191\bin\jconsole.exe`

3、官方使用教程：[Using jconsole](https://docs.oracle.com/javase/1.5.0/docs/guide/management/jconsole.html)



官方教程为全英文，以下为自己阅读过程中的笔记：

`Jconsole` 是一个兼容 JMX（Java 管理扩展） 的监控工具，它使用 Java 虚拟机的广泛 JMX 工具来提供有关 Java 平台上运行的应用程序的性能和资源消耗的信息。

- [Starting jconsole](Starting jconsole)
- [Command Syntax](## Command Syntax)



## Starting jconsole

当你安装了 JDK，并且 `JDK_HOME`是一个目录时， jconsole 是位于 `JDK_HOME/bin`下的一个可执行文件，如果这个目录在你的系统路径下的话，你可以通过简单的命令来启动这个工具。否则，你必须通过完整的路径来运行这个可执行文件。

### Command Syntax

你可以使用 `jconsole` 来监控你的本地应用（运行在同一个系统）和远程应用（运行在另一个系统）。

笔记：对于开发和原型来说使用 `jconsole`来监控一个本地应用是有用的，但不建议在生产线上使用，因为`jconsole` 自身会消耗重要的系统资源。推荐在远程使用时，隔离`jconsole`应用与受监控的应用。

关于 `jconsole`的命令，请参考 [jconsole - Java Monitoring and Management Console](https://docs.oracle.com/javase/1.5.0/docs/tooldocs/share/jconsole.html).

#### *Local Monitoring*

要监视一个本地应用， 它必须以同样的用户 ID 运行在 `jconsole`中，启动`jconsole`本地监控的命令语法是： `jconsole [processID]`

当 processID 是应用的进程 ID（PID）时，要确定一个应用的 PID：

- 在 Unix 或者 Linux 系统，使用 `ps` 命令来找到 **java** 的 PID
- 在 Windows 系统中，使用任务管理器来找 **java** 或者 **javaw** 的 PID

你也可以使用 [jps](https://docs.oracle.com/javase/1.5.0/docs/tooldocs/share/jps.html) 命令行来确定 PIDs。

例如，如果你确定 *Notepad* 应用的进程 ID 为 2956，然后你可以通过 `jconsole 2956` 来启动 jconsole

`jconsole` 和 应用程序都必须通过同一用户来执行。管理和监视系统使用操作系统的文件权限。如果你没有特殊的进程 ID， jconsole 将会自动检测所有的本地 Java 应用，并且显示并显示一个对话框，您可以选择要监视的对话框（请参阅下一节）。

关于更多的信息，请看[Local JMX Monitoring and Management](https://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html#local).

#### *Remote Monitoring*

要启动 jconsole 来远程监控， 使用这个命令语法：

`jconsole [hostName:portNum]` 当 *hostName* 是系统正在运行的应用和端口号 *portNum* ，您在启动 JVM 时启用 JMX 代理时指定的。更多信息，请看[Remote JMX Monitoring and Management](https://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html#remote).

如果你没有指定主机名/端口号组合，jconsole 将显示一个链接对话框（详见下一节）使你能够输入主机名和端口号。

### Connecting to a JMX Agent

如果使用指定要连接的 JMX 代理的参数启动 `jconsole`，它将自动启动来监控指定 JVM。你可以通过选择 **Connection | New Connection** 在任何时候来连接一个不同的主机，并且输入必要的信息。

否则，如果在你不提供任何参数的情况下启动`jconsole`，你看到的第一件事就是连接的对话框。这个对话框有三个标签：

- Local（本地）
- Remote（远程）
- Advanced（高级）

#### *Local Tab* （本地选项卡）

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/localtab.jpg"/></div>

本地选项卡列出在本地系统上运行的所有 JVM，它们使用与 jconsole 相同的用户 ID 启动，以及它们的进程 ID 和类/参数信息。选择你想监控的应用，然后点击 Connection。

#### *Remote Tab*（远程选项卡

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

## The jconsole interface

jconsole 界面由 6 个选项卡组成：

- Summary tab: displays summary information on the JVM and monitored values.（“摘要”选项卡：显示有关JVM和受监视值的摘要信息）
- Memory tab: displays information on memory use.（ Memory选项卡：显示有关内存使用的信息）
- Threads tab: displays information on thread use.（线程选项卡：显示有关线程使用的信息）
- Classes tab: displays information on class loading（类选项卡：显示有关类加载的信息 ）
- MBeans tab: displays information on MBeans（MBeans选项卡：显示有关MBean的信息）
- VM tab: displays information on the JVM（VM选项卡：显示有关JVM的信息）

以下部分提供有关每个选项卡的信息。

## Viewing Summary Information

摘要选项卡显示有关线程使用情况，内存消耗和类加载的一些关键监视信息，以及有关JVM和操作系统的信息。

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/summarytab.jpg" /></div>

### Summary

- **Uptime**: how long the JVM has been running（正常运行时间： JVM 运行了多久）
- **Total compile time**: the amount of time spent in just-in-time (JIT) compilation.（总编译时间：在即时（JIT）编译中花费的时间量）
- **Process CPU time**: the total amount of CPU time consumed by the JVM（进程CPU时间：JVM消耗的CPU总时间）

### Threads

- **Live threads**: Current number of live daemon threads plus non-daemon threads
- **Peak**: Highest number of live threads since JVM started.
- **Daemon threads**: Current number of live daemon threads
- **Total started**: Total number of threads started since JVM started (including daemon, non-daemon, and terminated).

### Memory

- **Current heap size**: Number of Kbytes currently occupied by the heap.
- **Committed memory**: Total amount of memory allocated for use by the heap.
- **Maximum heap size**: Maximum number of Kbytes occupied by the heap.
- **Objects pending for finalization**: Number of objects pending for finalization.
- **Garbage collector information**: Information on GC, including the garbage collector names, number of collections performed, and total time spent performing GC.

### Classes

- **Current classes loaded**: Number of classes currently loaded into memory.
- **Total classes loaded**: Total number of classes loaded into memory since the JVM started, included those subsequently unloaded.
- **Total classes unloaded**: Number of classes unloaded from memory since the JVM started.

### Operating System

- **Total physical memory**: Amount of random-access memory (RAM) that the OS has.
- **Free physical memory**: Amount of free RAM the OS has.
- **Committed virtual memory**: Amount of virtual memory guaranteed to be available to the running process.

## Monitoring Memory Consumption

The Memory tab provides information on memory consumption and memory pools.

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/memtab.jpg" /></div>