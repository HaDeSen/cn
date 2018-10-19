# 产品功能



## 负载均衡实例

### 负载均衡基本信息
所属VPC、子网、多可用区（AZ）等。负载均衡可以选择单AZ或者多AZ，但为了保障业务高可用、强烈建议选择多AZ。

### 弹性公网IP绑定/解绑
可自主选择是否绑定、解绑公网IP、自由切换，灵活搭建内网、公网负载均衡，适应不同场景下的业务需求；通过公网IP绑定的方式，可隐藏内部网络结构，提高系统安全性。

## 监听器

### 监听规则
监听的协议类型与端口号信息，目前支持TCP协议类型。用户在同一个负载均衡内可以定义多个监听器，不同监听器可以选择不同协议类型、或者选择相同协议类型但定义不同端口号。


### 空闲连接超时设置
针对一个监听规则的空闲连接超时设置。当连接空闲超时时，负载均衡会自动断开客户端与后端服务器之间的连接。

## 后端服务

### 服务规则
定义后端服务器可接收的协议类型与端口号。当后端服务关联虚拟服务器组时，如果虚拟服务器组中注册的实例（虚机或者容器）定义了端口，负载均衡使用实例定义的端口将报文转发给服务实例；如果虚拟服务器组中注册的实例（虚机或者容器）没有定义端口，负载均衡会采用后端服务定义的端口号将报文转发给服务实例。当后端服务关联高可用组时，高可用组内的服务实例没有定义端口信息，因此负载均衡会采用后端服务定义的端口号将报文转发给服务实例。

### 调度算法选择
支持加权轮询、加权最小连接、源IP三种调度算法。可根据自身业务需求选择合适的算法进行流量分配，例如后端主机性能差别不大的情况下可选择加权轮询算法，以均匀后端主机流量分配、提升对外服务能力；也可以选择源IP转发，将相同来源的客户端请求通过哈希表映射到后端同一台服务器。同时可根据后端服务器性能配置权重（加权的权重值通过向虚拟服务器组注册实例时设置），可设置1-100的整数，高配置的服务器可设置更高的权重，承载更多的访问流量，当后端服务关联高可用组时，高可用组内所有实例的权重相同。

### 会话保持设置
支持基于TCP连接的会话保持。会话保持缺省超时时间为1440s，为会话保持的最小保证时间，在此期间内、无论NLB以及后端服务如何弹性扩展，所有源、目的IP相同的报文会保证转发到同一个后端服务器。当会话保持时间超时后，报文不能保证转发到同一个后端服务器。

### 源IP透传
目前NLB缺省能够把源自客户端的源IP透传到后端服务器上，不需用户特殊干预与配置。

### 连接耗尽
连接耗尽（connection draining）是后端服务器优雅退出服务的一种方式。当一个服务器从“虚拟服务器组”或者高可用组（AG）中摘除时，开始启动连接耗尽计时器，此后只有已建立的TCP连接报文会继续向该服务器转发、直到连接耗尽时间超时为止，而新建立的TCP连接将不会向该服务器转发。

### 健康检查
负载均衡会定时检测后端服务器运行状况，可自定义检测频率与健康/不健康判断条件；一旦检测到服务器运行异常，则不会再将流量分配给这些异常的实例，直至检测到实例运行正常后再恢复流量转发，保证服务的可用性。

### 服务关联
每个后端服务当前可以关联一个虚拟服务器组或高可用组。

## 虚拟服务器组

### 注册/取消服务实例
用户可手动向虚拟服务器组添加或者删除服务实例。服务实例包括虚机和容器两种类型。

### 服务实例的端口定义
向虚拟服务器组注册服务实例时，如果定义了端口信息，负载均衡使用实例定义的端口将报文转发给服务实例；如果注册服务实例时没有定义端口，负载均衡会采用后端服务定义的端口号将报文转发给服务实例。

### 服务实例的权重定义
向虚拟服务器组注册服务实例时，如果定义了权重信息，负载均衡则按照后端服务定义的调度算法+实例定义的权重进行报文的加权转发。例如，虚拟服务器组有2个实例，权重分别为5和10，后端服务选择加权轮询，负载均衡就会按照1:2的比例把流量转发给两个服务实例。

### 关联AS（Auto Scaling ）
负载均衡的虚拟服务器组可以通过关联AS，以实现服务实例的自动弹性伸缩。不过AS的弹性伸缩只考虑可用区（AZ）级的资源分配调度，不考虑机房内（例如跨机架）的高可用，如跨机架粒度的高可用保障，请使用高可用组。

## 高可用组

高可用组功能不属于负载均衡，但可以与负载均衡的后端服务关联，高可用组具备弹性伸缩能力，可根据业务负载情况自调节后端服务主机数量，以提供跨AZ、机房内跨机架的细粒度高可用服务能力。

## 监控报警

### 可视化监控
网络负载均衡NLB控制台提供丰富的监控信息，包括出/入流量、连接数、新增连接数等信息，您可以随时查看服务运行状态。

### 自动报警
您可以根据监控项设置报警规则，当监控项达到设置的阈值时，系统会通过短信和邮件的方式向您发送报警信息。

## 相关参考

- [产品概述](../Product-Introduction/Overview.md)
- [产品规格](../Product-Introduction/Specification.md)
- [价格总览](../Pricing/Price-Overview.md)
- [创建实例](../Getting-Started/Create-Instance.md)
- [创建虚拟服务器组](../Operation-Guide/TargetGroup-Management.md)
- [配置侦听策略](../Operation-Guide/Listener-Management.md)
- [管理后端服务与查看服务实例健康状态](../Operation-Guide/Backend-Management.md)
- [查看监控信息](../Operation-Guide/Monitoring.md)

