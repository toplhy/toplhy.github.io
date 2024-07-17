Grails版本: 4.0.4

项目中引入quartz插件：
```
compile 'org.grails.plugins:quartz:2.0.13'
```
启动报错：
```
Caused by: java.lang.NoClassDefFoundError: org/quartz/JobExecutionContext
```
解决：
```
compile("org.quartz-scheduler:quartz:2.2.3") {
    exclude group: 'slf4j-api', module: 'c3p0'
}
compile ('org.grails.plugins:quartz:2.0.13')
```
参考[https://github.com/grails-plugins/grails-quartz/issues/107](https://github.com/grails-plugins/grails-quartz/issues/107)
