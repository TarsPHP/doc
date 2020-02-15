# Frequently Asked Questions

## Does this framework have to be bundled with swoole?
Tars-server must be bundled with swoole. But if you only use the tars-client, you won't need to.

## What is the difference between the client and server?
They have different responsibilities and different project structures. Client is just a caller, server is a service provider.

## Is it possible to use only tars-php as a client without swoole?
Yes, you can. Only tars-server is bundled with swoole and tars-client is not restricted.

## Must monitoring data be stored in the swoole_table?
Users can implement the `Tars\monitor\contract\StoreCacheInterface` to implement other storage methods, as shown in [the sample code](!https://github.com/TarsPHP/TarsPHP/tree/master/examples/tars-http-server).

## Error when generating code automatically
Some users executed the command `php ../src/vendor/phptars/tars2php/src/rc/tars2php.php ./t ./ ./tars.proto.php` incorrectly：
>mkdir: cannot create directory ‘../src/servant/Common’: No such file or directory
  
This is a directory permission issue. You can manually create *../src/servant*, or modify directory permissions.

## Where is the manual startup script?
In the *./bin* directory under the service root.

## After the manual script was executed, the setting state was not active.
Please try to restart the service on the platform to avoid manual script restart. For users that want to automate the release flow, you can restart the service by calling the http restart api.

## How to deploy multiple registries?
Deploy the two registers and the conf file will be automatically updated after the business service is restarted.

## When deploying TarsPHP on multiple machines, is the installation of the second machine exactly the same as the first, or does the second machine require only partial service installation?
On the second machine, you only need to install the core base service and tarsnode, while others, such as mysql, do not need to be installed.

## Error when downloading composer packages
```
composer require phptars/tars-server
```
Some users have encountered the above command execution failure. For this problem, first make sure that the local composer is ok. Then judge the specific reason according to the message of failure. The problem once occurred was that the domestic source was not synchronized, resulting in inconsistent versions. After switching sources, the problem was solved.

## The configuration center sends tcp requests to the service. How to use it in the http server?
The push of the configuration center is not available for the http server. For the http server, you can specify the configuration file, which will be pulled when starting or stopping.

## Is the tarslog compatiable with ELK?
The tarslog component can implement log output to elk by adding ElasticSearchHandler, with an interface reserved.

## The tars runs in the windows, publishing service failed.
If you encountered the problem like `pid is not digit`，it is because the method of fetching pid in the windows is difference with it in the linux，and the startup script needs to be modified with powershell to be compatible, For example:    
`ps -eopid,cmd | grep "$bin"| grep "tarsnode" |  grep -v "grep"|grep -v "sh" |awk '{print $1}'`

## Does the dispatch_mode have to be 2 in the configuration file?
The dispatch mode 2 means that the data sent by a request will only be processed by one worker. This configuration can be adjusted as needed, as detailed in [the swoole documentation](!https://wiki.swoole.com/wiki/page/277.html).

## Missing tars extension
Problems like the following are caused by not installing the tars extension. Please refer to php environment setup for installation.
>PHP Fatal error:  Class 'TARS_Struct' not found in /usr/local/app/tars/tarsnode/data/PHPTest.PHPServer/bin/src/vendor/phptars/tars-report/src/ServerInfo.php on line 5

>Fatal error: Uncaught Error: Class 'TUPAPI' not found in D:\www\tars-client\tests\vendor\phptars\tars-registry\src\client\TUPAPIWrapperRegistry.php:369
