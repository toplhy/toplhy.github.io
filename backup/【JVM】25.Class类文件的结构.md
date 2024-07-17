## Class类文件的结构

Java虚拟机不和包括Java在内的任何语言绑定，它只与Class文件这种特定的二进制文件格式关联，Class文件中包含了Java虚拟机指令集和符号表以及若干其他辅助信息。

Class文件是一组以8位字节为基础单元的二进制流，各个数据项目严格按照顺序紧凑排列，中间没有添加任何分隔符。当需要占用8位字节以上空间的数据项目时，会按照高位在前的方式分割成若干个8位字节进行存储。

Class文件采用一种类似于C语言结构体的的伪结构来存储数据，这种伪结构中只有两种数据类型：无符号数和表。

+ 无符号数属于基本的数据类型，以u1、u2、u4、u8来分别代表1个字节、2个字节、4个字节和8个字节的无符号数，无符号数可以用来描述数字、索引引用、数量值或者按照UTF-8编码构成字符串值。
+ 表是由多个无符号数或者其他表作为数据项构成的复合数据类型，所有表都习惯性地以“_info”结尾。表用于描述有层次关系的复合结构的数据，整个Class文件本质上就是一张表。

Class文件格式如下：

| 类型           | 名称                | 数量                  |
| -------------- | ------------------- | --------------------- |
| u4             | magic               | 1                     |
| u2             | minor_version       | 1                     |
| u2             | major_version       | 1                     |
| u2             | constant_pool_count | 1                     |
| cp_info        | constant_pool       | constant_pool_count-1 |
| u2             | access_flags        | 1                     |
| u2             | this_class          | 1                     |
| u2             | super_class         | 1                     |
| u2             | interfaces_count    | 1                     |
| u2             | interfaces          | interfaces_count      |
| u2             | field_count         | 1                     |
| field_info     | fields              | fields_count          |
| u2             | methods_count       | 1                     |
| method_info    | methods             | methods_count         |
| u2             | attributes_count    | 1                     |
| attribute_info | attributes          | attributes_count      |

无论是无符号数还是表，当需要描述同一类型但数量不定的多个数据时，经常会使用一个前置的容量计数器加若干个连续的数据项的形式，这时称这一系列连续的某一类型的数据为某一类型的集合。
