---
title: arther学习
date: 2021-11-21 10:38:41
tags:
---
# Arthes学习

1. arthes 是什么

   线上整断工具

2. 这次涉及哪些命令

# logger

1. 查看对应的logger
- logger

```
name         ROOT
class        ch.qos.logback.classic.Logger
classLoader  org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb
classLoader  6267c3bb
Hash
level        INFO
effectiveLe  INFO
vel
additivity   true
codeSource   jar:file:/Users/ezreal/temp/demo/target/demo-0.0.1-SNAPSHOT.jar!/
             BOOT-INF/lib/logback-classic-1.2.7.jar!/
appenders    name            CONSOLE
             class           ch.qos.logback.core.ConsoleAppender
             classLoader     org.springframework.boot.loader.LaunchedURLClass
                             Loader@6267c3bb
             classLoaderHash 6267c3bb
             target          System.out
             name            APPLICATION
             class           ch.qos.logback.core.rolling.RollingFileAppender
             classLoader     org.springframework.boot.loader.LaunchedURLClass
                             Loader@6267c3bb
             classLoaderHash 6267c3bb
             file            app.log
             name            ASYNC
             class           ch.qos.logback.classic.AsyncAppender
             classLoader     org.springframework.boot.loader.LaunchedURLClass
                             Loader@6267c3bb
             classLoaderHash 6267c3bb
             blocking        true
             appenderRef     [APPLICATION]
```

- logger -c 6267c3bb --name ROOT --level debug

```
Update logger level success.
```


    
### 问题
    
使用了高版本的slf4j 打印logger会无效
    
```
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>2.0.0-alpha5</version>
</dependency>
```
    
# classloader
    
查看classloader的继承树，urls，类加载信息

classloader 命令将 JVM 中所有的classloader的信息统计出来，并可以展示继承树，urls等。

可以让指定的classloader去getResources，打印出所有查找到的resources的url。对于ResourceNotFoundException比较有用。

### 参数说明

