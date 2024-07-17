## JDK命令行工具-jstack

jstack命令用来生成虚拟机当前时刻的线程快照。线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的主要目的是定位线程穿线长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等都是导致线程长时间停顿的常见原因。


命令格式： jstack  \[ options \] \<pid\>

jstack工具主要选项：

| 选项 | 作用                                         |
| ---- | -------------------------------------------- |
| -F   | 当正常输出的请求不被相应时，强制输出线程堆栈 |
| -l   | 除堆栈外，显示关于锁的信息                   |
| -m   | 如果调用到本地方法的话，可以显示C/C++的堆栈  |

可以使用java.lang.Thread类的getAllStackTraces()方法获取虚拟机中所有线程的StackTraceElement对象。

```
for(mapEntry<Thread, StackTraceElement[]> stackTrace: Thread.getAllStackTraces().entrySet()){
    Thread thread = (Thread)stackTrace.getKey();
    StackTraceElement[] stack = (StackTraceElement[])stackTrace.getValue();
    if(thread.equals(Thread.currentThread())){
        continue;
    }
    out.print("\n线程：" + thread.getName() + "\n");
    for(StackTraceElement element: stack) {
        out.print("\t" + element + "\n");
    }
}
```
