### 1. IDEA配置远程信息

![1](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/java/remote_1.png)

### 2. 配置远程服务

+ 复制图1中的Command line arguments for remote JVM

```
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005
```

+ 在远程服务上添加第二步的参数

此处以tomcat示例，编辑catalina.sh文件，添加参数。

![1](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/java/remote_2.png)

直接以jar包运行的项目，直接在启动命令添加即可：

```
java `Command line arguments for remote JVM` -jar xxx.jar ...
```

### 3. 启动远程服务

![1](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/java/remote_3.png)

### 4. 开始远程调试
