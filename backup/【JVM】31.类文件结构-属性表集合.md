## 类文件结构-属性表集合

在Class文件、字段表】方法表都可以携带自己的属性表集合，以用于描述某些场景专有的信息。

下表是Java虚拟机规范预定义的属性：

| 属性名称                             | 使用位置           | 含义                                                         |
| ------------------------------------ | ------------------ | ------------------------------------------------------------ |
| Code                                 | 方法表             | Java代码编译成的字节码文件                                   |
| ConstantValue                        | 字段表             | final关键字定义的常量值                                      |
| Deprecated                           | 类、方法表、字段表 | 被声明为deprecated的方法和字段                               |
| Exceptions                           | 方法表             | 方法抛出的异常                                               |
| EnclosingMethod                      | 类文件             | 仅当一个类为局部类或者匿名类是才能拥有这个属性，这个属性用于标识这个类所在的外围方法 |
| InnerClasses                         | 类文件             | 内部类列表                                                   |
| LineNumberTable                      | Code属性           | Java源码的行号和字节码指令的对应关系                         |
| LocalVariableTable                   | Code属性           | 方法的局部变量描述                                           |
| StrackMapTable                       | Code属性           | JDK1.6中新增的属性，供新的类型检查验证器检查和处理目标方法的局部变量和操作数栈所需要的类型是否匹配 |
| Signature                            | 类、方法表、字段表 | JDK1.5中新增的属性，这个属性用于支持泛型情况下的方法签名     |
| SourceFile                           | 类文件             | 记录源文件名称                                               |
| SourceDebugExtension                 | 类文件             | JDK1.6中新增的属性，SourceDebugExtension属性用于存储额外的调试信息。 |
| Synthetic                            | 类、方法表、字段表 | 标识方法或字段为编译器自动生成的                             |
| LocalVariableTypeTable               | 类                 | JDK1.5中新增的属性，它使用特征签名代表描述符，是为了引入泛型语法之后能描述泛型参数化类型而添加 |
| RuntimeVisibleAnnotations            | 类、方法表、字段表 | JDK1.5中新增的属性，为动态注解提供支持，用于指明哪些注解是运行时可见的 |
| RuntimeInvisibleAnnotations          | 类、方法表、字段表 | JDK1.5中新增的属性，与RuntimeVisibleAnnotations属性作用相反，指明哪些注解是运行时不可见的 |
| RuntimeVisibleParameterAnnotations   | 方法表             | JDK1.5中新增的属性，与RuntimeVisibleAnnotations属性作用类似，只不过作用对象为方法参数 |
| RuntimeInvisibleParameterAnnotations | 方法表             | JDK1.5中新增的属性，与RuntimeInvisibleAnnotations属性作用类似，只不过作用对象为方法参数 |
| AnnotationDefault                    | 方法表             | JDK1.5中新增的属性，用于记录注解类元素的默认值               |
| BootstrapMethods                     | 方法表             | JDK1.7中新增的属性，用于保存invokedynamic指令引用的引导方法限定符 |

属性表的结构如下：

| 类型 | 名称                 | 数量             |
| ---- | -------------------- | ---------------- |
| u2   | attribute_name_index | 1                |
| u4   | attribute_length     | 1                |
| u1   | info                 | attribute_length |

+ Code属性

  Java程序方法体中的代码经过Javac编译器处理后，最终变为字节码指令存储在Code属性内。Code属性出现在方法表的属性集合中，但并非所有的方法表都必须存在这个属性，譬如接口或抽象类中的方法就不存在Code属性。Code属性的结构如下表：

  | 类型           | 名称                   | 数量                   |
  | -------------- | ---------------------- | ---------------------- |
  | u2             | attribute_name_index   | 1                      |
  | u4             | attribute_length       | 1                      |
  | u2             | max_stack              | 1                      |
  | u2             | max_locals             | 1                      |
  | u4             | code_length            | 1                      |
  | u1             | code                   | code_length            |
  | u2             | exception_table_length | 1                      |
  | exception_info | exception_table        | exception_table_length |
  | u2             | attributes_count       | 1                      |
  | attribute_info | attributes             | attributes_count       |

  attribute_name_index是一项指向CONSTANT_Utf8_info型常量的索引，常量值固定为“Code”

  attribute_length指示了属性值的长度

  max_stack代表了操作数栈深度的最大值

  max_locals代表了局部变量表所需的存储空间，单位是Slot

  code_length和code用来存储Java源程序编译后生的字节码指令。code_length代表字节码长度，code是用于存储字节码指令的一系列字节流

  字节码指令之后是这个方法的显式异常处理表集合，异常表结构如下：

  | 类型 | 名称       | 数量 |
  | ---- | ---------- | ---- |
  | u2   | start_pc   | 1    |
  | u2   | end_pc     | 1    |
  | u2   | handler_pc | 1    |
  | u2   | catch_type | 1    |

+ Exceptions属性

  Exceptions属性的作用是列举出方法中可能抛出的受查异常，也就是方法描述时在throws关键字后面的异常。它的结构见下表：

  | 类型 | 名称                  | 数量                 |
  | ---- | --------------------- | -------------------- |
  | u2   | attribute_name_index  | 1                    |
  | u4   | attribute_length      | 1                    |
  | u2   | number_of_exceptions  | 1                    |
  | u2   | exception_index_table | number_of_exceptions |

