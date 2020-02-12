
# TARSPHP简介
# TARSPHP Introduction


TARSPHP作为TARS在PHP语言的解决方案，设计的时候主要考虑如下四个方面：
TARSPHP, as a solution for TARS in PHP, is designed with four aspects:

* 功能完善：对标现有C++、JAVA、NodeJS体系功能。
* 灵活：论灵活，谁与PHP争锋?
* 轻量：用最轻量的设计，点到即止, 即插即用。
* 高效：插上SWOOLE协程的翅膀，不得不飞。

* Perfect functions: benchmark with C++, JAVA, NodeJS. (这句没听懂，不太清楚怎么翻译)
* Flexible: PHP is Flexible.
* Lightweight: lightweight design, plug and play.
* Efficient: support coroutine with SWOOLE.

## 功能完善
## Perfect function

TARSPHP主要实现了如下功能模块：
* TARS-CLIENT
* TARS-SERVER
* TARS-EXT扩展
* TARS2PHP工具

main functional modules of TARSPHP:
* TARS-CLIENT
* TARS-SERVER
* TARS-EXT 
* TARS2PHP 

另有一些其他模块，可在[Framework](../Framework/introduce.md)进行查看。
There are other functional modules in [Framework] (../ Framework / introduce.md).

从宏观上来讲，使用TARSPHP你可以做这些事：
* 启动一个HTTP服务，QPS达到万级
* 启动一个二进制协议的TCP服务，QPS达到十几万级
* 启动一个定时服务，可以统一管理一定数量的定时任务(与worker数量相等)
* 启动一个Websocket服务，与浏览器进行ws协议的交互
* 调用其他支持TARS协议的服务(不限语言)

On the one hand,You can do things withs TARSPHP including:
* Start a HTTP service which can load 10,000 QPS.
* Start a TCP service with binary protocol which can load 100,000 QPS.
* Start a timer service, which can manage a certain number of timer tasks(equal to the number of workers).
* Start a Websocket service, which can interact with the browser using ws protocol.
* Invoke TARS services(unlimited languages).

从细分模块上来讲，你可以：
* 向TARS的本地和远程目录写日志。
* 向TARS监控上报调用监控以获得图表展示。
* 将TARS服务打包发布。
* 拉取TARS平台的配置。
* 拉取其他服务的地址。
* 解析TARS平台的配置文件。

On the other hand,You can use functions following using TARSPHP:
* Write local and remote logs at the same time.
* View chart of the RPC call information which reported to TARS monitor.
* Package and release TARS services.
* Pull configurations of TARS platform.
* Pull other services Address.
* Parse configuration file of TARS platform.

## 灵活
## Flexible

从灵活性上来讲，TARSPHP做了很多优化。
TARSPHP has made a lot of optimizations for better flexibility.

以TARS-CLIENT为例，它的使用者既可以是后台系统，比如Laravel，也可以是SWOOLE同步的客户端, 更可以是SWOOLE协程的客户端。
For example, TARS-CLIENT can be used by a back-end systems such as Laravel, it also can be used by a synchronization SWOOLE client or a coroutines SWOOLE client.

这三种情况，兼顾了不同的应用场景，使用者是需要按需配置即可。
These three solutions can deal with different application scenarios.

除此之外，TARS-LOG模块，既支持本地的日志，也支持远程的日志，使用者也可以灵活的选择。
In addition, TARS-LOG supports both local logs and remote logs.

## 轻量
## Lightweight

为了最大程度的提高性能和与其他成熟框架进行扩展，我们选择实现轻量的框架，同时提供实现其他功能的指南，让开发者可以根据自己的情况进行定制。
In order to maximize performance and support extend with other mature frameworks, we design TARSPHP lightweight and provide an implementation guideline to developers to customize function according to their own circumstances.

现有社区中，已有不少TARSPHP与成熟框架进行扩展的案例:
There are some cases that TARSPHP extend with frameworks inside community including:

* [与Kong结合](https://tangramor.gitlab.io/tars-docker-guide/3.TARS-PHP-HTTP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)
* [与PHALCON结合]()
* [与SWOFT结合](../Advanced/swoft.md)
* [与Laravel结合](../Advanced/laravel.md)
* [与Yii2结合](../Advanced/yii2.md)

* [Combined with ong](https://tangramor.gitlab.io/tars-docker-guide/3.TARS-PHP-HTTP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)
* [Combined with PHALCON]()
* [Combined with SWOFT](../Advanced/swoft.md)
* [Combined with Laravel](../Advanced/laravel.md)
* [Combined with Yii2](../Advanced/yii2.md)

我们会不断尝试接入更多的框架，从而更好的为开发者服务。
We will try to combined with more frameworks in the future to service developers better.

## 高效
在性能方面，TARSPHP也一直在追求极致:
* 使用SWOOLE作为服务容器。
* 使用PHP扩展进行二进制流的打包和解包。
* 对服务端的路由配置进行预热，减少路由时间。
* 对寻址信息进行本地缓存，降低耗时。
* 对日志写入、监控上报、服务保活上报进行异步化处理，绝不阻塞。


## Efficient
TARSPHP always pursue extreme performance:
* Use SWOOLE as a service container.
* Use PHP extensions to pack and unpack binary streams.
* Preheat the routing configuration on the server to reduce routing time.
* Locally cache addressing information to reduce time consumption.
* Asynchronous processing of log writing, monitoring reporting and service keepalive reporting to prevent block.
