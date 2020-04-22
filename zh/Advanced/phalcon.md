# 简介
Phalcon集成微服务治理框架Tars
* 支持服务打包
* 支持Phalcon部分原生开发(路由、session暂不支持)
* 支持TarsConfig
* 支持http和tars协议
* 受限于框架本身的设计，不支持协程

# 相关项目
* tars-php-phalcon (https://github.com/luoxiaojun1992/tars-php-phalcon)
* Phalcon (https://github.com/phalcon/cphalcon)
* TARSPHP (https://github.com/TarsPHP)
* TARSPHP DOCKER (https://github.com/tangramor/docker-tars)

# 感谢
* [iamloso](https://github.com/iamloso)

# 项目目录结构
- component: 存储Controller的基础类,方便所有的Controller公用
- conf: 业务需要的配置
- controller: MVC模型中的C层
- database: 数据库连接配置
- libs: 存储公用类库, server/model扩展类库, 插件工具类 
- models: 存储库表对应的model文件
- servant: 使用tars2php生成的client端的代码, 这个目录名称完全可以自定义,只需要在使用的时候加以区分就可以了
- services: 业务服务实现类
- tasks:  命令行方式执行业务services, 可以方便业务代码调试, 也可以做crontable定时任务
- composer.json: 说明项目的依赖情况
- index.php: 整个服务的入口文件,这个文件名称可以自定义,但是必须要更改平台上的私有模板,增加entrance这个字段在server下面
- services.php: 声明整个项目的基础namespaceName

# 使用
1. 创建项目

   克隆示例代码

   ```shell
   git clone git@github.com:iamloso/tars-php-phalcon.git
   ```

   安装composer依赖

   ```shell
   cd src && composer install -vvv
   ```

2. 配置服务基本信息

   修改tars/tars.proto.php中的appName、serverName、objName

3. 如果使用tars协议

   在tars目录下编写tars接口描述文件，修改tars/tars.proto.php，新增tarsFiles

   在scripts目录执行编译脚本生成接口代码

   ```shell
   /bin/bash tars2php.sh
   ```

   在src/services目录下创建接口实现类，编写业务逻辑代码

   修改src/services.php，替换接口(servant)和接口实现命名空间

4. 搭建Tars-PHP开发环境

   如果使用http协议，请参考[TARS-PHP-HTTP服务端与客户端开发](https://tangramor.gitlab.io/tars-docker-guide/3.TARS-PHP-HTTP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)

   如果使用tars协议，请参考[TARS-PHP-TCP服务端与客户端开发](https://tangramor.gitlab.io/tars-docker-guide/2.TARS-PHP-TCP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)

5. 在Tars-PHP开发环境下打包项目(在src目录下执行```composer run-script deploy```)

6. 在Tars管理后台发布项目，请参考[TARS-PHP-TCP服务端与客户端开发](https://tangramor.gitlab.io/tars-docker-guide/2.TARS-PHP-TCP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/))，测试```curl 'http://{ip}:{port}/{api_route}'```