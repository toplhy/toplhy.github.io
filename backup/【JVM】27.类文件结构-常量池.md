## 类文件结构-常量池

紧接着主次版本号之后的是常量池入口，常量池可以理解为Class文件中的资源仓库，它是Class文件结构中与其他项目关联最多的数据类型，同时它还是在Class文件中第一个出现的表类型数据项目。

常量池中的常量的数量不是固定的，所以在常量池的入口有一项u2类型的数据，代表常量池容量计数值，这个值是从1开始而不是0。第0项常量空出来的目的在于满足后面某些指向常量池的索引值的数据在特定情况下需要表达“不引用任何一个常量池项目”的含义。

常量池中主要存放两大类常量：字面量（Literal）和符号引用（Symbolic References）。字面量比较接近Java语言层面的常量概念，如文本字符串、声明为final的常量值等。而符号引用则属于编译原理方面的概念，包括下面三类常量：

+ 类和接口的全限定名
+ 字段的名称和描述符
+ 方法的名称和描述符

常量池的每一项常量都是一个表，在JDK1.7中共有14种结构不同的表结构数据，这14种表有一个共同的特点，就是在表开始的第一位是一个u1类型的标志位（tag），代表当前这个常量属于哪种常量类型。这14种类型代表的含义如下表：

| 常量类型                         | 标志 | 描述                     |
| -------------------------------- | ---- | ------------------------ |
| CONSTANT_Utf8_info               | 1    | UTF-8编码的字符串        |
| CONSTANT_Integer_info            | 3    | 整形字面量               |
| CONSTANT_Float_info              | 4    | 浮点型字面量             |
| CONSTANT_Long_info               | 5    | 长整形字面量             |
| CONSTANT_Double_info             | 6    | 双精度浮点型字面量       |
| CONSTANT_Class_info              | 7    | 类或接口的符号引用       |
| CONSTANT_String_info             | 8    | 字符串类型字面量         |
| CONSTANT_Fieldref_info           | 9    | 字段的符号引用           |
| CONSTANT_Methodref_info          | 10   | 类中方法的符号引用       |
| CONSTANT_InterfaceMethodref_info | 11   | 接口中方法的符号引用     |
| CONSTANT_NameAndType_info        | 12   | 字段或方法的部分符号引用 |
| CONSTANT_MethodHandle_info       | 15   | 表示方法句柄             |
| CONSTANT_MethodType_info         | 16   | 标识方法类型             |
| CONSTANT_InvokeDynamic_info      | 18   | 表示一个动态方法调用点   |

CONSTANT_Utf8_info常量的结构如下表：

| 名称   | 类型 | 描述                            |
| ------ | ---- | ------------------------------- |
| tag    | u1   | 值为1                           |
| length | u2   | UTF-8编码的字符串占用的字节数   |
| bytes  | u1   | 长度为length的UTF-8编码的字符串 |

CONSTANT_Integer_info常量的结构如下表：

| 名称  | 类型 | 描述                    |
| ----- | ---- | ----------------------- |
| tag   | u1   | 值为3                   |
| bytes | u4   | 按照高位在前存储的int值 |

CONSTANT_Float_info常量的结构如下表：

| 名称  | 类型 | 描述                      |
| ----- | ---- | ------------------------- |
| tag   | u1   | 值为4                     |
| bytes | u4   | 按照高位在前存储的float值 |

CONSTANT_Long_info常量的结构如下表：

| 名称  | 类型 | 描述                     |
| ----- | ---- | ------------------------ |
| tag   | u1   | 值为5                    |
| bytes | u8   | 按照高位在前存储的long值 |

CONSTANT_Double_info常量的结构如下表：

| 名称  | 类型 | 描述                       |
| ----- | ---- | -------------------------- |
| tag   | u1   | 值为6                      |
| bytes | u8   | 按照高位在前存储的double值 |

CONSTANT_Class_info常量的结构如下表：

| 名称  | 类型 | 描述                   |
| ----- | ---- | ---------------------- |
| tag   | u1   | 值为7                  |
| index | u2   | 指向全限定常量项的索引 |

CONSTANT_String_info常量的结构如下表：

| 名称  | 类型 | 描述                   |
| ----- | ---- | ---------------------- |
| tag   | u1   | 值为8                  |
| index | u2   | 指向字符串字面量的索引 |

CONSTANT_Fieldref_info常量的结构如下表：

| 名称  | 类型 | 描述                                                    |
| ----- | ---- | ------------------------------------------------------- |
| tag   | u1   | 值为9                                                   |
| index | u2   | 指向声明字段的类或接口描述符CONSTANT_Class_info的索引项 |
| index | u2   | 指向字段描述符CONSTANT_NameAndType_info的索引项         |

CONSTANT_Methodref_info常量的结构如下表：

| 名称  | 类型 | 描述                                                  |
| ----- | ---- | ----------------------------------------------------- |
| tag   | u1   | 值为10                                                |
| index | u2   | 指向声明方法的类描述符CONSTANT_Class_info的索引项     |
| index | u2   | 指向名称及类型描述符CONSTANT_NameAndType_info的索引项 |

CONSTANT_InterfaceMethodref_info常量的结构如下表：

| 名称  | 类型 | 描述                                                |
| ----- | ---- | --------------------------------------------------- |
| tag   | u1   | 值为11                                              |
| index | u2   | 指向声明方法的接口描述符CONSTANT_Class_info的索引项 |
| index | u2   | 指向字段描述符CONSTANT_NameAndType_info的索引项     |

CONSTANT_NameAndType_info  常量的结构如下表：

| 名称  | 类型 | 描述                               |
| ----- | ---- | ---------------------------------- |
| tag   | u1   | 值为12                             |
| index | u2   | 指向该字段或方法名称常量项的索引   |
| index | u2   | 指向该字段或方法描述符常量项的索引 |

CONSTANT_MethodHandle_info常量的结构如下表：

| 名称            | 类型 | 描述                                                |
| --------------- | ---- | --------------------------------------------------- |
| tag             | u1   | 值为15                                              |
| reference_kind  | u1   | 值必须在1-9之间（包括1和9），它决定了方法句柄的类型 |
| reference_index | u2   | 值必须是对常量池的有效索引                          |

CONSTANT_MethodType_info常量的结构如下表：

| 名称             | 类型 | 描述                                                         |
| ---------------- | ---- | ------------------------------------------------------------ |
| tag              | u1   | 值为16                                                       |
| descriptor_index | u2   | 值必须是对常量池的有效索引，常量池在该索引处必须是CONSTANT_Utf8_info结构 |

CONSTANT_InvokeDynamic_info常量的结构如下表：

| 名称                        | 类型 | 描述                                                         |
| --------------------------- | ---- | ------------------------------------------------------------ |
| tag                         | u1   | 值为18                                                       |
| bootstrap_method_attr_index | u2   | 值必须是对当前Class文件中引导方法表的bootstrap_methods[]数组的有效索引 |
| name_and_type_index         | u2   | 值必须是对常量池的有效索引，常量池在该索引处的项必须是CONSTANT_NameAndType_info结构 |


