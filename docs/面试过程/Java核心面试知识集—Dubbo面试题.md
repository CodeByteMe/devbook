# Contanct Me

如果觉得看起来比较麻烦，需要PDF版本，或是需要更多学习资料，都可以加上QQ群领取

>本群由我创立，目前已将群主权限交由合作方便于进行日常管理，介意的朋友们在GitHub上看最新版就好了
>
>> 这份笔记资料是会免费提供的，特地向你们保证…毕竟还是要恰饭的嘛…

祝愿每一位有追求的Java开发同胞都能进大厂拿高薪！

## QQ群

Java架构交流QQ群：**578486082** （备注一下GitHub，免得被认成打无良广告的）

快捷加群方式：[点击此处加入群聊：java高级程序猿①](https://jq.qq.com/?_wv=1027&k=oE5kCnMu)

![image.png](https://upload-images.jianshu.io/upload_images/24613101-931262091ba7ed2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> PS：
>
> > 平常很忙，找miffy小姐姐领取就好了，免费获取的！

![image.png](https://upload-images.jianshu.io/upload_images/24613101-4b0507ab7ef34106.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1、为什么要用Dubbo？

随着服务化的进一步发展，服务越来越多，服务之间的调用和依赖关系也越来越复杂，诞生了面向服务的架构体系(SOA)，

也因此衍生出了一系列相应的技术，如对服务提供、服务调用、连接处理、通信协议、序列化方式、服务发现、服务路由、日志输出等行为进行封装的服务框架。

就这样为分布式系统的服务治理框架就出现了，Dubbo也就这样产生了。

## 2、Dubbo 的整体架构设计有哪些分层?

接口服务层（Service）：该层与业务逻辑相关，根据 provider 和 consumer 的业务设计对应的接口和实现

配置层（Config）：对外配置接口，以 ServiceConfig 和 ReferenceConfig 为中心

服务代理层（Proxy）：服务接口透明代理，生成服务的客户端 Stub 和 服务端的 Skeleton，以 ServiceProxy 为中心，扩展接口为 ProxyFactory

服务注册层（Registry）：封装服务地址的注册和发现，以服务 URL 为中心，扩展接口为 RegistryFactory、Registry、RegistryService

路由层（Cluster）：封装多个提供者的路由和负载均衡，并桥接注册中心，以Invoker 为中心，扩展接口为 Cluster、Directory、Router和LoadBlancce

监控层（Monitor）：RPC调用次数和调用时间监控，以 Statistics 为中心，扩展接口为 MonitorFactory、Monitor和MonitorService

远程调用层（Protocal）：封装 RPC 调用，以 Invocation 和 Result 为中心，扩展接口为 Protocal、Invoker和Exporter

信息交换层（Exchange）：封装请求响应模式，同步转异步。以 Request 和 Response 为中心，扩展接口为 Exchanger、ExchangeChannel、ExchangeClient和ExchangeServer

网络传输层（Transport）：抽象 mina 和 netty 为统一接口，以 Message 为中心，扩展接口为Channel、Transporter、Client、Server和Codec

数据序列化层（Serialize）：可复用的一些工具，扩展接口为Serialization、 ObjectInput、ObjectOutput和ThreadPool

## 3、默认使用的是什么通信框架，还有别的选择吗?

默认也推荐使用netty框架，还有mina。

## 4、服务调用是阻塞的吗？

默认是阻塞的，可以异步调用，没有返回值的可以这么做。

Dubbo 是基于 NIO 的非阻塞实现并行调用，客户端不需要启动多线程即可完成并行调用多个远程服务，相对多线程开销较小，异步调用会返回一个 Future 对象。

## 5、一般使用什么注册中心？还有别的选择吗？

推荐使用 Zookeeper 作为注册中心，还有 Redis、Multicast、Simple 注册中心，但不推荐。

## 6、默认使用什么序列化框架，你知道的还有哪些？

推荐使用Hessian序列化，还有Duddo、FastJson、Java自带序列化。

## 7、服务提供者能实现失效踢出是什么原理？

服务失效踢出基于zookeeper的临时节点原理。

## 8、服务上线怎么不影响旧版本？

采用多版本开发，不影响旧版本。

## 9、如何解决服务调用链过长的问题？

可以结合zipkin实现分布式服务追踪。

## 10、说说核心的配置有哪些？

| 配置 | 配置说明 |

| --- | --- |

| dubbo:service | 服务配置 |

| dubbo:reference | 引用配置 |

| dubbo:protocol | 协议配置 |

| dubbo:application | 应用配置 |

| dubbo:module | 模块配置 |

| dubbo:registry | 注册中心配置 |

| dubbo:monitor | 监控中心配置 |

| dubbo:provider | 提供方配置 |

| dubbo:consumer | 消费方配置 |

| dubbo:method | 方法配置 |

| dubbo:argument | 参数配置 |

## 11、Dubbo 推荐用什么协议？

· dubbo://（推荐）

· rmi://

· hessian://

· http://

· webservice://

· thrift://

· memcached://

· redis://

· rest://

## 12、同一个服务多个注册的情况下可以直连某一个服务吗？

可以点对点直连，修改配置即可，也可以通过telnet直接某个服务。

## 13、画一画服务注册与发现的流程图？

![image.png](https://upload-images.jianshu.io/upload_images/11474088-be34bef59137b1aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 14、Dubbo 集群容错有几种方案？

| 集群容错方案 | 说明 |

| --- | --- |

| Failover Cluster | 失败自动切换，自动重试其它服务器（默认） |

| Failfast Cluster | 快速失败，立即报错，只发起一次调用 |

| Failsafe Cluster | 失败安全，出现异常时，直接忽略 |

| Failback Cluster | 失败自动恢复，记录失败请求，定时重发 |

| Forking Cluster | 并行调用多个服务器，只要一个成功即返回 |

| Broadcast Cluster | 广播逐个调用所有提供者，任意一个报错则报错 |

## 15、Dubbo 服务降级，失败重试怎么做？
    服务降级：通常来讲为了保证服务可用性，如果服务A调用服务B一直不可用，那么可以将服务B的服务进行降级处理，自定义Moc，在自定义Moc类中实现降级逻辑


## 16、Dubbo 使用过程中都遇到了些什么问题？
    序列化失败的问题：因为默认hessian2序列化，当使用jsonlib框架的时候，jsonlib采用的是自身序列化方式，当json中属性存在null值时，jsonlib会创建一个JSONNull对象，此时dubbo没办法反序列化会抛出异常。【替换json框架即可】。
    
    子类字段属性值丢失的问题：因为默认hessian2序列化 当父类与子类有相同属性的时候，JavaDeserializer解析filed会进行去重，对于子类字段会被赋两次值，导致子类丢失。 【去掉重复属性即可】。
    
    服务无法调用的问题：服务不可用或者版本号不一致导致的。
    
    dubbo服务自检问题：如果服务未启动，会尝试自检服务，会一直不停抛出异常。
    
    dubbo 调用时 Data length too large的问题：因为dubbo默认请求报文长度是8M，【减少报文长度即可】。
    
    


## 17、Dubbo Monitor 实现原理？(监控中心)
    通过实现MonitorService ：1.实现收集请求链路数据汇总，并将数据持久化本地；
                                1).通过ScheduledExecutorService 定时任务调度线程将监控数据写入饼图定时任务。
                                2).创建有界阻塞队列LinkedBlockingQueue。
                                3).创建持久化监控数据线程，从阻塞队列中获取数据，并写入文件中。
                                4).开启定时调度任务，将持久化数据写入饼图。
                                5).将饼图与持久化数据放在同一目录
                            2.再实现监控报表生成逻辑



