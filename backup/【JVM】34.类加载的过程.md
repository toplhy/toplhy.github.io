## 类加载的过程

### 加载

  在加载阶段，虚拟机要完成一下3件事情：

  1. 通过一个类的全限定名来获取定义此类的二进制字节流

  2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构

  3. 在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口

### 验证

  验证是连接阶段的第一步，目的是为了确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。

  + 文件格式验证

    第一阶段验证字节流是否符合Class文件格式的规范。这一阶段的验证可能包括下面这些验证点：

    + 是否以魔数0xCAFEBABE开头
    + 主次版本号是否在当前虚拟机的处理范围之内
    + 常量池中的长两种是否有不被支持的常量类型
    + 指向常量的各种索引值中是否有指向不存在的常量或不符合类型的常量
    + CONSTANT_Utf8_info型的常量中是否有不符合UTF8编码的数据
    + Class文件中各个部分及文件本身是否有被删除的或附加的其他信息
    + ......

  + 元数据验证

    第二阶段是对字节码描述的信息进行语义分析，以保证其描述的信息符合Java语言规范的要求，这个阶段可能包括的验证点如下：

    + 这个类是否有父类（除了java.lang.Object外，所有的类都应当有父类）
    + 这个类的父类是否继承了不允许继承的类（被final修饰的类）
    + 如果这个类不是抽象类，是否实现了其父类或接口中要求实现的所有方法
    + 类中的字段、方法是否与父类产生矛盾
    + ......

  + 字节码验证

    第三阶段验证是最复杂的一部分，目的是通过数据流和控制流分析，确定语义是合法的、符合逻辑的。在第二阶段堆元数据信息中的数据类型做完校验后，这个阶段将对类的方法体进行校验分析，保证被校验类的方法在运行时不会做出危害虚拟机安全的事件。

  + 符号引用验证

    最后一个阶段的校验发生在虚拟机将符号引用转化为直接引用的时候，这个转化动作将在连接的第三阶段-解析阶段发生。符号引用验证可以看做是堆类自身以外的信息进行匹配性校验，通常需要交验下列内容：

    + 符号引用中通过字符串描述的全限定名是否能找到对应的类
    + 在指定类中是否存在符合方法的字段描述符以及简单名称所描述的方法和字段
    + 符号引用中的类、字段】方法的访问性是否可以被当前类访问
    + ......

    符号引用验证的目的是确保解析动作能正常执行，如果无法通过符号引用验证，将会抛出一个java.lang.IncompatibleClassChangeError异常的子类，如java.lang.IllegalAccessError、java.lang.NoSuchFieldError、java.lang.NoSuchMethodError等。

### 准备

  准备阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些变量所使用的内存都将在方法去中进行分配（首先，这时候进行内存分配的仅包括类变量，而不包括实例变量，实例变量将会在对象实例化时随对象一起被分配在Java堆中。其次，这里说的初始值“通常情况”是数据类型的零值）。

  基本数据类型的零值如下：

  | 数据类型  | 零值     |
  | --------- | -------- |
  | int       | 0        |
  | long      | 0L       |
  | short     | (short)0 |
  | char      | '\\u000' |
  | byte      | (byte)0  |
  | boolean   | false    |
  | float     | 0.0f     |
  | double    | 0.0d     |
  | reference | null     |

  如果类字段的字段属性表中存在ConstantValue属性，那么在准备阶段变狼就会被初始化为ConstantValue属性所指定的值。

### 解析

  解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程。

  符号引用和直接引用之间的关联：

  + 符号引用：符号引用以一组符号来描述所引用的目标，符号可以使任何形式的字面量，只要使用时能无歧义的定位到目标即可。符号引用与虚拟机实现的内存布局无关，引用的目标不一定已经加载到内存中。
  + 直接引用：直接引用可以使直接指向目标的指针、相对偏移量或是一个能间接定位到目标的句柄。如果有了直接引用，那引用的目标必定已经在内存中存在。

  解析动作主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符7类符号意义进行，分别对应常量池CONSTANT_Class_info、CONSANT_Fieldref_info、CONSTANT_Methodref_info、CONSTANT_InterfaceMethodref_info、CONSTANT_MethodType_info、CONSTANT_MethodHandle_info和CONSTANT_InvokeDynamic_info7种类型常量。

  1. 类或接口的解析
  2. 字段解析
  3. 类方法解析
  4. 接口方法解析

### 初始化

  类初始化阶段是类加载过程的最后一步，到了初始化阶段才开始执行类中定义的Java'程序代码。

  \<clinit>()方法执行过程中一些可能会影响程序运行行为的特点和细节：

  + \<clinit>()方法是由编译器自动收集类中的所有类变量的赋值动作和静态语句块（static{}块）中的语句合并产生的，编译器收集的顺序是由语句在源文件中出现的顺序所决定的，静态语句块只能访问到定义在静态语句块之前的变量，定义在它之后的变量，在前面的静态语句块中可以赋值，但是不能访问。
  + \<clinit>()方法与类的构造韩式不同，它不需要显式的调用父类构造器，虚拟机会保证在子类的\<clinit>()方法执行前，父类的\<clinit>()方法已经执行完毕。
  + 由于父类的\<clinit>()方法先执行，也就意味着父类中定义的静态语句块要优先于子类的变量赋值操作。
  + \<clinit>()方法对于类或接口来说并不是必需的，如果一个类中没有静态语句块，也没有对变量的赋值操作，那么编译器可以不为这个类生成\<clinit>()方法。
  + 接口中不能使用静态语句块，但仍然有变量初始化的赋值操作，因此接口与类一样都会生成\<clinit>()方法。但接口与类不同的是，执行接口的\<clinit>()方法不需要先执行父接口的\<clinit>()方法。只有当父接口中定义的变量使用时，父接口才会初始化。另外，接口的实现类在初始化时也一样不会执行接口的\<clinit>()方法。
  + 虚拟机会保证一个类的\<clinit>()方法在多线程环境中被正确的加锁、同步，如果多个线程同时区初始化一个类，那么只有一个线程去执行这个类的\<clinit>()方法，其他线程都需要阻塞等待，知道活动线程执行\<clinit>()方法完毕。