[classloader参数说明](https://www.notion.so/9a1d036f22104adfb90818b78c1f5610)

### 按类加载类型查看统计信息

- classloader
    
    ```
    [arthas@62190]$ 
     name                                                    numberOfInstances  loadedCountTotal
     org.springframework.boot.loader.LaunchedURLClassLoader  1                  4003
     BootstrapClassLoader                                    1                  3360
     com.taobao.arthas.agent.ArthasClassloader               1                  1352
     sun.misc.Launcher$AppClassLoader                        1                  61
     sun.reflect.DelegatingClassLoader                       60                 60
     sun.misc.Launcher$ExtClassLoader                        1                  58
    Affect(row-cnt:6) cost in 11 ms.
    ```
    

### 按类加载实例查看统计信息
    
- classloader -l
    
    ```
    [arthas@62190]$
     name                                                             loadedCount  hash      parent
     BootstrapClassLoader                                             3402         null      null
     com.taobao.arthas.agent.ArthasClassloader@6d09b6d4               1364         6d09b6d4  sun.misc.Launcher$ExtClassLoader@bebdb06
     org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb  4216         6267c3bb  sun.misc.Launcher$AppClassLoader@55f96302
     sun.misc.Launcher$AppClassLoader@55f96302                        61           55f96302  sun.misc.Launcher$ExtClassLoader@bebdb06
     sun.misc.Launcher$ExtClassLoader@bebdb06                         58           bebdb06   null
    Affect(row-cnt:5) cost in 22 ms.
    ```
    

### 查看ClassLoader的继承树

- classloader -t
    
    ```
    [arthas@62190]$ 
    +-BootstrapClassLoader
    +-sun.misc.Launcher$ExtClassLoader@bebdb06
      +-com.taobao.arthas.agent.ArthasClassloader@6d09b6d4
      +-sun.misc.Launcher$AppClassLoader@55f96302
        +-org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb
    Affect(row-cnt:5) cost in 8 ms.
    ```
    
- classload
    
```
jsx[arthas@51951]$ classloader -t
   +-BootstrapClassLoader
   +-sun.misc.Launcher$ExtClassLoader@bebdb06
     +-com.taobao.arthas.agent.ArthasClassloader@20d25ec3
     +-sun.misc.Launcher$AppClassLoader@55f96302
       +-org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb
         +-TomcatEmbeddedWebappClassLoader
             context: ROOT
             delegate: true
           ----------> Parent Classloader:
           org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb
       
           +-java.net.URLClassLoader@7a0b7a00
           +-java.net.URLClassLoader@5e7bbf14
   Affect(row-cnt:8) cost in 7 ms.
```
        
- classloader -c 26548706 -r META-INF/MANIFEST.MF
        
```
jsx
jar:file:/System/Library/Java/Extensions/MRJToolkit.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/.arthas/lib/3.5.4/arthas/arthas-agent.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/demo-common-0.0.1-SNAPSHOT.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/slf4j-api-1.7.32.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/commons-lang3-3.12.0.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/lombok-1.18.22.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-2.5.7.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-autoconfigure-2.5.7.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/logback-classic-1.2.7.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/logback-core-1.2.7.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/log4j-to-slf4j-2.14.1.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/log4j-api-2.14.1.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jul-to-slf4j-1.7.32.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/snakeyaml-1.28.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jackson-databind-2.12.5.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jackson-annotations-2.12.5.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jackson-core-2.12.5.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jackson-datatype-jdk8-2.12.5.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jackson-datatype-jsr310-2.12.5.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jackson-module-parameter-names-2.12.5.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-web-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-beans-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-webmvc-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-aop-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-context-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-expression-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/jakarta.annotation-api-1.3.5.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/tomcat-embed-core-9.0.55.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/tomcat-embed-el-9.0.55.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/tomcat-embed-websocket-9.0.55.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-core-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-jcl-5.3.13.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-jarmode-layertools-2.5.7.jar!/META-INF/MANIFEST.MF
jar:file:/Users/ezreal/temp/demo/launch-admin/target/classes/plugin/demo-duck-0.0.1-SNAPSHOT-jar-with-dependencies.jar!/META-INF/MANIFEST.MF
```
        
- classloader -t
        
```
+-BootstrapClassLoader
+-sun.misc.Launcher$ExtClassLoader@bebdb06
  +-com.taobao.arthas.agent.ArthasClassloader@3991f7c9
  +-sun.misc.Launcher$AppClassLoader@55f96302
    +-org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb
      +-TomcatEmbeddedWebappClassLoader
          context: ROOT
          delegate: true
        ----------> Parent Classloader:
        org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb
  
        +-java.net.URLClassLoader@26548706
Affect(row-cnt:7) cost in 11 ms.
```
        
    
## watch
    
查看返回
    
- watch com.example.demo.service.MathGame primeFactors "{params,target,returnObj}" -n 1
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 1) cost in 29 ms, listenerId: 14
        method=com.example.demo.service.MathGame.primeFactors location=AtExit
        ts=2021-12-09 21:41:10; [cost=2.282282ms] result=@ArrayList[
            @Object[][isEmpty=false;size=1],
            @MathGame[com.example.demo.service.MathGame@4eec4bc6],
            @ArrayList[isEmpty=false;size=3],
        ]
        
```
        
    
观察之前-b
    
- watch com.example.demo.service.MathGame primeFactors -x 2 -b -n 2
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 1) cost in 136 ms, listenerId: 8
        method=com.example.demo.service.MathGame.primeFactors location=AtEnter
        ts=2021-12-09 23:43:40; [cost=0.148451ms] result=@ArrayList[
            @Object[][
                @Integer[149371],
            ],
            @MathGame[
                logger=@Logger[Logger[com.example.demo.service.MathGame]],
                random=@Random[java.util.Random@1cb4e5dc],
                illegalArgumentCount=@Integer[250],
                running=@Boolean[true],
            ],
            null,
        ]
        method=com.example.demo.service.MathGame.primeFactors location=AtEnter
        ts=2021-12-09 23:43:41; [cost=0.015406ms] result=@ArrayList[
            @Object[][
                @Integer[126691],
            ],
            @MathGame[
                logger=@Logger[Logger[com.example.demo.service.MathGame]],
                random=@Random[java.util.Random@1cb4e5dc],
                illegalArgumentCount=@Integer[250],
                running=@Boolean[true],
            ],
            null,
        ]
        Command execution times exceed limit: 2, so command will exit. You can set it with -n option.
        
```
        
    
查看条件下的返回值
    
- watch com.example.demo.service.MathGame primeFactors "{params[0],target}" "params[0]<0"
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 1) cost in 67 ms, listenerId: 9
        method=com.example.demo.service.MathGame.primeFactors location=AtExceptionExit
        ts=2021-12-09 23:45:50; [cost=0.477431ms] result=@ArrayList[
            @Integer[-189035],
            @MathGame[com.example.demo.service.MathGame@380bab48],
        ]
        method=com.example.demo.service.MathGame.primeFactors location=AtExceptionExit
        ts=2021-12-09 23:45:51; [cost=0.235983ms] result=@ArrayList[
            @Integer[-63330],
            @MathGame[com.example.demo.service.MathGame@380bab48],
        ]
        method=com.example.demo.service.MathGame.primeFactors location=AtExceptionExit
        ts=2021-12-09 23:45:52; [cost=0.191665ms] result=@ArrayList[
            @Integer[-154052],
            @MathGame[com.example.demo.service.MathGame@380bab48],
        ]
