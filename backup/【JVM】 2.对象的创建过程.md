## 对象的创建过程

![jvm_2](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/jvm/jvm_2.png)


+ 为对象分配内存：指针碰撞、空闲列表。

  根据使用的垃圾收集器判定java堆是否是规整的，规整时使用指针碰撞，不规整则使用空闲列表。

+ 解决并发情况下为对象分配内存可能会出现的线程安全问题：同步机制、本地线程分配缓冲（TLAB）。

+ 内存分配完成后，虚拟机把分配到的内存空间设置为零值，保证了对象的实例在java代码中可以不赋初始值就可以直接使用。
