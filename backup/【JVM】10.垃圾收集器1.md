## 垃圾收集器1

### Serial收集器

   Serial收集器是最基本、发展历史最悠久的收集器。它是一个单线程的收集器，使用复制算法。它在进行垃圾收集时，必须暂停其他所有的工作线程，直到它收集结束。

   运行示意图如下：

   ![jvm_10_1](/images/jvm/jvm_10_1.png)
   
### ParNew收集器

   ParNew收集器是Serial收集器的多线程版本。

   运行示意图如下:

   ![jvm_10_2](/images/jvm/jvm_10_2.png)

   ParNew是除Serial外能与CMS收集器配合工作的收集器。

   它默认开启线程数与CPU的数量相同，可以使用-XX:ParallelGCThreads参数来限制垃圾收集的线程数。

   Serial收集器在单CPU的环境比ParNew收集器效率更高。

### Parallel Scavenge收集器

   Parallel Scavenge收集器和ParNew收集器类似，但它更关注的是吞吐量，适合在后台运算而不需要太多与用户交互的任务。

   吞吐量是CPU用于运行用户代码的时间与CPU总消耗时间的比值，即吞吐量=运行用户代码时间/(运行用户代码时间+垃圾收集时间)。

   Parallel Scavenge收集器提供了两个参数用于精确控制吞吐量，分别是最大垃圾收集停顿时间的-XX:MaxGCPauseMillis以及直接设置吞吐量大小的-XX:GCTimeRatio参数。

   + MaxGCPauseMillis参数允许一个大于0的毫秒数，收集器尽可能保证内存回收花费的时间不超过设定值（把值设小并不一定能使垃圾收集的速度变快，通过牺牲吞吐量和新生代空间来减少停顿时间）。
   + GCTimeRatio参数的值应当是一个大于0且小于100的整数。GCTime=1/(1+GCTimeRatio)。

   Parallel Scavenge收集器和ParNew收集器的另一个重要区别就是自适应调节策略，虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最合适的停顿时间或最大的吞吐量。可以通过-XX:UseAdaptiveSizePolicy参数打开该策略。

### Serial Old收集器

  Serial Old收集器是Serial收集器的老年代版本，也是一个单线程收集器，使用标记-整理算法。

  运行示意图如下:

  ![jvm_10_3](/images/jvm/jvm_10_3.png)

### Parallel Old收集器

  Parallel Old收集器是Parallel Scavenge收集器的老年代版本，使用多线程和标记-整理算法。在注重吞吐量以及CPU资源敏感的场合，可以优先考虑Parallel Scavenge加Parallel Old收集器。

  运行示意图如下:

  ![jvm_10_4](/images/jvm/jvm_10_4.png)