```
        
    
查看异常情况下的返回值
    
- watch com.example.demo.service.MathGame primeFactors "{params[0],throwExp}" -e -x 2
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 1) cost in 34 ms, listenerId: 11
        method=com.example.demo.service.MathGame.primeFactors location=AtExceptionExit
        ts=2021-12-09 23:47:15; [cost=0.440865ms] result=@ArrayList[
            @Integer[-12954],
            java.lang.IllegalArgumentException: number is: -12954, need >= 2
        	at com.example.demo.service.MathGame.primeFactors(MathGame.java:60)
        	at com.example.demo.service.MathGame.run(MathGame.java:37)
        	at com.example.demo.service.MathGame.startGame(MathGame.java:26)
        	at com.example.demo.controller.MathGameController.startGame(MathGameController.java:18)
        	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        	at java.lang.reflect.Method.invoke(Method.java:498)
        	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205)
        	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:150)
        	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117)
        	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895)
        	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:808)
        	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)
        	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1067)
        	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:963)
        	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)
        	at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898)
        	at javax.servlet.http.HttpServlet.service(HttpServlet.java:655)
        	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)
        	at javax.servlet.http.HttpServlet.service(HttpServlet.java:764)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:227)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)
        	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)
        	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)
        	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:197)
        	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97)
        	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:540)
        	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:135)
        	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)
        	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78)
        	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:357)
        	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:382)
        	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)
        	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:895)
        	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1722)
        	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)
        	at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191)
        	at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)
        	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
        	at java.lang.Thread.run(Thread.java:748)
        ,
        ]
        method=com.example.demo.service.MathGame.primeFactors location=AtExceptionExit
        ts=2021-12-09 23:47:16; [cost=0.186838ms] result=@ArrayList[
            @Integer[-200115],
            java.lang.IllegalArgumentException: number is: -200115, need >= 2
        	at com.example.demo.service.MathGame.primeFactors(MathGame.java:60)
        	at com.example.demo.service.MathGame.run(MathGame.java:37)
        	at com.example.demo.service.MathGame.startGame(MathGame.java:26)
        	at com.example.demo.controller.MathGameController.startGame(MathGameController.java:18)
        	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        	at java.lang.reflect.Method.invoke(Method.java:498)
        	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205)
        	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:150)
        	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117)
        	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895)
        	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:808)
        	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)
        	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1067)
        	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:963)
        	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)
        	at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898)
        	at javax.servlet.http.HttpServlet.service(HttpServlet.java:655)
        	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)
        	at javax.servlet.http.HttpServlet.service(HttpServlet.java:764)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:227)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)
        	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)
        	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)
        	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
        	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)
        	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)
        	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:197)
        	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97)
        	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:540)
        	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:135)
        	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)
        	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78)
        	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:357)
        	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:382)
        	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)
        	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:895)
        	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1722)
        	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)
        	at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191)
        	at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)
        	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
        	at java.lang.Thread.run(Thread.java:748)
        ,
        ]
```
        
    
查看耗时时间
    
- watch com.example.demo.service.MathGame primeFactors '{params, returnObj}' '#cost>10' -x 2
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 1) cost in 126 ms, listenerId: 1
        method=com.example.demo.service.MathGame.primeFactors location=AtExceptionExit
        ts=2021-12-09 23:54:21; [cost=1005.196186ms] result=@ArrayList[
            @Object[][
                @Integer[-66484],
            ],
            null,
        ]
        method=com.example.demo.service.MathGame.primeFactors location=AtExceptionExit
        ts=2021-12-09 23:54:23; [cost=1005.69874ms] result=@ArrayList[
            @Object[][
                @Integer[-124272],
            ],
            null,
        ]
        method=com.example.demo.service.MathGame.primeFactors location=AtExit
        ts=2021-12-09 23:54:25; [cost=1005.44769ms] result=@ArrayList[
            @Object[][
                @Integer[1],
            ],
            @ArrayList[
                @Integer[2],
                @Integer[3],
                @Integer[61],
                @Integer[167],
            ],
        ]
