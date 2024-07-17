记录grails3.x下使用cxf开发webservice

### 1. 在build.gradle中添加依赖

```
dependencies {
	...
	compile 'org.grails.plugins:cxf:3.1.2'
}
```

### 2. 创建Service类

```
import grails.gorm.transactions.Transactional
import org.grails.cxf.utils.GrailsCxfEndpoint

import javax.jws.WebMethod
import javax.jws.WebParam
import javax.jws.WebResult

@Transactional
@GrailsCxfEndpoint(address = '/demo')
class DemoService {

	@WebMethod
	@WebResult(name = "result")
	def serviceMethod(@WebParam(name = "id")Long id) {
		println "接收到的ID為：${id}"
		def map = [:]
		map.result = 1
		map.message = "success"
		return map.toString()
	}

}
```

### 3. 开发完成，启动项目

访问http://localhost:8080/services

### 4. 配置拦截器
因为业务需要，配置拦截器，定义LogDbInterceptor.groovy

```

import org.apache.cxf.helpers.IOUtils
import org.apache.cxf.interceptor.Fault
import org.apache.cxf.message.Message
import org.apache.cxf.phase.AbstractPhaseInterceptor

/**
 * 拦截器
 * 用以记录日志
 */
class LogDbInterceptor extends AbstractPhaseInterceptor<Message>{


    LogDbInterceptor(String phase) {
        super(phase)
    }

    @Override
    void handleMessage(Message message) throws Fault {
        InputStream is = message.getContent(InputStream.class)
        if (is != null) {
            try {
                // 得到报文
                String str = IOUtils.toString(is);
                println str
//                <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:rec="http://recruit.bjrxkj.com/">
//                    <soapenv:Header/>
//                    <soapenv:Body>
//                        <rec:serviceMethod>
//                            <!--Optional:-->
//                            <id>111111</id>
//                        </rec:serviceMethod>
//                    </soapenv:Body>
//                </soapenv:Envelope>
                // 流读取之后，会报Couldn't parse stream.
                InputStream ism = new ByteArrayInputStream(str.getBytes());
                message.setContent(InputStream.class, ism);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 5. 注入Bean

此处在Application中注入定义的拦截器

```
import com.bjrxkj.recruit.interceptor.LogDbInterceptor
import grails.boot.GrailsApp
import grails.boot.config.GrailsAutoConfiguration
import org.apache.cxf.phase.Phase
import org.springframework.context.annotation.Bean

class Application extends GrailsAutoConfiguration {
    static void main(String[] args) {
        GrailsApp.run(Application, args)
    }

    @Bean
    public LogDbInterceptor logDbInterceptor() {
        // Phase.RECEIVE 在接收的时候进行拦截
        return new LogDbInterceptor(Phase.RECEIVE);
    }
}
```

### 6. webservice引用拦截器

```
import grails.gorm.transactions.Transactional
import org.grails.cxf.utils.GrailsCxfEndpoint

import javax.jws.WebMethod
import javax.jws.WebParam
import javax.jws.WebResult

@Transactional
@GrailsCxfEndpoint(address = '/demo', inInterceptors = ['logDbInterceptor'])
class DemoService {

    @WebMethod
    @WebResult(name = "result")
    def serviceMethod(@WebParam(name = "id")Long id) {
        println "接收到的ID為：${id}"
        def map = [:]
        map.result = 1
        map.message = "success"
        return map.toString()
    }
}
```


