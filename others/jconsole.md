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

## Command Syntax

你可以使用 `jconsole` 来监控你的本地应用（运行在同一个系统）和远程应用（运行在另一个系统）。

笔记：对于开发和原型来说使用 `jconsole`来监控一个本地应用是有用的，但不建议在生产线上使用，因为`jconsole` 自身会消耗重要的系统资源。推荐在远程使用时，隔离`jconsole`应用与受监控的应用。

关于 `jconsole`的命令，请参考 [jconsole - Java Monitoring and Management Console](https://docs.oracle.com/javase/1.5.0/docs/tooldocs/share/jconsole.html).

#### *Local Monitoring*

