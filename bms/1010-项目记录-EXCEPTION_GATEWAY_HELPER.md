## 项目记录 - EXCEPTION_GATEWAY_HELPER

- 错误描述：调用接口的时候，会返回错误信息

~~~java
<error>
  <status>EXCEPTION_GATEWAY_HELPER</status>
  <code>error.gatewayHelper.exception</code>
  <message>gateway helper error happened: java.lang.IllegalStateException: No instances available for oauth-server</message>
</error>
~~~

- 详细描述如下：

~~~java
2019-10-10 19:01:27.166  INFO 15732 --- [ XNIO-2 task-18] i.c.g.h.api.filter.RootServletFilter     : Check permission error

java.lang.IllegalStateException: No instances available for oauth-server
	at org.springframework.cloud.netflix.ribbon.RibbonLoadBalancerClient.execute(RibbonLoadBalancerClient.java:89) ~[spring-cloud-netflix-ribbon-2.0.2.RELEASE.jar:2.0.2.RELEASE]
	at org.springframework.cloud.client.loadbalancer.LoadBalancerInterceptor.intercept(LoadBalancerInterceptor.java:55) ~[spring-cloud-commons-2.0.2.RELEASE.jar:2.0.2.RELEASE]
	at org.springframework.http.client.InterceptingClientHttpRequest$InterceptingRequestExecution.execute(InterceptingClientHttpRequest.java:92) ~[spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.http.client.InterceptingClientHttpRequest.executeInternal(InterceptingClientHttpRequest.java:76) ~[spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.http.client.AbstractBufferingClientHttpRequest.executeInternal(AbstractBufferingClientHttpRequest.java:48) ~[spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.http.client.AbstractClientHttpRequest.execute(AbstractClientHttpRequest.java:53) ~[spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.web.client.RestTemplate.doExecute(RestTemplate.java:687) ~[spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.web.client.RestTemplate.execute(RestTemplate.java:644) ~[spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.web.client.RestTemplate.exchange(RestTemplate.java:564) ~[spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at io.choerodon.gateway.helper.api.service.impl.GetUserDetailsServiceImpl.getUserDetails(GetUserDetailsServiceImpl.java:60) ~[classes/:na]
	at io.choerodon.gateway.helper.api.filter.GetUserDetailsFilter.run(GetUserDetailsFilter.java:45) ~[classes/:na]
	at io.choerodon.gateway.helper.api.filter.RootServletFilter.doFilter(RootServletFilter.java:70) ~[classes/:na]
	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:99) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at org.springframework.web.filter.HttpPutFormContentFilter.doFilterInternal(HttpPutFormContentFilter.java:109) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at org.springframework.web.filter.HiddenHttpMethodFilter.doFilterInternal(HiddenHttpMethodFilter.java:93) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at org.springframework.boot.actuate.metrics.web.servlet.WebMvcMetricsFilter.filterAndRecordMetrics(WebMvcMetricsFilter.java:155) [spring-boot-actuator-2.0.6.RELEASE.jar:2.0.6.RELEASE]
	at org.springframework.boot.actuate.metrics.web.servlet.WebMvcMetricsFilter.filterAndRecordMetrics(WebMvcMetricsFilter.java:123) [spring-boot-actuator-2.0.6.RELEASE.jar:2.0.6.RELEASE]
	at org.springframework.boot.actuate.metrics.web.servlet.WebMvcMetricsFilter.doFilterInternal(WebMvcMetricsFilter.java:108) [spring-boot-actuator-2.0.6.RELEASE.jar:2.0.6.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:200) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-5.0.10.RELEASE.jar:5.0.10.RELEASE]
	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.FilterHandler.handleRequest(FilterHandler.java:84) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.security.ServletSecurityRoleHandler.handleRequest(ServletSecurityRoleHandler.java:62) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletChain$1.handleRequest(ServletChain.java:65) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletDispatchingHandler.handleRequest(ServletDispatchingHandler.java:36) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.security.SSLInformationAssociationHandler.handleRequest(SSLInformationAssociationHandler.java:132) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.security.ServletAuthenticationCallHandler.handleRequest(ServletAuthenticationCallHandler.java:57) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.security.handlers.AbstractConfidentialityHandler.handleRequest(AbstractConfidentialityHandler.java:46) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.security.ServletConfidentialityConstraintHandler.handleRequest(ServletConfidentialityConstraintHandler.java:64) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.security.handlers.AuthenticationMechanismsHandler.handleRequest(AuthenticationMechanismsHandler.java:60) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.security.CachedAuthenticatedSessionHandler.handleRequest(CachedAuthenticatedSessionHandler.java:77) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.security.handlers.AbstractSecurityContextAssociationHandler.handleRequest(AbstractSecurityContextAssociationHandler.java:43) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletInitialHandler.handleFirstRequest(ServletInitialHandler.java:292) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletInitialHandler.access$100(ServletInitialHandler.java:81) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:138) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:135) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.core.ServletRequestContextThreadSetupAction$1.call(ServletRequestContextThreadSetupAction.java:48) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.core.ContextClassLoaderSetupAction$1.call(ContextClassLoaderSetupAction.java:43) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletInitialHandler.dispatchRequest(ServletInitialHandler.java:272) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletInitialHandler.access$000(ServletInitialHandler.java:81) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.servlet.handlers.ServletInitialHandler$1.handleRequest(ServletInitialHandler.java:104) [undertow-servlet-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.server.Connectors.executeRootHandler(Connectors.java:336) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at io.undertow.server.HttpServerExchange$1.run(HttpServerExchange.java:830) [undertow-core-1.4.26.Final.jar:1.4.26.Final]
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) [na:1.8.0_191]
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) [na:1.8.0_191]
	at java.lang.Thread.run(Thread.java:748) [na:1.8.0_191]

~~~

- 如图所示：

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20191010190239.png"/></div>

- 解决：//TODO