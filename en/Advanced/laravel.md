# Introduction
Laravel in Tars with ci
* Support Packing code
* Support Laravel/Lumen develop
* Support TarsConfig
* Support TarsLog
* Support tars register
* Support Request start(laravel.tars.requesting)、Request end(laravel.tars.requested) event
* Support echo content
* Support http & tars Protocol
* Support zipkin (Removed，can use[laravel-zipkin](https://github.com/luoxiaojun1992/laravel-zipkin))
* Limited by the design of the framework itself, it does not support cooperation

# Related 
* LaravelTars (https://github.com/luoxiaojun1992/laravel-tars)
* LaravelTarsDemo (https://github.com/luoxiaojun1992/laravel-tars-demo)
* LumenTarsDemo (https://github.com/luoxiaojun1992/lumen-tars-demo)
* Laravel5.x (https://github.com/laravel/framework)
* Lumen5.x (https://github.com/laravel/lumen)
* TARSPHP (https://github.com/TarsPHP)
* TARSPHP DOCKER (https://github.com/tangramor/docker-tars)

# Use
1. Create project

   Create the tars project directory structure (scripts、src、tars)，put Laravel/Lumen project in src directory.

2. Install Laravel Tars

   Update Composer 

   ```shell
   composer require "luoxiaojun1992/laravel-tars:*"
   ```

   or add requirement to composer.json

   ```json
   {
     "require": {
       "luoxiaojun1992/laravel-tars": "*"
     }
   }
   ```

   add ServiceProvider，edit src/bootstrap/app.php
   
   ```php
   $app->register(\Lxj\Laravel\Tars\ServiceProvider::class);
   ```
   
   Initialization Laravel Tars

   ```
   php artisan vendor:publish --tag=tars
   ```

3. edit `proto` in src/config/tars.php ，change `appName、serverName、objName`

4. If you use the HTTP protocol, you need to automatically register to the gateway (now only support Kong)，edit src/config/tars.php

   ```php
   'registries' => [
        [
            'type' => 'kong',
            'url' => 'http://kong:8001/upstreams/tars_mysql8/targets', //Fill in according to the actual situation
        ]
   ]
   ```

5. TarsConfig & TarsLog

   The configuration will be pulled automatically when the service starts，If you need to log，can use ```Log::info('test log');```
   
   If you need to specify the minimum log level for Tarslog logging，edit src/config/tars.php
   
   ```php
   'log_level' => \Monolog\Logger::INFO
   ```

6. If you use the HTTP protocol, code in a framework native way

   ```php
    $router->get('/test', function () {
        \Illuminate\Support\Facades\Log::info('laravel tars test log');
        return 'Laravel Tars Test Success';
    });
   ```

7. If you use the tasrsprotocol

   Write the tars interface description file in the tars directory，edit proto in src/config/tars，add tarsFiles

   Executing compilation script in scripts directory to generate interface code

   ```shell
   /bin/bash tars2php.sh
   ```

   Create interface implementation class in src/app/Tars/impl directory and write business logic code

   Edit services in src/config/tars.php，replace interface and interface implementation namespace


8. Setting up the development environment of TarsPHP

   If using the HTTP protocol, refer to[TARS-PHP-HTTP Server and client development](https://tangramor.gitlab.io/tars-docker-guide/3.TARS-PHP-HTTP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)

   If using the tars protocol, refer to[TARS-PHP-TCP Server and client development](https://tangramor.gitlab.io/tars-docker-guide/2.TARS-PHP-TCP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)

9. Package the project in the tars PHP development environment(run ```php artisan tars:deploy``` in src)

10. Publish the project in the tars management background, please refer to [TARS-PHP-TCP Server and client development](https://tangramor.gitlab.io/tars-docker-guide/2.TARS-PHP-TCP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/))，测试```curl 'http://{ip}:{port}/{api_route}'```

# Continuous integration
Jenkins Pipeline Configuration example (modified according to actual situation)

Laravel:
```
pipeline {
    agent {
        node {
            label 'phpenv'
        }
    }
    parameters {
        string(defaultValue: 'upload_from_jenkins', name: 'TAG_DESC', description: 'description' )
        string(defaultValue: 'master', name: 'BRANCH_NAME', description: 'git brunch，such as：develop,master  default: master')
    }
    environment {
        def JENKINS_HOME = "/root/jenkins"
        def PROJECT_ROOT = "$JENKINS_HOME/workspace/laravel-tars-demo"
        def APP_NAME = "PHPTest"
        def SERVER_NAME = "LaravelTars"
    }
    stages {
        stage('codePull'){
            steps {
                echo "checkout from git"
                git credentialsId:'2', url: 'https://gitee.com/lb002/laravel-tars-demo', branch: "${env.BRANCH_NAME}"
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "Composer Install"
                        sh "composer clear-cache"
                        sh "composer install -vvv"
                        sh "cp .env.example .env"
                        sh "php artisan config:clear"
                        sh "php artisan config:cache"
                    }
                }
            }
        }
        stage('unitTest') {
            steps {
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "phpunit test"
                        sh "vendor/bin/phpunit tests/"
                        echo "valgrind test"
                    }
                }
            }
        }
        stage('coverageTest') {
            steps {
                echo "LCOV coverageTest"
            }
        }
        stage('Compiling and publishing') {
            steps {
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "deploy"
                        sh "php artisan tars:deploy"
                        echo "publishing"
                        sh "ls *.tar.gz > tmp.log"
                        echo "upload build tgz"
                        def packageDeploy = sh(script: "head -n 1 tmp.log", returnStdout: true).trim()
                        sh "curl -H 'Host:172.18.0.3:3000' -F 'suse=@./${packageDeploy}' -F 'application=${APP_NAME}' -F 'module_name=${SERVER_NAME}' -F 'comment=${env.TAG_DESC}' http://172.18.0.3:3000/pages/server/api/upload_patch_package > curl.log"
                        echo "package build tagz"
                        def packageVer = sh(script: "jq '.data.id' curl.log", returnStdout: true).trim()
                        def postJson = '{"serial":true,"items":[{"server_id":"34","command":"patch_tars","parameters":{"patch_id":' + packageVer + ',"bak_flag":false,"update_text":"${env.TAG_DESC}"}}]}'
                        echo postJson
                        sh "curl -H 'Host:172.18.0.3:3000' -H 'Content-Type:application/json' -X POST --data '${postJson}' http://172.18.0.3:3000/pages/server/api/add_task"
                    }
                }
            }
        }
    }
    post {
        success {
            emailext (
                subject: "[jenkins]Construction notice：${env.JOB_NAME} brunch: ${env.BRANCH_NAME} - Build# ${env.BUILD_NUMBER} success  !",
                body: '${SCRIPT, template="groovy-html.template"}',
                mimeType: 'text/html',
                to: "luoxiaojun1992@sina.cn",
            )
            cleanWs()
        }
        failure {
            emailext (
                subject: "[jenkins]Construction notice：${env.JOB_NAME} brunch: ${env.BRANCH_NAME} - Build# ${env.BUILD_NUMBER} fail !",
                body: '${SCRIPT, template="groovy-html.template"}',
                mimeType: 'text/html',
                to: "luoxiaojun1992@sina.cn",
            )
            cleanWs()
        }
    }
}
```

Lumen:
```
pipeline {
    agent {
        node {
            label 'phpenv'
        }
    }
    parameters {
        string(defaultValue: 'upload_from_jenkins', name: 'TAG_DESC', description: 'description' )
        string(defaultValue: 'master', name: 'BRANCH_NAME', description: 'gitbrunch，such as：develop,master  default: master')
    }
    environment {
        def JENKINS_HOME = "/root/jenkins"
        def PROJECT_ROOT = "$JENKINS_HOME/workspace/lumen-tars-demo"
        def APP_NAME = "PHPTest"
        def SERVER_NAME = "LumenTars"
    }
    stages {
        stage('codePull'){
            steps {
                echo "checkout from git"
                git credentialsId:'2', url: 'https://gitee.com/lb002/lumen-tars-demo', branch: "${env.BRANCH_NAME}"
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "Composer Install"
                        sh "composer clear-cache"
                        sh "composer install -vvv"
                        sh "cp .env.example .env"
                    }
                }
            }
        }
        stage('unitTest') {
            steps {
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "phpunit test"
                        sh "vendor/bin/phpunit tests/"
                        echo "valgrind test"
                    }
                }
            }
        }
        stage('coverageTest') {
            steps {
                echo "LCOV coverageTest"
            }
        }
        stage('Compiling and publishing') {
            steps {
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "deploy"
                        sh "php artisan tars:deploy"
                        echo "publishing"
                        sh "ls *.tar.gz > tmp.log"
                        echo "upload build tgz"
                        def packageDeploy = sh(script: "head -n 1 tmp.log", returnStdout: true).trim()
                        sh "curl -H 'Host:172.18.0.3:3000' -F 'suse=@./${packageDeploy}' -F 'application=${APP_NAME}' -F 'module_name=${SERVER_NAME}' -F 'comment=${env.TAG_DESC}' http://172.18.0.3:3000/pages/server/api/upload_patch_package > curl.log"
                        echo "package build tagz"
                        def packageVer = sh(script: "jq '.data.id' curl.log", returnStdout: true).trim()
                        def postJson = '{"serial":true,"items":[{"server_id":"33","command":"patch_tars","parameters":{"patch_id":' + packageVer + ',"bak_flag":false,"update_text":"${env.TAG_DESC}"}}]}'
                        echo postJson
                        sh "curl -H 'Host:172.18.0.3:3000' -H 'Content-Type:application/json' -X POST --data '${postJson}' http://172.18.0.3:3000/pages/server/api/add_task"
                    }
                }
            }
        }
    }
    post {
        success {
            emailext (
                subject: "[jenkins]Construction notice：${env.JOB_NAME} brunch: ${env.BRANCH_NAME} - Build# ${env.BUILD_NUMBER} success  !",
                body: '${SCRIPT, template="groovy-html.template"}',
                mimeType: 'text/html',
                to: "luoxiaojun1992@sina.cn",
            )
            cleanWs()
        }
        failure {
            emailext (
                subject: "[jenkins]Construction notice：${env.JOB_NAME} brunch: ${env.BRANCH_NAME} - Build# ${env.BUILD_NUMBER} fail !",
                body: '${SCRIPT, template="groovy-html.template"}',
                mimeType: 'text/html',
                to: "luoxiaojun1992@sina.cn",
            )
            cleanWs()
        }
    }
}
```
