## 字节码指令

Java虚拟机的指令是由一个字节长度的、代表这某种特定操作含义的数字（操作码，Opcode）以及跟随其后的零至多个代表此操作所需参数（操作数，Operands）而构成。

在Java虚拟机的指令集中，大多数的指令都包含了其操作所对应的数据类型信息，它们的操作码助记符中都有特殊的字符来表明专门为那种数据类型服务：i代表对int类型的数据操作，l代表long，s代表short，b代表byte，c代表char，f代表float，d代表double，a代表reference，还有一些没有明确指明操作类型，还有一些与数据类型无关。

Java虚拟机指令集所支持的数据类型：

| opcode    | byte    | short   | int       | long    | float   | double  | char    | reference |
| --------- | ------- | ------- | --------- | ------- | ------- | ------- | ------- | --------- |
| Tipush    | bipush  | sipush  |           |         |         |         |         |           |
| Tconst    |         |         | iconst    | lconst  | fconst  | dconst  |         | aconst    |
| Tload     |         |         | iload     | lload   | fload   | dload   |         | aload     |
| Tstore    |         |         | istore    | lstore  | fstore  | dstore  |         | astore    |
| Tinc      |         |         | iinc      |         |         |         |         |           |
| Taload    | baload  | saload  | iaload    | laload  | faload  | daload  | caload  | aaload    |
| Tastore   | bastore | sastore | iastore   | lastore | fastore | dastore | castore | aastore   |
| Tadd      |         |         | iadd      | ladd    | fadd    | dadd    |         |           |
| Tsub      |         |         | isub      | lsub    | fsub    | dsub    |         |           |
| Tmul      |         |         | imul      | lmul    | fmul    | dmul    |         |           |
| Tdiv      |         |         | idiv      | ldiv    | fdiv    | ddiv    |         |           |
| Trem      |         |         | irem      | lrem    | frem    | drem    |         |           |
| Tneg      |         |         | ineg      | lneg    | fneg    | dneg    |         |           |
| Tshl      |         |         | ishl      | lshl    |         |         |         |           |
| Tshr      |         |         | ishr      | lshr    |         |         |         |           |
| Tushr     |         |         | iushr     | lushr   |         |         |         |           |
| Tand      |         |         | iand      | land    |         |         |         |           |
| Tor       |         |         | ior       | lor     |         |         |         |           |
| Txor      |         |         | ixor      | lxor    |         |         |         |           |
| i2T       | i2b     | i2s     |           | i2l     | i2f     | i2d     |         |           |
| l2T       |         |         | l2i       |         | l2f     | l2d     |         |           |
| f2T       |         |         | f2i       | f2l     |         | f2d     |         |           |
| d2T       |         |         | d2i       | d2l     | d2f     |         |         |           |
| Tcmp      |         |         |           | lcmp    |         |         |         |           |
| Tcmpl     |         |         |           |         | fcmpl   | dcmpl   |         |           |
| Tcmpg     |         |         |           |         | fcmpg   | dcmpg   |         |           |
| if_TcmpOP |         |         | if_icmpOP |         |         |         |         | if_acmpOP |
| Treturn   |         |         | ireturn   | lreturn | freturn | dreturn |         | aretrun   |

+ 加载和存储指令

  加载和存储指令用于将数据在栈帧中的局部变量表和操作数栈之间来回传输，这类指令包括如下：

  + 将一个局部变量加载到操作栈：iload、iload\_\<n\>、lload、lload\_\<n\>、fload、fload\_\<n\>、dload、dload\_\<n\>、aload、aload\_\<n\>
  + 将一个数值从操作数栈存储到局部变量表：istore、istore\_\<n\>、lstore、lstore\_\<n\>、fstore、fstore\_\<n\>、dstore、dstore\_\<n\>、astore、astore\_\<n\>
  + 将一个常量加载到操作数栈：bipush、sipush、ldc、ldc_w、ldc2_w、aconst_null、iconst_ml、iconst\_\<i\>、lconst\_\<l\>、fconst\_\<f\>、dconst\_\<d\>
  + 扩充局部变量表的访问索引的指令：wide