```
        
    
一些特殊的用法[https://github.com/alibaba/arthas/issues/71](https://github.com/alibaba/arthas/issues/71)
    
# tt
    
方法执行数据的时空隧道，记录下指定方法每次调用的入参和返回信息，并能对这些不同的时间下调用进行观测
    
- tt -t *MathGame primeFactors
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 1) cost in 193 ms, listenerId: 4
         INDEX      TIMESTAMP                   COST(ms)      IS-RET      IS-EXP     OBJECT               CLASS                                     METHOD
        -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
         1000       2021-12-10 00:18:09         4.2872283446  true        false      0x77326531           MathGame                                  primeFactors
                                                9185E8
         1001       2021-12-10 00:18:11         1005.300827   false       true       0x77326531           MathGame                                  primeFactors
         1002       2021-12-10 00:18:13         1000.34044    false       true       0x77326531           MathGame                                  primeFactors
         1003       2021-12-10 00:18:15         1005.074176   false       true       0x77326531           MathGame                                  primeFactors
         1004       2021-12-10 00:18:17         1007.539237   true        false      0x77326531           MathGame                                  primeFactors
         1005       2021-12-10 00:18:19         1003.290587   true        false      0x77326531           MathGame                                  primeFactors
         1006       2021-12-10 00:18:21         1003.784568   true        false      0x77326531           MathGame                                  primeFactors
         1007       2021-12-10 00:18:23         1005.241037   true        false      0x77326531           MathGame                                  primeFactors
         1008       2021-12-10 00:18:25         1005.282362   true        false      0x77326531           MathGame                                  primeFactors
         1009       2021-12-10 00:18:27         1005.280449   true        false      0x77326531           MathGame                                  primeFactors
         1010       2021-12-10 00:18:29         1001.732149   true        false      0x77326531           MathGame                                  primeFactors
         1011       2021-12-10 00:18:31         1005.430258   false       true       0x77326531           MathGame                                  primeFactors
         1012       2021-12-10 00:18:33         1004.76114    false       true       0x77326531           MathGame                                  primeFactors
         1013       2021-12-10 00:18:35         1003.424502   false       true       0x77326531           MathGame                                  primeFactors
         1014       2021-12-10 00:18:37         1003.070364   true        false      0x77326531           MathGame                                  primeFactors
         1015       2021-12-10 00:18:39         1000.755568   true        false      0x77326531           MathGame                                  primeFactors
         1016       2021-12-10 00:18:41         1004.861604   true        false      0x77326531           MathGame                                  primeFactors
         1017       2021-12-10 00:18:43         1001.180428   false       true       0x77326531           MathGame                                  primeFactors
         1018       2021-12-10 00:18:45         1004.164503   false       true       0x77326531           MathGame                                  primeFactors
         1019       2021-12-10 00:18:47         1001.723306   true        false      0x77326531           MathGame                                  primeFactors
         1020       2021-12-10 00:18:49         1001.636767   true        false      0x77326531           MathGame                                  primeFactors
         1021       2021-12-10 00:18:51         1001.050037   false       true       0x77326531           MathGame                                  primeFactors
```
        
    
流量回放
    
- tt -i 1020 -p
        
```jsx
        RE-INDEX       1020
         GMT-REPLAY     2021-12-10 00:23:41
         OBJECT         0x77326531
         CLASS          com.example.demo.service.MathGame
         METHOD         primeFactors
         PARAMETERS[0]  @Integer[116312]
         IS-RETURN      true
         IS-EXCEPTION   false
         COST(ms)       1003.278846
         RETURN-OBJ     @ArrayList[
                            @Integer[2],
                            @Integer[2],
                            @Integer[2],
                            @Integer[7],
                            @Integer[31],
                            @Integer[67],
                        ]
        Time fragment[1020] successfully replayed 1 times.
```
        
    
# trace
查看一段链路中耗时以及运行
    
- trace com.example.demo.controller.UserController *
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 2) cost in 235 ms, listenerId: 5
        `---ts=2021-12-10 00:28:18;thread_name=http-nio-8080-exec-2;id=26;is_daemon=true;priority=5;TCCL=org.springframework.boot.web.embedded.tomcat.TomcatEmbeddedWebappClassLoader@214b199c
            `---[0.715811ms] com.example.demo.controller.UserController:findUserById()
                +---[0.136949ms] org.slf4j.Logger:info() #16
                `---[0.015105ms] com.example.demo.model.User:<init>() #22
```
        
