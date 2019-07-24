## SpringCloud

### SpringCLoud 微服务架构

- 集群
  - 高性能：通过多台计算机完成同一个工作，分担压力，提高效率
  - 高可用：两机或者多机工作内容、过程完全一致，可以相互顶替
- 分布式
  - 低耦合：便于扩展，提高资源利用率
  - 高吞吐：功能拆分，分散到不同的模块执行
- 集群和分布式是可以并存的，两者并不冲突
- CAP 理论 ：强一致性（C）、极致可用性（A）、分区容错性（P）
  - 分区容错性（P）:指的是分布式系统中子网络（分区）通信失败的情况
  - 这三个指标无法同时满足

- 集群与分布式的区别之一

  - 集群是这一组服务起当中的任何一个服务器都可以单独完成所指派的任务
  - 分布式是这一组服务起需要协调合作才能完成所指派的任务



  - Ribbon

    > 处理一个服务多实例下的负载均衡

      - 当需要对集群的调用做负载均衡的时候，SpringCloud提供过了组件 Ribbon；

- Hystrix

  > 熔断和限流（可以避免系统长时间无响应 ）

  - 服务之间的调用会有阻塞，但是还在发送请求的时候，这个组件就可以处理这种情况。

网关：
>  API 网关也是需要注册到注册中心

- 功能：统一对外的入口，请求不再访问指定的服务，而是统一从网关访问

- SpringCloud 组件

![SpringCloud 组件](https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190724090410.png)



- SpringCloud 常用组件

  - 服务处理：SpringCloud Eureka
- 负载均衡：SpringCloud Ribbon
  - 熔断限流：SpringCloud Hystrix
- 服务调用：SpringCloud Feign
  - 网关服务：SpringCloud Zuul

    - 网关服务：Spring Cloud Gateway
- 配置中心：SpringCloud Config
  - 消息总线：SpringCloud Bus
- 消息驱动：SpringCloud Stream
  - 服务追踪：SpringCloud Sleuth



### SpringCLoud 网关

1、网关应用场景：

- 统一外部入口
- 请求路由
- 认证授权
- 请求限流
- 请求日志和监控

2、SpringCloud Zuul

- 服务路由
- 自定义过滤器：ZuulFilter
  - filterType()：过滤类型
  - filterOrder()：过滤顺序
  - shouldFilter()：是否需要过滤
  - run()：过滤逻辑
- SpringCLoud Zuul 2.X 之后不再继续维护
- SpringCloud Gateway 是由 SpringCLoud 团队在Zuul 2.X 上进行的扩展
  - 两者底层实现基本一致，Gateway 比 Zuul 稍微强大一点
  - 两者可以直接替换



### SpringCLoud 服务发现

- SpringCloud Eureka
  - Eureka Client ：服务注册
  - Eureka Server：服务发现
- SpringCloud Eureka & Consul
  - Consul：保证强一致性
  - Eureka：保证高可用



### SpringCLoud 其他组件







