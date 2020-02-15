# tars-config document

## Introduction

**Tars-config** is used to obtain the configuration delivered from the tars management platform. The usual usage scenario is to obtain the business configuration, such as mysql, redis address, port and other information

## Instructions

```php
<?php

	require_once "../vendor/autoload.php";
	$config = new \Tars\client\CommunicatorConfig();
	$config->setLocator("tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890"); //The tars master address is configured here
	$config->setModuleName("tedtest"); //The module name is used to display and report
	$config->setCharsetName("UTF-8");

	$conigServant = new \Tars\config\ConfigServant($config); 
	$result = $conigServant->loadConfig("PHPTest",'helloTars','hhh.txt',$configtext); //The parameters are appName (servant name first part), server name (servant name second part), file name, and the last one is a reference parameter, which is the output configuration file content.
	var_dump($configtext);

	$config->setSocketMode(2); //set socket model 1:socket, 2:swoole tcp client, 3:swoole coroutine client
	$conigServant = new \Tars\config\ConfigServant($config);
	$result = $conigServant->loadConfig("PHPTest",'helloTars','hhh.txt',$configtext);
	var_dump($configtext);
```