- trace javax.servlet.Filter *
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 15 , method count: 87) cost in 779 ms, listenerId: 7
        `---ts=2021-12-10 00:30:03;thread_name=http-nio-8080-exec-4;id=28;is_daemon=true;priority=5;TCCL=org.springframework.boot.web.embedded.tomcat.TomcatEmbeddedWebappClassLoader@214b199c
            `---[7.222187ms] org.springframework.web.filter.OncePerRequestFilter:doFilter()
                +---[0.163155ms] org.springframework.web.filter.OncePerRequestFilter:getAlreadyFilteredAttributeName() #97
                |   `---[0.138661ms] org.springframework.web.filter.OncePerRequestFilter:getAlreadyFilteredAttributeName()
                |       `---[0.103349ms] org.springframework.web.filter.OncePerRequestFilter:getFilterName() #172
                |           `---[0.062593ms] org.springframework.web.filter.GenericFilterBean:getFilterName()
                |               `---[0.014955ms] javax.servlet.FilterConfig:getFilterName() #298
                +---[0.017961ms] javax.servlet.ServletRequest:getAttribute() #98
                +---[0.118082ms] org.springframework.web.filter.OncePerRequestFilter:skipDispatch() #100
                |   `---[0.097038ms] org.springframework.web.filter.OncePerRequestFilter:skipDispatch()
                |       +---[0.070357ms] org.springframework.web.filter.OncePerRequestFilter:isAsyncDispatch() #129
                |       |   `---[0.04831ms] org.springframework.web.filter.OncePerRequestFilter:isAsyncDispatch()
                |       |       +---[0.010064ms] javax.servlet.http.HttpServletRequest:getDispatcherType() #148
                |       |       `---[0.007452ms] javax.servlet.DispatcherType:equals() #148
                |       `---[0.008342ms] javax.servlet.http.HttpServletRequest:getAttribute() #132
                +---[0.025445ms] org.springframework.web.filter.OncePerRequestFilter:shouldNotFilter() #100
                |   `---[0.00328ms] org.springframework.web.filter.OncePerRequestFilter:shouldNotFilter()
                +---[0.013307ms] javax.servlet.ServletRequest:setAttribute() #117
                +---[5.926056ms] org.springframework.web.filter.OncePerRequestFilter:doFilterInternal() #119
                |   `---[5.883628ms] org.springframework.web.filter.CharacterEncodingFilter:doFilterInternal()
                |       +---[0.029643ms] org.springframework.web.filter.CharacterEncodingFilter:getEncoding() #192
                |       |   `---[0.010081ms] org.springframework.web.filter.CharacterEncodingFilter:getEncoding()
                |       +---[0.033267ms] org.springframework.web.filter.CharacterEncodingFilter:isForceRequestEncoding() #194
                |       |   `---[0.009699ms] org.springframework.web.filter.CharacterEncodingFilter:isForceRequestEncoding()
                |       +---[0.018291ms] javax.servlet.http.HttpServletRequest:setCharacterEncoding() #195
                |       +---[0.017065ms] org.springframework.web.filter.CharacterEncodingFilter:isForceResponseEncoding() #197
                |       |   `---[0.004087ms] org.springframework.web.filter.CharacterEncodingFilter:isForceResponseEncoding()
                |       `---[5.208667ms] javax.servlet.FilterChain:doFilter() #201
                |           `---[5.185514ms] org.springframework.web.filter.OncePerRequestFilter:doFilter()
                |               +---[0.035588ms] org.springframework.web.filter.OncePerRequestFilter:getAlreadyFilteredAttributeName() #97
                |               |   `---[0.027696ms] org.springframework.web.filter.OncePerRequestFilter:getAlreadyFilteredAttributeName()
                |               |       `---[0.019775ms] org.springframework.web.filter.OncePerRequestFilter:getFilterName() #172
                |               |           `---[0.011432ms] org.springframework.web.filter.GenericFilterBean:getFilterName()
                |               |               `---[0.003942ms] javax.servlet.FilterConfig:getFilterName() #298
                |               +---[0.005286ms] javax.servlet.ServletRequest:getAttribute() #98
                |               +---[0.058232ms] org.springframework.web.filter.OncePerRequestFilter:skipDispatch() #100
                |               |   `---[0.050437ms] org.springframework.web.filter.OncePerRequestFilter:skipDispatch()
                |               |       +---[0.032741ms] org.springframework.web.filter.OncePerRequestFilter:isAsyncDispatch() #129
                |               |       |   `---[0.024426ms] org.springframework.web.filter.OncePerRequestFilter:isAsyncDispatch()
                |               |       |       +---[0.003501ms] javax.servlet.http.HttpServletRequest:getDispatcherType() #148
                |               |       |       `---[0.008479ms] javax.servlet.DispatcherType:equals() #148
                |               |       `---[0.004692ms] javax.servlet.http.HttpServletRequest:getAttribute() #132
                |               +---[0.014375ms] org.springframework.web.filter.OncePerRequestFilter:shouldNotFilter() #100
                |               |   `---[0.00291ms] org.springframework.web.filter.OncePerRequestFilter:shouldNotFilter()
                |               +---[0.00623ms] javax.servlet.ServletRequest:setAttribute() #117
                |               +---[5.010865ms] org.springframework.web.filter.OncePerRequestFilter:doFilterInternal() #119
                |               |   `---[4.984638ms] org.springframework.web.filter.FormContentFilter:doFilterInternal()
                |               |       +---[0.17726ms] org.springframework.web.filter.FormContentFilter:parseIfNecessary() #88
                |               |       |   `---[0.158322ms] org.springframework.web.filter.FormContentFilter:parseIfNecessary()
                |               |       |       `---[0.148897ms] org.springframework.web.filter.FormContentFilter:shouldParse() #99
                |               |       |           `---[0.132044ms] org.springframework.web.filter.FormContentFilter:shouldParse()
                |               |       |               +---[0.014649ms] javax.servlet.http.HttpServletRequest:getContentType() #113
                |               |       |               +---[0.070001ms] javax.servlet.http.HttpServletRequest:getMethod() #114
                |               |       |               `---[0.009539ms] org.springframework.util.StringUtils:hasLength() #115
                |               |       +---[0.012539ms] org.springframework.util.CollectionUtils:isEmpty() #89
                |               |       `---[4.767776ms] javax.servlet.FilterChain:doFilter() #93
                |               |           `---[4.743202ms] org.springframework.web.filter.OncePerRequestFilter:doFilter()
                |               |               +---[0.046163ms] org.springframework.web.filter.OncePerRequestFilter:getAlreadyFilteredAttributeName() #97
                |               |               |   `---[0.038273ms] org.springframework.web.filter.OncePerRequestFilter:getAlreadyFilteredAttributeName()
                |               |               |       `---[0.021492ms] org.springframework.web.filter.OncePerRequestFilter:getFilterName() #172
                |               |               |           `---[0.013515ms] org.springframework.web.filter.GenericFilterBean:getFilterName()
                |               |               |               `---[0.006358ms] javax.servlet.FilterConfig:getFilterName() #298
                |               |               +---[0.005214ms] javax.servlet.ServletRequest:getAttribute() #98
                |               |               +---[0.07771ms] org.springframework.web.filter.OncePerRequestFilter:skipDispatch() #100
                |               |               |   `---[0.066453ms] org.springframework.web.filter.OncePerRequestFilter:skipDispatch()
                |               |               |       +---[0.035689ms] org.springframework.web.filter.OncePerRequestFilter:isAsyncDispatch() #129
                |               |               |       |   `---[0.022074ms] org.springframework.web.filter.OncePerRequestFilter:isAsyncDispatch()
                |               |               |       |       +---[0.003305ms] javax.servlet.http.HttpServletRequest:getDispatcherType() #148
                |               |               |       |       `---[0.003099ms] javax.servlet.DispatcherType:equals() #148
                |               |               |       `---[0.010847ms] javax.servlet.http.HttpServletRequest:getAttribute() #132
                |               |               +---[0.01408ms] org.springframework.web.filter.OncePerRequestFilter:shouldNotFilter() #100
                |               |               |   `---[0.006064ms] org.springframework.web.filter.OncePerRequestFilter:shouldNotFilter()
                |               |               +---[0.0093ms] javax.servlet.ServletRequest:setAttribute() #117
                |               |               +---[4.507961ms] org.springframework.web.filter.OncePerRequestFilter:doFilterInternal() #119
                |               |               |   `---[4.472643ms] org.springframework.web.filter.RequestContextFilter:doFilterInternal()
                |               |               |       +---[0.02315ms] org.springframework.web.context.request.ServletRequestAttributes:<init>() #96
                |               |               |       +---[0.204234ms] org.springframework.web.filter.RequestContextFilter:initContextHolders() #97
                |               |               |       |   `---[0.135717ms] org.springframework.web.filter.RequestContextFilter:initContextHolders()
                |               |               |       |       +---[0.021615ms] javax.servlet.http.HttpServletRequest:getLocale() #112
                |               |               |       |       +---[0.024175ms] org.springframework.context.i18n.LocaleContextHolder:setLocale() #112
                |               |               |       |       +---[0.020328ms] org.springframework.web.context.request.RequestContextHolder:setRequestAttributes() #113
                |               |               |       |       `---[0.01158ms] org.apache.commons.logging.Log:isTraceEnabled() #114
                |               |               |       +---[2.813907ms] javax.servlet.FilterChain:doFilter() #100
                |               |               |       |   `---[2.774797ms] org.apache.tomcat.websocket.server.WsFilter:doFilter()
                |               |               |       |       +---[0.012406ms] org.apache.tomcat.websocket.server.WsServerContainer:areEndpointsRegistered() #51
                |               |               |       |       `---[2.714243ms] javax.servlet.FilterChain:doFilter() #53
                |               |               |       +---[0.738443ms] org.springframework.web.filter.RequestContextFilter:resetContextHolders() #103
                |               |               |       |   `---[0.717705ms] org.springframework.web.filter.RequestContextFilter:resetContextHolders()
                |               |               |       |       +---[0.017395ms] org.springframework.context.i18n.LocaleContextHolder:resetLocaleContext() #120
                |               |               |       |       `---[0.678822ms] org.springframework.web.context.request.RequestContextHolder:resetRequestAttributes() #121
                |               |               |       +---[0.005241ms] org.apache.commons.logging.Log:isTraceEnabled() #104
                |               |               |       `---[0.013931ms] org.springframework.web.context.request.ServletRequestAttributes:requestCompleted() #107
                |               |               `---[0.019318ms] javax.servlet.ServletRequest:removeAttribute() #123
                |               `---[0.010271ms] javax.servlet.ServletRequest:removeAttribute() #123
                `---[0.006625ms] javax.servlet.ServletRequest:removeAttribute() #123
```
        
