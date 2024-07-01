记录达梦数据库创建JAVA外部函数的过程

### 开发java函数功能
开发java函数功能，输出jar包

### 将jar包安装到数据库
按照文档中例子中再DM安装目录bin下创建external_jar目录，将jar包放入了进去。

### 查看外部函数功能是否开启
达梦默认关闭外部函数的创建和执行功能，DM 提供了 ini 参数 ENABLE_EXTERNAL_CALL 来开关外部函数功能.
```
select para_name,para_value,para_type from v$dm_ini where para_name = 'ENABLE_EXTERNAL_CALL';
```

### 打开外部函数功能，并重启数据库
```
sp_set_para_value(2,'ENABLE_EXTERNAL_CALL',1);
```

### 创建外部函数
```
CREATE OR REPLACE FUNCTION SIMILARITY(str1 varchar, str2 varchar)
RETURN double
EXTERNAL '..\dmdbms\tool\dmagent\lib\external_jar\nlp-word.jar' "com.rici.spp.Cosine.getSimilarity" USING JAVA;
```

### 启动DMAgent

JAVA外部函数的执行都通过代理工具DMAgent进行。

DMAgent执行程序在 DM8安装目录的 tool/dmagent子目录，目录下有readme文件。

DMAgent 存在 3 种运行模式，开启 java 外部函数的代理模式 assist process。

使用外部函数功能时DMAgent 配置文件中 ap_port 要和 dm.ini 中的 EXTERNAL_JFUN_PORT保持一致。

```
select para_name,para_value,para_type from v$dm_ini where para_name = 'EXTERNAL_JFUN_PORT';
```

### 遇到的问题

#### 1. “外部函数共享库加载失败”

<b>解决：</b>将jar包放到了DMAgment目录/lib下

#### 2. DMAgent的三种模式
![DMAgent的三种模式](/images/database/2020-05-11-3.png)

使用后两种模式应该同时开启外部函数代理模式，但实际调用时无法连接到。telnet代理模式6363端口不通。
