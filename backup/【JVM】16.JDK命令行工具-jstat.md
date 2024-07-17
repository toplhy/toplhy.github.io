##  JDK命令行工具-jstat

jstat：虚拟机统计信息监视工具

jstat用于见识虚拟机各种运行状态信息的命令行工具。它可以显示本地或远程虚拟机进程中的类加载、内存、垃圾收集、JIT编译等运行数据。

jstat命令格式：jstat -\<option\>  \[-t\] \[-h\<lines\>\] \<vmid\> \[\<interval\> \[\<count\>\]\]

 option代表着用户希望查询的虚拟机信息，主要分为3类：类加载、垃圾收集、运行期编译状况，具体有：

| 选项              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| -class            | 监视类装载、卸载数量、总空间及装载所耗费时间                 |
| -gc               | 监视Java堆状况，包括Eden区、两个Survivor区、老年代、永久代等的容量、已用空间、GC时间合计等信息 |
| -gccapacity       | 监视内容与-gc基本相同，但输出主要关注Java堆各个区域使用到的最大、最小空间 |
| -gcutil           | 监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比 |
| -gccause          | 与-gcutil功能一样，但是会额外输出导致上一次GC产生的原因      |
| -gcnew            | 监视新生代的GC状况                                           |
| -gcnewcapacity    | 监视内容与-gcnew基本相同，输出主要关注使用到的最大、最小空间 |
| -gcold            | 监视老年代的GC状况                                           |
| -gcoldcapacity    | 监视内容与-gcold基本相同，输出主要关注使用到的最大、最小空间 |
| -gcpermcapacity   | 输出永久代使用到的最大、最小空间                             |
| -compiler         | 输出JIT编译器编译过的方法、耗时等信息                        |
| -printcompilation | 输出已经被JIT编译的方法                                      |

vmid：指虚拟机进程ID，

-t：可以在打印的列加上Timestamp列，用于显示系统运行的时间

-h：可以在周期性数据数据的时候，可以在指定输出多少行以后输出一次表头

interval：执行每次的间隔时间，单位为毫秒

count：用于指定输出多少次记录

示例：

1. ```jstat -class <vmid>``` 

   ![jvm_16_1](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/jvm/jvm_16_1.png)

   - Loaded : 已经装载的类的数量
   - Bytes : 装载类所占用的字节数
   - Unloaded：已经卸载类的数量
   - Bytes：卸载类的字节数
   - Time：装载和卸载类所花费的时间

2. ```jstat -compiler <vmid> ```

   ![jvm_16_2](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/jvm/jvm_16_2.png)

   - Compiled：编译任务执行数量
   - Failed：编译任务执行失败数量
   - Invalid ：编译任务执行失效数量
   - Time ：编译任务消耗时间
   - FailedType：最后一个编译失败任务的类型
   - FailedMethod：最后一个编译失败任务所在的类及方法

3. ```jstat -gc <vmid>```

   ![jvm_16_3](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/jvm/jvm_16_3.png)

   - S0C：年轻代中第一个survivor（幸存区）的容量 （字节）

   - S1C：年轻代中第二个survivor（幸存区）的容量 (字节)

   - S0U：年轻代中第一个survivor（幸存区）目前已使用空间 (字节)

   - S1U：年轻代中第二个survivor（幸存区）目前已使用空间 (字节)

   - EC：年轻代中Eden（伊甸园）的容量 (字节)

   - EU：年轻代中Eden（伊甸园）目前已使用空间 (字节)

   - OC：Old代的容量 (字节)

   - OU：Old代目前已使用空间 (字节)

   - MC：metaspace(元空间)的容量 (字节)

   - MU：metaspace(元空间)目前已使用空间 (字节)

   - YGC：从应用程序启动到采样时年轻代中gc次数

   - YGCT：从应用程序启动到采样时年轻代中gc所用时间(s)

   - FGC：从应用程序启动到采样时old代(全gc)gc次数

   - FGCT：从应用程序启动到采样时old代(全gc)gc所用时间(s)

   - GCT：从应用程序启动到采样时gc用的总时间(s)

4. ```jstat -printcompilation <vmid>```

   ![jvm_16_4](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/jvm/jvm_16_4.png)

   - Compiled ：编译任务的数目
   - Size ：方法生成的字节码的大小
   - Type：编译类型
   - Method：类名和方法名用来标识编译的方法。类名使用/做为一个命名空间分隔符。方法名是给定类中的方法。上述格式是由-XX:+PrintComplation选项进行设置的
