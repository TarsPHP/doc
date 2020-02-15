
# TARS功能特性
TARS Features

## 多语言
TARS最早使用C++语言进行开发，随着业务的发展，逐步实现了C++/JAVA/PHP/NodeJS/Go的服务端和客户端。
TARS was developed using C++ initially, and accomplished C++/JAVA/PHP/NodeJS/Go servers and clients through the development.

也可以使用Python语言等作为客户端，进行服务的调用。
Tars could also be called as a service leveraging other programming language such as python as a client. 

更多语言，包括C#等，也在逐步的开发中。
Services and clients of others languages  including C# is under active development continually.

各语言均已支持基本的RPC能力，不同语言之间，使用统一的接口定义语言tars来约定协议。
Different languages achieve basic RPC capabilities through contract agreement with . 

同时，TARS会针对各种语言的不同特性, 开发相对差异化的功能。
At the same time, TARS will develop different functionality based on different language features.

高性能是各种语言开发时应该遵循的重要准则。使用目前支持的语言开发的服务, 均已经过大流量的线上验证, 符合业务的高并发与高可用要求。
High performance is the one of the main focus while developing .As for now, high throughput in production environment has been test through,and it aligned with our requirements of high concurrency and high availability.

## 敏捷研发
首先，使用TARS可以快速的构建微服务。
First of all, microservices could be built by TARS rapidly.

其次，由于TARS多语言的特性, 对团队技术栈的差异有很高的兼容性。
And  thanks to multi-language support by TARS, it has high compatibility between difference technology stack.

对于需要快速迭代开发的场景，开发者可以选择使用PHP搭建微服务；
For some scenarios requiring fast iteration, developers can choose PHP to build microservices easily.

而对于一些性能要求比较极致的场景, 开发者则可以搭建基于C++的微服务。
And for other scenarios requiring extreme performance, developers build micorservices based on C++.

最后, TARS在各个语言的实现中, 都配备了服务端和客户端的自动代码生成工具。
Last but not least, TARS is equipped with automatic code generation tools for server and client for different language implementation.

通过这个工具:
Using TARS:

* 服务调用者可以非常方便的根据协议生成调用代码，并像调用本地函数一样调用远程服务。
* Service caller can easily generate the calling code according to the protocol and calls remote service like a local function.

* 服务提供者可以很快的生成代码的骨架，并在其中进行快速的需求开发。
* Service providers can quickly generate the skeleton of the code and then develop inside it efficiently.

## 高可用
High availability

TARS本身提供了一系列的基础服务, 这些基础服务有效的保证了TARS的高可用。
TARS provides a series of basic services to ensure high availability effectively. 

这些基础服务包括:
These basic services includes:

* Tars-Registry: 主控服务，中心化存储所有服务的可用地址，分布式部署，db存储。
  Tars-Registry: Master Control Service,  stores all the available addresses of services centrally, distributed deployment, db storage.
* Tars-Node: 节点服务，负责监控机器上所有的tars服务, 并在发现异常后及时恢复服务。
* Tars-Node: Node services,in charge of monitoring all tars services on one machine and resuming  services in time when abnormalities happened;
* Tars-State: 监控服务，负责收集服务状态、机器状态、调用情况等监控信息, 通过设置不同的告警策略, 及时通知服务异常;
* Tars-State: Monitoring service,responsible of collecting monitoring information such as the service state, machine state and RPC call information, and noticing  service anomalies in time by setting different alarm policies.

除此之外，各语言为了保证高可用，都会对服务可用列表进行一定时间的缓存，保证主控有问题的时候, 可以使用本地可用服务列表。
In addition, in order to ensure high availability, each language will cache the available services list for a period of time. If there is some thing wrong within the Tars-Registry, service can fallback to the available services list locally stored.

部分语言也已经实现了如果遇到某个服务ip不可用的情况, 自动标记并剔除的功能。
Some languages ​​have implemented the function that automatically tagging and rejecting a service when its IP is unavailable.

## 高效运营
## Highly efficient operations

TARS在发布的时候，也提供了无损的选项。通过使用无损发布，可以实现类似于nginx reload的功能。
TARS provides non-destructive options similar to Nginx reload when shipping code.

由于各语言框架不同, 无损发布的实现的方式也各不相同。
Due to the different language frameworks, the implementation of non-destructive publishing is also different.

服务分批无损重启，即在可用时再对外提供服务，已经成为了大部分高可用发布的标准选择。
Batches of non-destructive service reboot which ensures services available all the time, is the standard choice for most high availability releases.

同时, 由于TARS是自带运维管理后台的, 因此它天然比单纯的RPC框架更强大一些。
In addition, TARS is more powerful than a simple RPC framework because of its operation platform. 

通过运维管理后台，使用者可以轻松完成如下工作：
* 查看可用服务ip列表。
* 对单台机器进行服务配置变更。
* 服务发布和版本回退。
* 查看调用上报的监控信息。
* 查看机器和服务上报的监控信息。
* 单个或批量进行服务配置。

Users can finish these works easily using operation platform:
* View available services IP list.
* Modify service configuration of one machine.
* Service release and version rollback.
* View monitor information reported by RPC calls.
* View monitor information reported by service and machine.
* Configure services single or batch .
