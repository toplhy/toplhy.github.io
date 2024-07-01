## JDK可视化工具-JConsole

JConsole是一种基于JMX的可视化监测、管理工具。

通过JAVA_HOME/bin下的jconsole.exe启动JConsole，将自动搜索出本机运行的所有虚拟机进程。

![jvm_21_1](/images/jvm/jvm_21_1.png)	

连接进程进入JConsole的主界面，共包括“概览”、“内存”、“线程”、“类”、“VM概要”、“MBean”6个标签页。

“概览”页签显示的是整个虚拟机的主要运行数据概览，其中包括“堆内存使用量”、“线程”、“类”、“CPU占用率”4种信息的曲线图。

![jvm_21_2](/images/jvm/jvm_21_2.png)

“内存”页签相当于可视化的jstat命令，用于监视受收集器管理的虚拟机内存的变化趋势。

![jvm_21_3](/images/jvm/jvm_21_3.png)

"线程"页签就相当于可视化的jstack命令，遇到线程停顿时可以使用这个页签进行监控分析。

![jvm_21_4](/images/jvm/jvm_21_4.png)

"类"页签显示的是JVM加载类的信息。

![jvm_21_5](/images/jvm/jvm_21_5.png)
