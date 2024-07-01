## 魔数与Class版本号

每个Class文件的头4个字节称为魔数，它的唯一作用是确定这个文件是否为一个能被虚拟机接受的Class文件。使用魔数而不是扩展名来进行识别主要是基于安全方面的考虑，因为文件扩展名可以随意改动。

Class文件的魔数值为：0xCAFEBABE。

紧接着魔数的4个字节存储的是Class文件版本号：第5和第6个字节是次版本号（Minor Version），第7和第8个字节是主版本号（Major Version）。如下图，次版本号为0x0000，主版本号为0x0034。

Linux下使用vim或者hexedit查看class文件。

![jvm_26](/images/jvm/jvm_26.png)
