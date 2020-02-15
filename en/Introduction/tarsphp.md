# TARSPHP Introduction

TARSPHP, as a solution for TARS in PHP, was designed with four priority:

* sufficient functionality: integret with available abilities within C++, Java and NodeJS.
* Flexible: PHP is Flexible.
* Lightweight: lightweight design, plug and play.
* Highly Efficient: support coroutine with SWOOLE.

## Sufficient Functionality

main functional modules of TARSPHP:
* TARS-CLIENT
* TARS-SERVER
* TARS-EXT 
* TARS2PHP 

There are other functional modules in [Framework] (../ Framework / introduce.md).

From the big picture ,You could do things withs TARSPHP including:
* Start an HTTP service with maximum QPS reaching ten thousands.
* Start a TCP service with binary protocol with maximum QPS reaching a 100,000.
* Start a timer service, which can manage a certain number of timer tasks(equal to the number of workers).
* Start a Websocket service, which can interact with the browser using ws protocol.
* Invoke TARS services( language agnostic).

From the smaller module's point of view, you could:
* Write local and remote logs at the same time.
* View chart of the RPC call information which reported to TARS monitor.
* Package and release TARS services.
* Pull configurations of TARS platform.
* Pull other services Address.
* Parse configuration file of TARS platform.

## Flexible

TARSPHP has made a lot of optimizations for better flexibility.

In the case of TARS-CLIENT, it can be used by a back-end systems such as Laravel,  a synchronization SWOOLE client, or a coroutines SWOOLE client.

These three solutions can deal with different application scenarios.

In addition, TARS-LOG supports both local logs and remote logs.

## Lightweight

In order to maximize performance and support extension with other mature frameworks, we designed TARSPHP in lightweight and provide an implementation guideline to developers to customize function according to their own needs.

There are some cases where TARSPHP extends with frameworks inside the community including:

* [Integrated with ong](https://tangramor.gitlab.io/tars-docker-guide/3.TARS-PHP-HTTP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)
* [Integrated with PHALCON]()
* [Integrated with SWOFT](../Advanced/swoft.md)
* [Integrated with Laravel](../Advanced/laravel.md)
* [Integrated with Yii2](../Advanced/yii2.md)

We will try to integrate with more frameworks in the future to serve developers better.

## Highly Efficient

TARSPHP always pursue extreme performance:
* Use SWOOLE as a service container.
* Use PHP extensions to pack and unpack binary streams.
* Preheat the routing configuration on the server to reduce routing time.
* Locally cache addressing information to reduce time consumption.
* Asynchronousl'y process log writing, monitoring and service keepalive reporting to prevent block.
