##安装tarsphp依赖环境
    建议安装php7+swoole2+

## Install dependency environment of tarsphp
php7 + swoole2+ is recommended.

##安装php
    项目地址 https://github.com/php/php-src 

## Install php
    Project: https://github.com/php/php-src
    
##安装swoole
    项目地址 https://github.com/swoole/swoole-src
    编译安装 https://wiki.swoole.com/wiki/page/6.html
    PECL一键下载安装
        pecl install swoole
        
## Install swoole
    Project: https://github.com/swoole/swoole-src
    Install via compile: https://wiki.swoole.com/wiki/page/6.html
    Install via PECL:
        pecl install swoole

##安装tarsphp扩展
    项目地址 https://github.com/TarsPHP/tars-extension
    
        下载&解压 https://github.com/TarsPHP/tars-extension/archive/master.zip
        cd tars-extension
        sudo phpize 
        sudo ./configure
        sudo make 
        sudo make install
        在php.ini里面加入extension=phptars.so


## Install tarsphp extension
    Project: https://github.com/TarsPHP/tars-extension
    
        Download & Unzip: https://github.com/TarsPHP/tars-extension/archive/master.zip
        cd tars-extension
        sudo phpize
        sudo ./configure
        sudo make
        sudo make install
        add extension = phptars.so in php.ini


##TarsPHP开发相关
### [快速运行tars-http-server](../QuickStart/tars-http-server.md)

## TarsPHP Development
### [Quick run tars-http-server] (../ QuickStart / tars-http-server.md)

### 开发和打包
    不同语言在开发上存在差异，但基本思路一致：
    1. 编写tars文件xx.tars；
    2. 利用tars官方转换工具根据tars文件内容生成客户端或服务端代码(在配置文件中指定生成客户端还是服务端，注意命名规范)；
    3. 完善代码逻辑（完形填空）；
    4. 打包代码；
    5. 通过web管理界面添加应用信息（与tars定义保持一致）并上传以上步骤最终的打包代码并发布。
    6. 代码打包文件上传位置`/data/tars/patchs/tars.upload/`

### Development and packaging
    The steps of development is same with a litter different between each languages:
    1. Write tars file xx.tars;
    2. Generate client or server code based on xx.tars by tars conversion tool (statement client or server in configuration, pay attention to the naming convention);
    3. develope the logic code (fill in the blanks);
    4. Package;
    5. Add application information (consistent with the definition of tars) , then upload and publish the package publish through the web management interface.
    6. Package will upload to `/data/tars/patchs/tars.upload/`

### 发布
    代码打包同时会执行发布过程，还可以在服务管理列表中重启或关闭处理。
    
    打包程序上传后，会放置在应用服务器的/usr/local/app/tars/tarsnode/data目录下，——应用名+服务名，服务名PHPTest，服务名PHPHttpServer则会将代码解压放置在PHPTest.PHPHttpServer目录下 (没看懂。。)
    
    每个完整的目录下会放置bin、conf、data文件夹，基本作用为：
    - bin目录下放置解压后的程序文件，tars同时自动生成对应的启动命令文件和关闭命令文件 tars_start.sh、tars_stop.sh （也可以跳过web管理界面直接使用此文件开启/关闭服务）
    - conf目录下存放此应用相关的服务模版配置文件——结合应用发布填写的配置项生成的最终配置文件；
    - data目录下放置服务的缓存信息文件，以及进程id文件等

### Publish
    Code will be published while packaging, and also can be restarted or closed through operation platform.
    
    Package will be uploaded to `/usr/local/app/tars/tarsnode/` in server -- application name + service name, service name PHPTest, service name PHPHttpServer will unzip the code and place it in PHPTest.PHPHttpServer.
    
    Bin, conf, and data folders are placed in each package directory.
    -bin: the decompressed program files, and start command file -- tars_start.sh -- and close command file -- tars_stop.sh -- which automatically generates by tars.(you can start/stop service by these two files instead of operation platform).
    -conf: the service template configuration, the complete configuration used by service will integrate this file and the configuration which filled in operation platform while publishing.
    -data: the cache file of the service information, and the process id file etc.

### 日志查看

    日志目录位置 /usr/local/app/tars/app_log/，在此目录下的应用名称目录下找到对应的日志文件，如 /usr/local/app/tars/app_log/PHPTest/PHPHttpServer/PHPTest.PHPHttpServer.log
    
    服务打的日志路径在/usr/local/app/tars/app_log/服务的应用名/服务的服务名/目录下，例如:/usr/local/app/tars/app_log/Test/HelloServer/
    
    服务的可执行文件在/usr/local/app/tars/tarsnode/data/服务的应用名.服务的服务名/bin/下,例如:/usr/local/app/tars/tarsnode/data/Test.HelloServer/bin/
    
    服务的模版配置文件在/usr/local/app/tars/tarsnode/data/服务的应用名.服务的服务名/conf/下,例如:/usr/local/app/tars/tarsnode/data/Test.HelloServer/conf/
    
    服务的缓存信息文件在/usr/local/app/tars/tarsnode/data/服务的应用名.服务的服务名/data/下,例如:/usr/local/app/tars/tarsnode/data/Test.HelloServer/data/


### Log View

    Log path is `/usr/local/app/tars/app_log/`, such as `/usr/local/app/tars/app_log/PHPTest/PHPHttpServer/PHPTest.PHPHttpServer.log`
    
    Log path of the service is `/usr/local/app/tars/app_log/{service application name}/{service service name}/`, for example, `/usr/local/app/tars/app_log/Test/HelloServer/`
    
    Path pf service executable is `/usr/local/app/tars/tarsnode/data/{service application name}.{service service name}/bin/`, for example: `/usr/local/app/tars/tarsnode/data/Test.HelloServer/bin/`
    
    Path of service template configuration file is `/usr/local/app/tars/tarsnode/data/{service application name}.{service service name}/conf/`, for example: `/usr/local/app/tars/tarsnode/data/Test.HelloServer/conf/`
    
    Path of service cache information file is `/usr/local/app/tars/tarsnode/data/{service application name}.{service service name}/data/`, for example: `/usr/local/app/tars/tarsnode/data/Test.HelloServer/data/`