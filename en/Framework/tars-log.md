# tars-log document

## Introduction
`tars-log` is a logging module for` phptars`, which is used to print log contents to local or remote logs

## Instructions

### Install
Install with `composer`
`composer install phptars/tars-log`

### Configuration

It can be configured one by one, or it can be configured uniformly through the configuration file issued by the platform.
- Configure a parameter individually
```php
$config  = new \Tars\client\CommunicatorConfig();  
$config->setLocator("tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890");  
$config->setModuleName("tedtest");  
$config->setCharsetName("UTF-8");
$config->setLogLevel("INFO");	//`INFO`,`DEBUG`,`WARN`,`ERROR` default:INFO
$config->setSocketMode(2);		//socket mode: 1: socket, 2: swoole tcp client, 3: swoole coroutine tcp client
```
- Configuration file initialization
```php
$config = new \Tars\client\CommunicatorConfig();
$sFilePath = '{project}/src/conf'; //Configuration file path
$config->init($sFilePath);
```

### Output log
There are two ways to output logs. One is to directly call the logger of `LogServant` to output the remote log, and the other is to combine the monolog to output the remote log.

- ` LogServant`

```php
$logServant  = new \Tars\log\LogServant($config);  
$appName = "App";
$serverName = "server";
$file = "test.log";
$format = "%Y%m%d";
$buffer = ["hahahahaha"];
$result = $logServant->logger($appName,$serverName,$file,$format,$buffer);
```

- `monolog`  

```php
$logger = new \Monolog\Logger("tars_logger");
//remote log
$tarsHandler = new \Tars\log\handler\TarsHandler($config);
//local log
$streamHandler = new \Monolog\Handler\StreamHandler(ENVConf::$logPath . "/" . __CLASS__  . ".log");

$logger->pushHandler($tarsHandler);
$logger->pushHandler($streamHandler);

$array = [
    "key1" => "value1",
    "key2" => "value2",
    "key3" => "value3"
];
$logger->debug("add a debug message", $array);
$logger->info("add a info message", $array);
$logger->notice("add a notice message", $array);
$logger->warning("add a warning message", $array);
$logger->error("add a error message", $array);
$logger->critical("add a critical message", $array);
$logger->emergency("add a emergency message", $array);
```

### With ELK
Remote logs are distinguished by application and service. They are located in `/usr/local/app/tars/remote_app_log/{App}/{ServerName}`. In actual business, you can synchronize remote logs to `filebeat` or` logstash`. ElasticSearch`.

In addition, `monolog` itself provides ` ElasticSearchHandler`, which can easily output logs directly to `ElasticSearch`.

The `ruflin/elastica` package needs to be introduced into composer before use.
> Note: Until the update of this article, the release of `ruflin/elastica` only supports elasticsearch: 6. * version. If you want to use elasticsearch: 7. *, you can use the ` elasticsearch/elasticsearch` package to encapsulate the handler yourself

The sample code：

```php
$logger = new \Monolog\Logger("elk_logger");
$client = new \Elastica\Client([
	'host' => '127.0.0.1',
	'port' => 9200
]);
$elkHanlder = new Monolog\Handler\ElasticSearchHandler($client, [
	'index' => 'monolog_index_test',
	'type' => 'record'
]);

$logger->pushHandler($elkHanlder);

$logger->error("this is a test msg from monolog");
```