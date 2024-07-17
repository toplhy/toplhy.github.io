## GC日志和垃圾收集器参数总结

+ GC日志

  例：33.125:  [GC  [DefNew:  3324/k->152K(3712K),  0.0025925 secs]  3324K->152K(11904K), 0.0031680 secs]

  例：100.667:  [Full GC  [Tenured:  0K->210K(10240K), 0.0149142  secs]  4603K->210K(19456K),  [Perm:  2999K->2999K(21248K), 0.0150007  secs]]  

  + 33.125、 100.667：表示GC发生的时间，这数字指的是从虚拟机启动以来经过的秒数。
  + GC、Full GC：表示垃圾收集的停顿类型。Full 说明这次发生了Stop The World。
  + DefNew、Tenured、Perm：表示GC发生的区域。例如，DefNew表示Serial收集器的新生代，ParNew表示ParNew收集器的新生代，PSYoungGen表示Parallel Scavenge收集器的新生代。
  + 3324K->152K(3712K)：方括号内表示“GC前该区域已使用容量->GC后该区域已使用容量(该内存区域总容量)”，方括号外表示“GC前Java堆已使用容量->GC后Java堆已使用容量(Java堆总容量)”
  + 0.0031680 secs：表示GC所占用的时间。[Times: user=0.01 sys=0.01 real=0.01 secs]中user、sys和real分别代表用户态消耗的CPU时间、内核态消耗的CPU时间和操作从开始到结束所经过的墙钟时间（Wall Clock Time）。墙钟时间包括各种各种非运算的等待耗时，例如等待磁盘I/O等，而CPU时间不包括这些耗时。


+ 垃圾收集器参数

  | 参数                           | 描述                                                         |
  | ------------------------------ | ------------------------------------------------------------ |
  | UseSerialGC                    | Client模式下默认值，打开此开关将使用Serial+Serial Old的收集器组合进行内存回收 |
  | UseParNewGC                    | 打开此开关后，使用ParNew+Serial Old的收集器组合进行内存回收  |
  | UseConcMarkSweepGC             | 打开此开关后，使用ParNew+CMS+Serial Old的收集器组合进行内存回收。Serial Old收集器作为CMS发生Concurrent Mode Failure失败后的后备收集器使用 |
  | UseParallelGC                  | Server模式下的默认值，打开此开关后，使用Parallel Scavenge+Serial Old的收集器组合进行内存回收 |
  | UseParallelOldGC               | 打开此开关后，使用Parallel Scavenge+Parallel Old的收集器组合进行内存回收 |
  | SurvivorRatio                  | 新生代中Eden区域和Survivor区域的容量比值，默认为8，代表Eden：Survivor=8：1 |
  | PretenureSizeThreshold         | 直接晋升到老年代的对象大小，大于这个参数的对象将直接分配到老年代 |
  | MaxTenuringThreshold           | 晋升代老年代的对象年龄。对象经过一次Minor GC年龄加1，超过这个设置的年龄将进入老年代 |
  | UseAdaptiveSizePolicy          | 动态调整Java堆中各个区域的大小以及进入老年代的年龄           |
  | HandlePromotionFailure         | 是否允许分配担保失败。即老年代的剩余空间不足以应付新生代的整个Eden和Survivor区的所有对象都存活的极端情况 |
  | ParallelGCThreads              | 设置并行GC时进行内存回收的线程数                             |
  | GCTimeRatio                    | GC时间占总时间的比率，默认99，允许1%的GC时间，仅在使用Parallel Scavenge收集器时生效 |
  | MaxGCPauseMills                | 设置GC的最大停顿时间，仅在使用Parallel Scavenge收集器时生效  |
  | CMSInitiatingOccupancyFraction | 设置CMS收集器在老年代空间被使用多少后触发垃圾收集。默认值为68%，仅在使用CMS收集器时生效 |
  | UseCMSCompactAtFullCollection  | 设置完成垃圾收集后是否需要进行一次内存碎片整理，仅在使用CMS收集器时生效 |
  | CMSFullGCsBeforeCompaction     | 设置CMS收集器在进行若干次垃圾收集后再启动一次内存碎片整理，仅在使用CMS收集器时生效 |
