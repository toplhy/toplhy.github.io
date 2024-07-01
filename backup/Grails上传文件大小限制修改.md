## grails3 文件上传大小限制修改

上传文件时后台报错：
```
org.springframework.web.multipart.MultipartException: Could not parse multipart servlet request; nested exception is java.lang.IllegalStateException: org.apache.tomcat.util.http.fileupload.FileUploadBase$SizeLimitExceededException
```

搜到的解决方案都是在启动类中注入Bean，如下：
```
@Bean
public MultipartConfigElement multipartConfigElement() {
    MultipartConfigFactory factory = new MultipartConfigFactory();
    //  单个数据大小
    factory.setMaxFileSize(MaxFileSize); // KB,MB
    /// 总上传数据大小
    factory.setMaxRequestSize(MaxRequestSize);
    return factory.createMultipartConfig();
}

```
配置之后测试无效。

后查阅Grails官网文档，找到解决方法。

在grails-app/conf/application.yml增加一下配置：
```
grails:
    controllers:
        upload:
            maxFileSize: 2000000
            maxRequestSize: 2000000
```