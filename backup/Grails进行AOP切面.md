记录在Grails3中使用AOP进行切面编程

### 1. build.gradle中加入依赖
```
compile "org.springframework.boot:spring-boot-starter-aop"
```

### 2. 在resources.groovy中声明spring命名空间
```
beans{
      xmlns aop:"http://www.springframework.org/schema/aop"
}
```

### 3. 注入bean，进行配置
```
beans{
      xmlns aop:"http://www.springframework.org/schema/aop"
      logAspect(LogAspect)
      aop {
            config("proxy-target-class": true) {
                  pointcut("id": "controllerPointcut", "expression": "execution(* cas.interfaces.*.*(..))")
                  aspect("id": "logAspect", "ref": "logAspect") {
                         "after-returning" "method":"doAfterReturning","pointcut-ref": "controllerPointcut", "returning": "returnObj"
                         "after-throwing" "method":"doAfterThrowing","pointcut-ref": "controllerPointcut", "throwing": "throwObj"
                  }
            }
       }
}
```
以上配置的意思是：

proxy-target-class：true表示基于类的代理将使用,false表示默认使用Jdk基于接口的代理；

pointcut是定义切入点，execution(* cas.interfaces.*.*(..))表示cas.interfaces包下的类的方法；

aspect是定义切面，指向logAspect的bean，里面定义了切入的时机和切入时执行的方法。

### 4. 编写切入方法

```
import com.rici.cas.dics.InterfaceLog
import com.rici.cas.interfaces.IpDataUtils
import grails.converters.JSON
import org.aspectj.lang.JoinPoint
import org.grails.web.servlet.mvc.GrailsWebRequest
import org.springframework.web.context.request.RequestAttributes
import org.springframework.web.context.request.RequestContextHolder
import org.springframework.web.context.request.ServletRequestAttributes

import javax.servlet.http.HttpServletRequest

/**
 * 切面日志记录
 */
class LogAspect {

    def doAfterReturning(JoinPoint joinPoint, Object returnObj) {
        doLog(joinPoint, returnObj?.toString())
    }

    def doAfterThrowing(JoinPoint joinPoint, Throwable throwObj) {
        doLog(joinPoint, throwObj?.toString())
    }
    
    def doLog(JoinPoint joinPoint, responseContent){
        RequestAttributes requestAttributes = RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = ((ServletRequestAttributes)requestAttributes).getRequest();

        def params = ((GrailsWebRequest)RequestContextHolder.currentRequestAttributes()).getParams()
        params.remove("format")
        params.remove("action")
        params.remove("controller")
        def ipAddr = IpDataUtils.getIpAddress(request)
        def log = new InterfaceLog(appId: params?.appId, ipAddr: ipAddr, requestUri: request.requestURI, interfaceName: joinPoint.getSignature().getDeclaringTypeName(),
                methodName: joinPoint.getSignature().getName(), parameters: (params as JSON).toString(), method: request.getMethod(), result: 2,
                responseContent: responseContent)
        log.save(flush: true)
        println log?.errors?.allErrors
    }
}


```

ps：遇到的一个问题是joinPoint.getArgs()一直得不到请求的参数，还未解决，目前是通过GrailsWebRequest来获取的
