
# TARS功能特性
TARS Features

## 多语言
TARS最早使用C++语言进行开发，随着业务的发展，逐步实现了C++/JAVA/PHP/NodeJS/Go的服务端和客户端。
TARS was developed using C++ initially. Through the development, the C++ / JAVA / PHP / NodeJS / Go server and client were gradually achieved.
TARS was developed using C++ initially, and accomplished C++/JAVA/PHP/NodeJS/Go servers and clients through the development.

也可以使用Python语言等作为客户端，进行服务的调用。
Other languages client such as Python client also can make service calls. 
Tars could also be called as a service leveraging other programming language such as python as a client. 

更多语言，包括C#等，也在逐步的开发中。
More languages server and client are being gradually developed including C# etc.
Clients of others languages  including C# is under active development continually.

各语言均已支持基本的RPC能力，不同语言之间，使用统一的接口定义语言tars来约定协议。
Basic RPC functionality is supported by a variety of languages, and a unified interface description language —— tars —— could be used to make contract.
Different languages achieve basic RPC capabilities through contract agreement with . 

同时，TARS会针对各种语言的不同特性, 开发相对差异化的功能。
At the same time,relatively different functionality will be developed for different language features.

高性能是各种语言开发时应该遵循的重要准则。使用目前支持的语言开发的服务, 均已经过大流量的线上验证, 符合业务的高并发与高可用要求。
High-performance is a important guidelines when develop service and client with different languages. Services which developed using TARS support languages ​​have been verified online with high flow capacity, and meet the high concurrency and high availability requirements.
                    the first priority when developing services and clients.As for now, high QPS benchmarks.
## 敏捷研发
首先，使用TARS可以快速的构建微服务。
First,TARS can quickly build microservices.

其次，由于TARS多语言的特性, 对团队技术栈的差异有很高的兼容性。
The second,TARS has high compatibility of the differences technology stacks inside team because of its languages agnostic feature.

对于需要快速迭代开发的场景，开发者可以选择使用PHP搭建微服务；
For some scenarios require fast iteration, developers can choose PHP to build microservices quickly.

而对于一些性能要求比较极致的场景, 开发者则可以搭建基于C++的微服务。
For some scenarios require extreme performance, developer can build microservices using C++.

最后, TARS在各个语言的实现中, 都配备了服务端和客户端的自动代码生成工具。
Last, TARS is equipped with automatic code generation tools for server and client in the implementation of all languages.

通过这个工具:
Using this tool:

* 服务调用者可以非常方便的根据协议生成调用代码，并像调用本地函数一样调用远程服务。
* Service caller can easily generate the calling code according to the protocol and call the remote service like a local function.

* 服务提供者可以很快的生成代码的骨架，并在其中进行快速的需求开发。
* Service providers can quickly generate the skeleton of the code and then efficient develop inside it.

## 高可用
High availability

TARS本身提供了一系列的基础服务, 这些基础服务有效的保证了TARS的高可用。
TARS provides a series of basic services to effectively ensure the high availability. 

这些基础服务包括:
Main basic services of TARS:

* Tars-Registry: 主控服务，中心化存储所有服务的可用地址，分布式部署，db存储。
* Tars-Registry: Master Control Service, centrally stores the available addresses of all services, distributed deployment, db storage.

* Tars-Node: 节点服务，负责监控机器上所有的tars服务, 并在发现异常后及时恢复服务。
* Tars-Node: Node services, monitoring all tars services on machine and resume services in time when abnormalities happened;

* Tars-State: 监控服务，负责收集服务状态、机器状态、调用情况等监控信息, 通过设置不同的告警策略, 及时通知服务异常;
* Tars-State: Monitoring service, collecting monitoring information such as the service state, the machine state and the RPC call information, and notices service anomalies in time by setting different alarm policies.

除此之外，各语言为了保证高可用，都会对服务可用列表进行一定时间的缓存，保证主控有问题的时候, 可以使用本地可用服务列表。
In order to ensure high availability,each language will cache the available services list for a period of time. When there is a problem with the Tars-Registry, service can fallback to the available services list stored in local.

部分语言也已经实现了如果遇到某个服务ip不可用的情况, 自动标记并剔除的功能。
Some languages ​​have implemented the function that automatic tagging and rejection a service if its IP is unavailable.

## 高效运营
## High efficient operations

TARS在发布的时候，也提供了无损的选项。通过使用无损发布，可以实现类似于nginx reload的功能。
TARS provides non-destructive options when release.Service can achieved function similar to NGINX reload by using non-destructive release.

由于各语言框架不同, 无损发布的实现的方式也各不相同。
The implementation of non-destructive release are different because of the different between each language framework.

服务分批无损重启，即在可用时再对外提供服务，已经成为了大部分高可用发布的标准选择。
Service batch non-destructive restart ———— provides services when services available, is the standard choice for most highly available releases.

同时, 由于TARS是自带运维管理后台的, 因此它天然比单纯的RPC框架更强大一些。
In addition, TARS is more powerful than a simple RPC framework because its operation platform. 

通过运维管理后台，使用者可以轻松完成如下工作：
* 查看可用服务ip列表。
* 对单台机器进行服务配置变更。
* 服务发布和版本回退。
* 查看调用上报的监控信息。
* 查看机器和服务上报的监控信息。
* 单个或批量进行服务配置。

Users can finish this works easily using operation platform:
* View the available services IP list.
* Modify service configuration of a single machine.
* Service release and version rollback.
* View monitor information reported by RPC calls.
* View monitor information reported by service and machine.
* Configure single service or batch services.