- trace *MathGame *
        
```jsx
        Press Q or Ctrl+C to abort.
        Affect(class count: 1 , method count: 5) cost in 111 ms, listenerId: 8
        `---ts=2021-12-10 00:36:11;thread_name=http-nio-8080-exec-1;id=24;is_daemon=true;priority=5;TCCL=org.springframework.boot.web.embedded.tomcat.TomcatEmbeddedWebappClassLoader@214b199c
            `---[1008.769205ms] com.example.demo.service.MathGame:run()
                `---[1007.569354ms] com.example.demo.service.MathGame:primeFactors() #38 [throws Exception]
                    `---[1007.376576ms] com.example.demo.service.MathGame:primeFactors() [throws Exception]
                        +---[1.313242ms] org.slf4j.Logger:info() #58
                        +---[1000.823069ms] com.example.demo.util.ThreadUtil:sleep() #59
                        `---throw:java.lang.IllegalArgumentException #62 [number is: -189292, need >= 2]
        
        `---ts=2021-12-10 00:36:13;thread_name=http-nio-8080-exec-1;id=24;is_daemon=true;priority=5;TCCL=org.springframework.boot.web.embedded.tomcat.TomcatEmbeddedWebappClassLoader@214b199c
            `---[1005.905484ms] com.example.demo.service.MathGame:run()
                +---[1004.844774ms] com.example.demo.service.MathGame:primeFactors() #38
                |   `---[1004.755121ms] com.example.demo.service.MathGame:primeFactors()
                |       +---[0.122696ms] org.slf4j.Logger:info() #58
                |       `---[1004.226247ms] com.example.demo.util.ThreadUtil:sleep() #59
                `---[0.905459ms] com.example.demo.service.MathGame:print() #39
                    `---[0.652883ms] com.example.demo.service.MathGame:print()
        
        `---ts=2021-12-10 00:36:15;thread_name=http-nio-8080-exec-1;id=24;is_daemon=true;priority=5;TCCL=org.springframework.boot.web.embedded.tomcat.TomcatEmbeddedWebappClassLoader@214b199c
            `---[1001.827702ms] com.example.demo.service.MathGame:run()
                +---[1001.576307ms] com.example.demo.service.MathGame:primeFactors() #38
                |   `---[1001.361225ms] com.example.demo.service.MathGame:primeFactors()
                |       +---[0.116614ms] org.slf4j.Logger:info() #58
                |       `---[1001.119712ms] com.example.demo.util.ThreadUtil:sleep() #59
                `---[0.144569ms] com.example.demo.service.MathGame:print() #39
                    `---[0.097149ms] com.example.demo.service.MathGame:print()
        
        `---ts=2021-12-10 00:36:17;thread_name=http-nio-8080-exec-1;id=24;is_daemon=true;priority=5;TCCL=org.springframework.boot.web.embedded.tomcat.TomcatEmbeddedWebappClassLoader@214b199c
            `---[1001.761043ms] com.example.demo.service.MathGame:run()
                `---[1001.529768ms] com.example.demo.service.MathGame:primeFactors() #38 [throws Exception]
                    `---[1001.467385ms] com.example.demo.service.MathGame:primeFactors() [throws Exception]
                        +---[0.278224ms] org.slf4j.Logger:info() #58
                        +---[1000.3417ms] com.example.demo.util.ThreadUtil:sleep() #59
                        `---throw:java.lang.IllegalArgumentException #62 [number is: -173095, need >= 2]
        
        `---ts=2021-12-10 00:36:19;thread_name=http-nio-8080-exec-1;id=24;is_daemon=true;priority=5;TCCL=org.springframework.boot.web.embedded.tomcat.TomcatEmbeddedWebappClassLoader@214b199c
            `---[1001.378485ms] com.example.demo.service.MathGame:run()
                +---[1000.92388ms] com.example.demo.service.MathGame:primeFactors() #38
                |   `---[1000.8895ms] com.example.demo.service.MathGame:primeFactors()
                |       +---[0.166392ms] org.slf4j.Logger:info() #58
                |       `---[1000.520228ms] com.example.demo.util.ThreadUtil:sleep() #59
                `---[0.239913ms] com.example.demo.service.MathGame:print() #39
                    `---[0.099868ms] com.example.demo.service.MathGame:print()
```
        
    
# SC
    