+ LineNumberTable属性

  LineNumberTable属性用于描述Java源码行号与字节码行号之间的对应关系，它并不是运行时必需的属性。它的结构见下表：

  | 类型             | 名称                     | 数量                     |
  | ---------------- | ------------------------ | ------------------------ |
  | u2               | attribute_name_index     | 1                        |
  | u4               | attribute_length         | 1                        |
  | u2               | line_number_table_length | 1                        |
  | line_number_info | line_number_table        | line_number_table_length |

  line_number_info的结构如下：

  | 类型 | 名称        | 数量 |
  | ---- | ----------- | ---- |
  | u2   | start_pc    | 1    |
  | u2   | line_number | 1    |

+ LocalVariableTable属性

  LocalVariableTable属性用于描述栈帧中局部变量表中的变量与Java源码中定义的变量之间的关系，它也不是运行时必需的属性。

  | 类型                | 名称                        | 数量                        |
  | ------------------- | --------------------------- | --------------------------- |
  | u2                  | attribute_name_index        | 1                           |
  | u4                  | attribute_length            | 1                           |
  | u2                  | local_variable_table_length | 1                           |
  | local_variable_info | local_variable_table        | local_variable_table_length |

  local_variable_info的结构如下：

  | 类型 | 名称             | 数量 |
  | ---- | ---------------- | ---- |
  | u2   | start_pc         | 1    |
  | u2   | length           | 1    |
  | u2   | name_index       | 1    |
  | u2   | descriptor_index | 1    |
  | u2   | index            | 1    |

+ SourceFile属性

  SourceFile属性用于记录生成这个Class文件的源码文件名称，其表结构见下表：

  | 类型 | 名称                 | 数量 |
  | ---- | -------------------- | ---- |
  | u2   | attribute_name_index | 1    |
  | u4   | attribute_length     | 1    |
  | u2   | sourcefile_index     | 1    |

+ ConstantValue属性

  ConstantValue属性的作用是通知虚拟机自动为静态变量赋值。只有被static关键字修饰的变量才可以使用这项属性。它的表结构见下表：

  | 类型 | 名称                 | 数量 |
  | ---- | -------------------- | ---- |
  | u2   | attribute_name_index | 1    |
  | u4   | attribute_length     | 1    |
  | u2   | constantvalue_index  | 1    |

+ InnerClasses属性

  InnerClasses属性用于记录内部类与宿主类之间的关联。它的表结构见下表：

  | 类型               | 名称                 | 数量              |
  | ------------------ | -------------------- | ----------------- |
  | u2                 | attribute_name_index | 1                 |
  | u4                 | attribute_length     | 1                 |
  | u2                 | number_of_classes    | 1                 |
  | inner_classes_info | inner_classes        | number_of_classes |

  inner_classes_info的结构如下：

  | 类型 | 名称                       | 数量 |
  | ---- | -------------------------- | ---- |
  | u2   | inner_classes_info_index   | 1    |
  | u2   | outer_classes_info_index   | 1    |
  | u2   | inner_name_index           | 1    |
  | u2   | inner_classes_access_flags | 1    |

+ Deprecated和Synthetic属性

  Deprecated和Synthetic属性都属于标志类型的布尔属性，只存在有和没有的区别，没有属性值的概念。

  表结构见下表：

  | 类型 | 名称                 | 数量 |
  | ---- | -------------------- | ---- |
  | u2   | attribute_name_index | 1    |
  | u4   | attribute_length     | 1    |

  attribute_length的值必须为0x00000000

+ StrackMapTable属性

  StrackMapTable属性会在虚拟机类加载的字节码验证阶段被新类型检查验证器使用，目的在于替换以前比较消耗性能的基于数据流分析的类型推导验证器。表结构见下表：

  | 类型            | 名称                    | 数量              |
  | --------------- | ----------------------- | ----------------- |
  | u2              | attribute_name_index    | 1                 |
  | u4              | attribute_length        | 1                 |
  | u2              | number_of_entries       | 1                 |
  | stack_map_frame | stack_map_frame_entries | number_of_entries |

+ Signature属性

  Signature属性用于支持泛型情况下的方法签名，它的结构见下表：

  | 类型 | 名称                 | 数量 |
  | ---- | -------------------- | ---- |
  | u2   | attribute_name_index | 1    |
  | u4   | attribute_length     | 1    |
  | u2   | signature_index      | 1    |

+ BootstrapMethods属性

  BootstrapMethods属性用于保存invokedynamic指令引用的引导方法限定符，它的结构见下表：

  | 类型             | 名称                  | 数量                  |
  | ---------------- | --------------------- | --------------------- |
  | u2               | attribute_name_index  | 1                     |
  | u4               | attribute_length      | 1                     |
  | u2               | num_bootstrap_methods | 1                     |
  | bootstrap_method | bootstrap_methods     | num_bootstrap_methods |

  bootstrap_method的属性见下表：

  | 类型 | 名称                    | 数量                    |
  | ---- | ----------------------- | ----------------------- |
  | u2   | bootstrap_method_ref    | 1                       |
  | u2   | num_bootstrap_arguments | 1                       |
  | u2   | bootstrap_arguments     | num_bootstrap_arguments |
