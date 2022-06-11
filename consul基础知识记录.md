#### consul基础知识记录

1.consul是什么，consul就是提供服务发现的工具，consul是分布式的、高可用、横向扩展的；

2.consul简单的描述图，consul服务发现模块在微服务中的作用。

![Image.png](http://xwyhhhh1.test.upcdn.net/4742055-b5c590e819912447.png)

3.consul的简介

- service discovery：服务费发现，consul通过DNS或者HTTP接口使服务注册和服务发现变的很容易，一些外部服务，例如saas提供的也可以一样注册。
- health checking：健康检测使consul可以快速的告警在集群中的操作。和服务发现的集成，可以防止服务转发到故障的服务上面。
- key/value storage：Key/Value存储，应用程序可以根据自己的需要使用Consul提供的Key/Value存储。 Consul提供了简单易用的HTTP接口，结合其他工具可以实现动态配置、功能标记、领袖选举等等功能。
- multi-datacenter：无需复杂的配置，即可支持任意数量的区域。

4.应用场景

+ 服务发现场景中consul作为注册中心，服务地址被注册到consul中以后，可以使用consul提供的dns、http接口查询，consul支持health check。
+ 服务隔离场景中consul支持以服务为单位设置访问策略，能同时支持经典的平台和新兴的平台，支持tls证书分发，service-to-service加密。
+ 服务配置场景中consul提供key-value数据存储功能，并且能将变动迅速地通知出去，通过工具consul-template可以更方便地实时渲染配置文件。