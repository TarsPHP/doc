# tars-report documentation

## Introduction
`tars-report` is the ` phptars` heartbeat report (keep alive) module
The tarsphp framework will report the service alive status when the master process starts (main process alive & management process alive).
When the first worker process is started, a timer will be started to periodically report the survival status.

## Instructions

```php
        $serverInfo = new ServerInfo();
        $serverInfo->adapter = 'PHPTest.test.objAdapter';
        $serverInfo->application = 'PHPTest';
        $serverInfo->serverName = 'test';
        $serverInfo->pid = $masterPid;

        $serverF = new ServerFSync($host, $port, $objName);
        $serverF->keepAlive($serverInfo); //主进程存活

        $adminServerInfo = new ServerInfo();
        $adminServerInfo->adapter = 'AdminAdapter';
        $adminServerInfo->application = 'PHPTest';
        $adminServerInfo->serverName = 'test';
        $adminServerInfo->pid = $masterPid;
        $serverF->keepAlive($adminServerInfo);//管理进程存活
```

## Other

Because other modules have integrated this module, __so in general, the service script does not need to use this module explicitly. __

