# tars-utils documentation

## Introduction
`tars-utils` is an auxiliary library for phptars, which mainly provides the following functions:
* Configuration file analysis and cache (swoole table)
* Master address resolution
* Node report configuration analysis

## Instructions
### Configuration file parsing

Take [QDPHP.TARSServer.config.conf]() as an example, parse the `conf` file into an array

### Geocoding
```php
$locatorString = "tars.tarsregistry.QueryObj@tcp -h 127.0.0.1 -p 17890:tcp -h 127.0.0.1 -p 17890";

$locatorInfo = \Tars\Utils::getLocatorInfo($locatorString);
```
The results are as follows：
```php
[
	'locatorName' => 'tars.tarsregistry.QueryObj',
	'routeInfo' => [
		[
			'sHost' => '127.0.0.1',
			'sProtocol' => 'tcp',
			'iPort' => 17890,
			'iTimeout' => '',
			'bIp' => '',
			'sIp' => ''
		],
		[
			'sHost' => '127.0.0.1',
			'sProtocol' => 'tcp',
			'iPort' => 17890,
			'iTimeout' => '',
			'bIp' => '',
			'sIp' => ''
		]
	]
]
```

### Node report configuration analysis
```php
$locatorString = "tars.tarsnode.ServerObj@tcp -h 127.0.0.1 -p 2345 -t 10000";

$nodeInfo = \Tars\Utils::parseNodeInfo($locatorString);
```

The results are as follows:
```php
[
	'objName' => 'tars.tarsnode.ServerObj',
	'mode' => 'tcp',
	'host' => '127.0.0.1',
	'port' => 2345,
	'timeout' => 1,
	'sIp' => '',
	'iPort' => 2345
]
```