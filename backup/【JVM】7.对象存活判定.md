## 对象存活判定

![jvm_7](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/jvm/jvm_7.png)

+ 对象没有覆盖finalize()方法或者finalize()方法已被虚拟机调用过，虚拟机将认为这两种情况为“没有必要执行”。
+ 任何一个对象的finalize()方法只会被系统自动调用一次。
+ finalize()方法运行代价高，不确定性大，无法保证各个对象的调用顺序。
