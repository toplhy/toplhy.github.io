## 	类文件结构-字段表集合、方法表集合

### 字段表集合

  字段表用于描述接口或类中声明的变量。字段包括类级变量以及实例级变量，但不包括在方法内部声明的局部变量。

  字段表的结构如下表：

  | 名称           | 类型             | 数量             |
  | -------------- | ---------------- | ---------------- |
  | u2             | access_flags     | 1                |
  | u2             | name_index       | 1                |
  | u2             | descriptor_index | 1                |
  | u2             | attributes_count | 1                |
  | attribute_info | attributes       | attributes_count |

  access_flags可以设置的标志位和含义如下表：

  | 标志名称      | 标志值 | 含义                       |
  | ------------- | ------ | -------------------------- |
  | ACC_PUBLIC    | 0x0001 | 字段是否public             |
  | ACC_PRIVATE   | 0x0002 | 字段是否private            |
  | ACC_PROTECTED | 0x0004 | 字段是否protected          |
  | ACC_STATIC    | 0x0008 | 字段是否static             |
  | ACC_FINAL     | 0x0010 | 字段是否final              |
  | ACC_VOLATILE  | 0x0040 | 字段是否volatile           |
  | ACC_TRANSIENT | 0x0080 | 字段是否transient          |
  | ACC_SYNTHETIC | 0x1000 | 字段是否由编译器自动产生的 |
  | ACC_ENUM      | 0x4000 | 字段是否enum               |

  name_index和descriptor_index是对常量池的引用，分别代表着字段简单名称和方法的描述符。

  描述符含义如下表：

  | 标识字符 | 含义                          |
  | -------- | ----------------------------- |
  | B        | 基本类型byte                  |
  | C        | 基本类型char                  |
  | D        | 基本类型double                |
  | F        | 基本类型float                 |
  | I        | 基本类型int                   |
  | J        | 基本类型long                  |
  | S        | 基本类型short                 |
  | Z        | 基本类型boolean               |
  | V        | 特殊类型void                  |
  | L        | 对象类型，如Ljava/lang/Object |

  对于数组类型，每一维度将使用一个前置的“[”字符来描述，如“[[Ljava/lang/String”记录了一个String\[\]\[\]，“[I”记录了一个int\[\]。

  用描述符来描述方法时，按照先参数列表，后返回值的顺序描述，参数列表按照参数的严格顺序放在一组小括号之内。如void inc()的描述符是"()V"，int indexOf(char[] source,int sourceOffset, int sourceCount, char[] target, int targetOffset, int targetCount, int fromIndex)的描述符为“([CII[CIII)I”。

  最后是属性表集合用于存放一些额外的信息。

### 方法表集合

  方法表的结构几乎和字段表一样

  | 名称           | 类型             | 数量             |
  | -------------- | ---------------- | ---------------- |
  | u2             | access_flags     | 1                |
  | u2             | name_index       | 1                |
  | u2             | descriptor_index | 1                |
  | u2             | attributes_count | 1                |
  | attribute_info | attributes       | attributes_count |

  方法访问标志见下表：

  | 标志名称         | 标志值 | 含义                             |
  | ---------------- | ------ | -------------------------------- |
  | ACC_PUBLIC       | 0x0001 | 方法是否public                   |
  | ACC_PRIVATE      | 0x0002 | 方法是否private                  |
  | ACC_PROTECTED    | 0x0004 | 方法是否protected                |
  | ACC_STATIC       | 0x0008 | 方法是否static                   |
  | ACC_FINAL        | 0x0010 | 方法是否final                    |
  | ACC_SYNCHRONIZED | 0x0020 | 方法是否synchronized             |
  | ACC_BRIDGE       | 0x0040 | 方法是否是由编译器产生的桥接方法 |
  | ACC_VARARGS      | 0x0080 | 方法是否接受不定参数             |
  | ACC_NATIVE       | 0x0100 | 方法是否native                   |
  | ACC_ABSTRACT     | 0x0400 | 方法是否abstract                 |
  | ACC_STRICTFP     | 0x0800 | 方法是否strictfp                 |
  | ACC_SYNTHETIC    | 0x1000 | 方法是否由编译器自动产生的       |

  方法里的Java代码经过编译器编译成字节码指令后，存放在方法属性表集合中一个名为Code的属性里面。
