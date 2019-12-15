	eureka负责服务注册和服务发现，为了高可用，一般需要多个eureka server相互注册，组成集群。Eureka Server的同步遵循着一个非常简单的原则：只要有一条边将节点连接，就可以进行信息传播与同步。
	eureka内部对于注册的service主要通过心跳来监控service是否已经挂掉，默认心跳时间是15s。这就意味着，当一个服务提供方挂掉以后，服务订阅者最长可能30s以后才发现。
	service启动连上eureka之后，会同步一份服务列表到本地缓存，服务注册有更新时，eureka会推送到每个service。
	eureka也会有一些策略防止由于某个服务所在网络的不稳定导致的所有服务心跳停止的雪崩现象。
	eureka自带web页面，在页面上能看到所有的服务注册情况 和 eureka集群状态。
	eureka支持服务自己主动下掉自己，请求service的下列地址，可以让服务从eureka上下掉自己，同时service进程也会自己停掉自己。
curl -H 'Accept:application/json' -X POST localhost:${management.port}/shutdown

1、Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中注册，这样Server中的服务注册表中将会存储所有可用的服务节点的信息；

2、Eureka Client是一个Java客户端，用于简化与Eureka Server交互，客户端同时具备一个内置的、使用轮询负载均衡算法的负载均衡器；

3、在应用启动后，将会向Eureka Server发送心跳（默认周期30s），如果Eureka Server在多个心跳周期没有收到某个节点的心跳，Eureka Server会从服务注册表中把这个服务节点删除（默认为90s）；

4、Eureka Server之间通过复制的方式完成数据的同步；

5、Eureka Client具有缓冲机制，如果Eureka Server全部宕机的情况，客户端依然可以利用缓存的信息消费其他服务API；

6、Eureka region可以理解为地理上的分区，没有具体大小的限制；

7、Eureka zone可以理解为region内具体的机房信息；

8、使用Jersey框架实现自身的Restful HTTP接口，peer之间同步与服务注册通过HTTP协议实现，定时任务（发送心跳、定时清理过期服务、节点同步等）通过JDK自带的Timer实现，内存缓存实现Google的guava实现；

9、当服务注册中心Server检测服务提供者宕机时，在服务中心将服务置为DOWN状态，并将该服务向其他订阅者发布，订阅者更新本地缓存信息；

10、当Eureka Server节点在短时间内丢失过多的客户端时，这个节点会进入自我保护模式，不再注销任何服务；

11、Eureka与Zookeeper区别：分布式系统中不可能同时满足C（一致性）、A（可用性）以及P（分区容错性）。P是分布式系统必须要保证的特性，对此Zookeeper保证的是CP，而Eureka保证的是AP。 

12、Zookeeper：zk会出现这样一种情况当master因为网络故障与其他节点事务联系时，其他节点会重新进行leader选举，问题在于，选举leader的时间太长，且选举期间整个zk集群是不能直接使用的。
