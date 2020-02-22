## Install dependency environment of tarsphp
php7 + swoole2+ is recommended.

## Install php
    Project: https://github.com/php/php-src
    
## Install swoole
    Project: https://github.com/swoole/swoole-src
    Install via compile: https://wiki.swoole.com/wiki/page/6.html
    Install via PECL:
        pecl install swoole

## Install tarsphp extension
    Project: https://github.com/TarsPHP/tars-extension
    
        Download & Unzip: https://github.com/TarsPHP/tars-extension/archive/master.zip
        cd tars-extension
        sudo phpize
        sudo ./configure
        sudo make
        sudo make install
        add extension = phptars.so in php.ini

## TarsPHP Development
### [Quick run tars-http-server] (../ QuickStart / tars-http-server.md)

### Development and packaging
    The steps of development is same with a litter different between each languages:
    1. Write tars file xx.tars;
    2. Generate client or server code based on xx.tars by tars conversion tool (statement client or server in configuration, pay attention to the naming convention);
    3. develope the logic code (fill in the blanks);
    4. Package;
    5. Add application information (consistent with the definition of tars) , then upload and publish the package publish through the web management interface.
    6. Package will upload to `/data/tars/patchs/tars.upload/`

### Publish
    Code will be published while packaging, and also can be restarted or closed through operation platform.
    
    Package will be uploaded to `/usr/local/app/tars/tarsnode/` in server -- application name + service name, service name PHPTest, service name PHPHttpServer will unzip the code and place it in PHPTest.PHPHttpServer.
    
    Bin, conf, and data folders are placed in each package directory.
    -bin: the decompressed program files, and start command file -- tars_start.sh -- and close command file -- tars_stop.sh -- which automatically generates by tars.(you can start/stop service by these two files instead of operation platform).
    -conf: the service template configuration, the complete configuration used by service will integrate this file and the configuration which filled in operation platform while publishing.
    -data: the cache file of the service information, and the process id file etc.

### Log View

    Log path is `/usr/local/app/tars/app_log/`, such as `/usr/local/app/tars/app_log/PHPTest/PHPHttpServer/PHPTest.PHPHttpServer.log`
    
    Log path of the service is `/usr/local/app/tars/app_log/{service application name}/{service service name}/`, for example, `/usr/local/app/tars/app_log/Test/HelloServer/`
    
    Path pf service executable is `/usr/local/app/tars/tarsnode/data/{service application name}.{service service name}/bin/`, for example: `/usr/local/app/tars/tarsnode/data/Test.HelloServer/bin/`
    
    Path of service template configuration file is `/usr/local/app/tars/tarsnode/data/{service application name}.{service service name}/conf/`, for example: `/usr/local/app/tars/tarsnode/data/Test.HelloServer/conf/`
    
    Path of service cache information file is `/usr/local/app/tars/tarsnode/data/{service application name}.{service service name}/data/`, for example: `/usr/local/app/tars/tarsnode/data/Test.HelloServer/data/`