# tars-monitor documentation

## Introduction
`tars-monitor` is the ` phptars` service and feature monitoring report module

It consists of two sub-modules:
* Service monitoring
* Feature monitoring

## Instructions

### Installation

Install with composer
`composer install phptars/tars-monitor`

### Usage

#### Service monitoring report

* locator is the reporting address, usually issued by the server
* socketMode is set to 1 to report using socket mode
   socketMode is set to 2 to report using swoole tcp client (requires swoole support)
   socketMode is set to 3 to report using swoole tcp coroutine client coroutine method (requires support of swoole2.0 or above)

#### Timely reporting (default)
> The use of regular reporting requires swoole_table support. By calling addStat to temporarily store the reporting information, the task process of the tars-server will collect for a period of time (the reporting interval is issued by the server, generally 60s).

```php
$locator = "tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890";
$socketMode = 1;
$statfWrapper = new \Tars\monitor\StatFWrapper($locator, $socketMode);
$statfWrapper->addStat ("PHPTest.helloTars.obj", "test", "172.16.0.116", 51003,200,0,0);
$statfWrapper->addStat ("PHPTest.helloTars.obj", "test", "172.16.0.116", 51003,200,0,0);
$statfWrapper->addStat ("PHPTest.helloTars.obj", "test", "172.16.0.116", 51003,200,0,0);
```

Reported data can use multiple storage methods. The cache provides the implementation of `swoole_table` and ` redis`. Users can also implement `contract/StoreCacheInterface` by themselves. For the configuration storage method, please refer to [examples](https://github.com/TarsPHP/TarsPHP/tree/master/examples)  `tars-http-server` configuration for` src / services.php`.

```php
return array (
    'namespaceName' => 'HttpServer \\',
    'monitorStoreConf' => [
        'className' => Tars \ monitor \ cache \ SwooleTableStoreCache :: class,
        'config' => []
    ]
);
```

`monitorStoreConf` is the configuration of the storage method, ` className` is the implementation class, and `config` is the corresponding configuration. For example, when using the redis storage method, you need to configure the redis host, port, and key prefix in the config.
When `monitorStoreConf` is not configured, ` SwooleTableStoreCache` is used for storage by default.


#### Single report
> At the same time, `tars-monitor` also provides a single report interface` monitorStat`, that is, each tars request call will be reported once.

```php
$locator = "tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890";
$socketMode = 1;
$statfWrapper  = new \Tars\monitor\StatFWrapper($locator,$socketMode);  
$statfWrapper->monitorStat("PHPTest.helloTars.obj","test","172.16.0.116",51003,200,0,0);
```



#### Feature monitoring

Parameters are similar to service monitoring

```php
$statfWrapper  = new \Tars\monitor\PropertyFWrapper("tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890",1);  
$statfWrapper->monitorProperty("127.0.0.1","userdefined",'Sum',2);  
$statfWrapper->monitorProperty("127.0.0.1","userdefined",'Count',2);  
$statfWrapper->monitorProperty("127.0.0.1","userdefined",'Count',1);
```



### Monitor View
After the data is reported, the user can view the reported data in the Service Monitoring / Feature Monitoring tab.

## Other
Because other modules have integrated this module, __so in general, the service script does not need to use this module explicitly. __