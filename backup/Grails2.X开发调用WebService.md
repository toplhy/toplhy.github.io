## 开发webservice接口

### 1. 新建grails1/2工程项目

### 2. 在BuildConfig.groovy文件的plugins闭包中加入cxf服务端插件：

```
compile "org.grails.plugins:cxf:2.1.1"
```

### 3. 在Config.groovy文件中配置cxf
![1](https://github.com/toplhy/toplhy.github.io/blob/main/images/grails/grails_1.png?raw=true)

### 4. 在grails-app/services目录下创建一个TestService类

### 5. 启动项目
输入http://localhost:8080/cxfProject/services 可以看到我们开发的接口，点击链接就是生成的WSDL文件。

![2](https://github.com/toplhy/toplhy.github.io/blob/main/images/grails/grails_2.png?raw=true)


## 调用webservice接口

### 1. 在BuildConfig.groovy文件的plugins闭包中加入cxf客户端件：

```
compile "org.grails.plugins:cxf-client:2.1.2"
```

### 2. 在Config.groovy文件中配置cxf客户端：
![3](https://github.com/toplhy/toplhy.github.io/blob/main/images/grails/grails_3.png?raw=true)
其中，wsdl和namespce用来生成客户端文件，clientInterface和serviceEndpointAddress是调用是必须配置的。

### 3. 运行grails wsdl2java命令在src/java下生成客户端文件

### 4. 接下来就可以写代码调用接口了。

以上只是开发简单的webservice接口，没有用到安全拦截器等高端操作，使用时可以查看官方文档。

cxf： https://github.com/Grails-Plugin-Consortium/grails-cxf/tree/grails-2

cxf-client： https://github.com/Grails-Plugin-Consortium/grails-cxf-client/tree/grails-2
