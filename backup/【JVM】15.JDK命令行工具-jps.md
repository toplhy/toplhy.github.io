## JDK命令行工具-jps

jps：虚拟机进程状况工具

jps可以列出正在运行的虚拟机进程，并显示虚拟机执行主类（Main Class，main函数所在的类）名称以及这些进程的本地虚拟机唯一ID（Local Virtual Machine Identifier，LVMID）。

命令格式： jps  \[ options \] \[ hostid \]

jps可以通过RMI协议查询开启了RMI服务的远程虚拟机进程状态，hostid为RMI注册表中注册的主机名。

jps工具主要选项：

| 选项 | 作用                                               |
| ---- | -------------------------------------------------- |
| -q   | 只输出LVMID，省略主类的名称                        |
| -m   | 输出虚拟机进程启动时传递给main()函数的参数         |
| -l   | 输出主类的全名，如果进程执行的是Jar包，输出Jar路径 |
| -v   | 输出虚拟机进程启动时JVM参数                        |

