# tars-registry documentation

## Introduction
The tarsregistry service of the tars platform provides a service discovery function.
This module provides the main control addressing capability (service discovery) for php.

## Instructions

```php
        //从tarsregistry服务寻找服务地址
        $wrapper = new \Tars\registry\QueryFWrapper("tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890",1,60000);
        $result = $wrapper->findObjectById("PHPTest.PHPServer.obj");
        var_dump($result);

        //优先从内存寻找服务地址，其次从主控寻址
        \Tars\registry\RouteTable::getInstance();
        $result = \Tars\registry\RouteTable::getRouteInfo("PHPTest.PHPServer.obj");
        echo "result:\n";
        var_dump($result);
```

## Other

Because other modules have integrated this module, __so in general, the service script does not need to use this module explicitly. __