查看JVM已加载的类信息
    
- sc -d -f com.example.demo.controller.CountryController
        
        ```jsx
        class-info        com.example.demo.controller.CountryController
         code-source       file:/Users/ezreal/temp/demo/launch-admin/target/launch-admin-0.0.1-SNAPSHOT.jar!/BOOT-INF/classes!/
         name              com.example.demo.controller.CountryController
         isInterface       false
         isAnnotation      false
         isEnum            false
         isAnonymousClass  false
         isArray           false
         isLocalClass      false
         isMemberClass     false
         isPrimitive       false
         isSynthetic       false
         simple-name       CountryController
         modifier          public
         annotation        org.springframework.web.bind.annotation.RestController
         interfaces
         super-class       +-java.lang.Object
         class-loader      +-org.springframework.boot.loader.LaunchedURLClassLoader@6267c3bb
                             +-sun.misc.Launcher$AppClassLoader@55f96302
                               +-sun.misc.Launcher$ExtClassLoader@bebdb06
         classLoaderHash   6267c3bb
         fields            name     logger
                           type     org.slf4j.Logger
                           modifier final,private,static
                           value    Logger[com.example.demo.controller.CountryController]
        
                           name       countryService
                           type       com.example.demo.service.CountryService
                           modifier   private
                           annotation javax.annotation.Resource
        
        Affect(row-cnt:1) cost in 13 ms.
        ```
        
    
