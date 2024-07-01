## 可能出现的故障
+ 集群间同步导致内存溢出

+ 堆外内存导致溢出错误

  除了Java堆和永久代之外，下面这些区域还会闸弄较多的内存：

  + Direct Memory：可通过-XX:MaxDirectMemorySize调整大小，内存不足时抛出OutOfMemoryError或者OutOfMemoryError：Direct buffer memory

    垃圾收集进行时，虚拟机虽然会对Direct Memory进行回收，但是Direct Memory却不能像新生代、老年代那样，发现空间不足就通知收集器进行垃圾回收，它只能等待老年代满了后Full GC，然后顺便帮它清理掉内存中的废弃对象

  + 线程堆栈：可通过-Xss调整大小，内存不足时抛出StackOverflowError或者OutOfMemoryError：unable  to create new native thread

  + Socket缓存区：每个Socket连接都Receive和Send两个缓存区，分别占37KB和25KB内存，连接多的话这块内存占用也比较客观。如果无法分配，则可能会抛出IOException：Too many open files异常

  + JNI代码：如果代码中使用JNI调用本地库，那本地库使用的内存也不再堆中

  + 虚拟机和GC：虚拟机、GC的代码执行也要消耗一定的内存

+ 外部命令导致系统缓慢

  例如系统中通过Java的Runtime.getRuntime().exec()命令调用外部命令来实现某个功能。这种调用方式可以达到目的，但是它在Java中虚拟机中是非常消耗资源的操作，即使外部命令能够很快执行完毕，频繁调用时创建进程的开销也非常客观。Java虚拟机执行这个命令的过程是：首先克隆一个和当前虚拟机拥有一样环境变量的进程，再用这个进程去执行外部命令，最后在退出这个进程，如果频繁执行这个操作，系统的消耗会很大们不仅是CPU，内存负担也很重。

+ 服务器JVM进程崩溃

  例如在系统中使用异步方式调用web服务，但由于两边的服务速度完全不对等，时间越长就累积了越多的web服务没有调用完成，导致在等待的线程和Socket连接越来越多，最终超过虚拟机的承受能力后使得虚拟机进程崩溃。可以将异步调用改为生产者/消费者模式的消息队列实现。

+ 不恰当数据结构导致内存占用过大

+ 由windows虚拟内存导致的长时间停顿

看完书中的很多案例，但对故障处理和性能调优还是很陌生，没有实际的处理经验。