+ 运算指令

  运算或算数指令用于对两个操作数栈上的值进行某种特定运算，并把结果重新存入到操作栈顶。大体上可以分为两种：对整型进行运算的指令与浮点型数据进行运算的指令。由于没有直接支持byte、short、char和boolean类型的算数指令，对这些类型数据的运算，使用int类型的指令代替。

  + 加法指令：iadd、ladd、fadd、dadd
  + 减法指令：isub、lsub、fsub、dsub
  + 乘法指令：imul、lmul、fmul、dmul
  + 除法指令：idiv、ldiv、fdiv、ddiv
  + 求余指令：irem、lrem、frem、drem
  + 取反指令：ineg、lneg、fneg、dneg
  + 位移指令：ishl、ishr、iushr、lshl、lshr、lushr
  + 按位或指令：ior、lor
  + 按位与指令：iand、land
  + 按位异或指令：ixor、lxor
  + 局部变量自增指令：iinc
  + 比较指令：dcmpg、dcmpl、fcmpg、fcmpl、lcmp

+ 类型转换指令

  类型转换指令可以将两种不同的数值类型进行相互转换。

  类型转换有两种：

  + 宽化类型转换，即小范围类型向大范围类型的安全转换，无需显式的转换指令
    + int到long、float、double类型
    + long到float、double类型
    + float到double类型
  + 窄化类型转换，必须显式地使用转换指令来完成，包括：i2b、i2c、i2s、l2i、f2i、f2l、d2i、d2l和d2f。窄化类型转换可能会导致转换结果产生不同的正负号、不同的数量级的情况，转换过程很可能会导致数值的精度丢失。

+ 对象创建与访问指令

  + 创建类的指令：new
  + 创建数组的指令：newarray、anewarray、multianewarray
  + 访问类字段（static字段，即类变量）和实例字段（非static字段，即实例变量）的指令：getfield、putfield、getstatic、putstatic
  + 把一个数组元素加载到操作数栈的指令：baload、caload、saload、iaload、laload、faload、daload、aaload
  + 将一个操作数栈的值存到数组元素中的指令：bastroe、castore、sastore、iastore、fastore、dastore、aastore
  + 取数组长度的指令：arraylength
  + 检查类实例类型的指令：instanceof、checkcast

+ 操作数栈管理指令

  + 将操作数栈的栈顶一个或两个元素出栈：pop、pop2
  + 复制栈顶一个或两个数值并将复制值或双份的复制值重新压入栈顶：dup、dup2、dup_x1、dup2_x1、dup_x2、dup2_x2
  + 将栈最顶端的两个数值互换：swap

+ 控制转移指令

  控制转移指令可以让Java虚拟机有条件或无条件地从指令的位置指令而不是控制转移指令的下一条指令继续执行程序。

  + 条件分支：ifeq、iflt、ifle、ifgt、ifge、ifnull、ifnonnull、if_icmpeq、if_icmpne、if_icmplt、if_icmpgt、if_icmple、if_icmpge、if_acmpeq和if_acmpne
  + 复合条件分支：tableswitch、lookupswitch
  + 无条件分支：goto、goto_w、jsr、jsr_w和ret

+ 方法调用和返回指令

  方法调用指令：

  + invokevirtual指令用于调用对象的实例方法，根据对象的实际类型进行分派
  + invokeinterface指令用于调用接口方法，他会在运行时搜索一个实现了这个接口方法的对象，找出适合的方法进行调用
  + invokespecial指令用于调用一些需要特殊处理的方法，包括实例初始化方法、私有方法和父类方法
  + invokestatic指令用于调用类方法（static方法）
  + invokedynamic指令用于在运行时动态解析出调用点限定符所引用的方法，并执行该方法，前面4条调用指令发的分派逻辑都固化在Java虚拟机内部，而invokedynamic指令的分派逻辑使用用户所设定的引导方法决定

  方法返回指令是根据返回值的类型区分的，包括ireturn（当返回值是boolean、byte、char、short和int类型时使用）、lreturn、freturn、dreturn和areturn

+ 异常处理指令

  在Java程序中显式抛出异常的操作（throw语句）都是由athrow指令来实现

  在Java虚拟机中处理异常（catch语句）不是由字节码指令来实现的，而是采用异常表来完成的

+ 同步指令

  Java虚拟机可以支持方法级的同步和方法内部一段指令序列的同步，这两种同步结构都是使用管程（Monitor）来支持的。