## 18、Dubbo 用到哪些设计模式？
    工厂模式：因为dubbo可以支持多种传输协议，默认dubbo;
    动态代理模式：通过获取url中的参数key，动态生成代理类。
    观察者模式：dubbo提供者服务会与注册中心交互，先注册自己服务，再订阅自己的服务，若服务有更新，则向provider发送notify消息。



## 19、Dubbo 配置文件是如何加载到Spring中的？ 
    spring 在初始化容器的时候，会扫描xml配置文件，并读取默认的schema一级Dubbo自定义的schema,每个schema都会对应一个自己的NamespaceHandler，NamespaceHandler里面通过BeanDefinitionParser来解析配置信息并转化为需要加载的bean对象



## 20、Dubbo SPI 和 Java SPI 区别？
       spi又称：服务发现机制
       java spi：jdk内置的一种服务发现机制，简单实现循环发现的功能，并没有命名，导致使用过程中很难准确引用，不支持自动注入和装配，不提供类似AOP功能，很难与其他框架集成。
       dubbo spi:是dubbo基于spi的机制，进行自行实现的服务发现机制，增加对dubbo的拓展，不需要改动源码。代码侵入几乎为0.符合开闭原则，并且支持IOC,AOP高级功能，也支持第三方IOC容器。
    

## 21、Dubbo 支持分布式事务吗？
    目前暂时不支持，可与通过 tcc-transaction 框架实现或其他方式实现


## 22、Dubbo 可以对结果进行缓存吗？
    可以的，在dubbo 的refrence标签中增加cache=true即可。


## 23、服务上线怎么兼容旧版本？
    通过保留多版本进行服务过度，不同版本之间服务是互不引用。



## 24、Dubbo必须依赖的包有哪些？
    JDK自身，不需要依赖其他第三方包


## 25、Dubbo telnet 命令能做什么？
    用于dubbo服务接口调用测试


## 26、Dubbo 如何优雅停机？
    通过JDK的ShutdownHook 钩子程序，来完成优雅停机的。


## 27、Dubbo 和 Dubbox 之间的区别？
    dubbox 增加了restfull风格调用。升级了spring和zk客户端的版本，增加jackson的json的序列化方式，增加java高级序列化实现。



## 28、Dubbo 和 Spring Cloud 的区别？
   
    dubbo在功能上与spring cloud非常类似。从流程上讲，dubbo是服务自动发现通过zk进行注册服务，是一种长连接的NIO 通信框架。
    cloud 虽然也提供了服务注册与发现的功能，但是更多的是cloud还提供了其他优秀的组件，譬如网关（Zuul）,断路器（Hystrix）
    
    cloud 在某一程度上有一定的侵入性，服务配置需要通过注解来完成,而dubbo程序入侵少。
    
    Dubbo 使用的是 RPC 通信，⼆进制传输，占⽤带宽⼩；
    Spring Cloud 使⽤的是 HTTP RESTFul ⽅式
    
    