# sm
    
查看已加载类的方法信息
    
- sm -d com.example.demo.controller.CountryController
        
        ```jsx
        declaring-class   com.example.demo.controller.CountryController
         constructor-name  <init>
         modifier          public
         annotation
         parameters
         exceptions
         classLoaderHash   6267c3bb
        
         declaring-class  com.example.demo.controller.CountryController
         method-name      initFarm
         modifier         public
         annotation       org.springframework.web.bind.annotation.GetMapping,org.springframework.web.bind.annotation.ResponseBody
         parameters       java.lang.String
                          java.lang.String
         return           java.lang.String
         exceptions
         classLoaderHash  6267c3bb
        
        Affect(row-cnt:2) cost in 16 ms.
        ```
        
    
# jad
    
反编译指定已加载类的源码
    
- jad --source-only com.example.demo.controller.UserController
    
# redefine
    
- 加载外部的`.class`文件，retransform jvm已加载的类。

# 热部署

1. 反编译错误的类

```jsx
jad --source-only com.example.demo.controller.UserController > /Users/ezreal/temp/UserController.java
```

1. 修改错误的位置
2. 获取加载该类的classLoader

```jsx
sc -d *UserController | grep classLoaderHash
classLoaderHash   6267c3bb
```

1. 用该classLoader 对该类编译

```jsx
mc -c 6267c3bb /Users/ezreal/temp/UserController.java -d /Users/ezreal/temp

```

1. 对该类进行热部署

```jsx
redefine /Users/ezreal/temp/com/example/demo/controller/UserController.class
```

## 后续问题

1. 热部署后，静态变量会不会被修改
2. 流量回放后，如果有调用数据库操作会不会执行

# demo应用

[https://github.com/EzrealOf/arthas-demo](https://github.com/EzrealOf/arthas-demo)

# 参考资料

[https://arthas.aliyun.com/doc/](https://arthas.aliyun.com/doc/classloader.html)

[https://hengyun.tech/tags/arthas/](https://hengyun.tech/tags/arthas/)