# Introduction
Yii2 in Tars
* Support Packing code
* Support Yii2 develop
* Support TarsConfig
* Support TarsLog
* Support tars register
* Support Request start(tarsRequesting)、Request end(tarsRequested) event
* Support echo content
* Support http & tars Protocol
* Limited by the design of the framework itself, it does not support cooperation

# Related
* Yii2Tars (https://github.com/luoxiaojun1992/yii2-tars)
* Yii2TarsDemo (https://github.com/luoxiaojun1992/yii2-tars-demo)
* Yii2 (https://github.com/yiisoft/yii2)
* TARSPHP (https://github.com/TarsPHP)
* TARSPHP DOCKER (https://github.com/tangramor/docker-tars)

# Use
1. Create project

   Create the tars project directory structure(scripts、src、tars)，put Yii2 project in src directory

2. Install Yii2 Tars

   Update Composer 

   ```shell
   composer require "luoxiaojun1992/yii2-tars:*"
   ```

   or add requirement to composer.json

   ```json
   {
     "require": {
       "luoxiaojun1992/yii2-tars": "*"
     }
   }
   ```
   
   Initialization Yii2 Tars

   ```
   ./yii tars/publish
   ```

3. Edit `proto` in src/config/params.php ，change `appName、serverName、objName`

4. If you use the HTTP protocol, you need to automatically register to the gateway (now only support Kong)，edit src/config/params.php

   ```php
   'registries' => [
        [
            'type' => 'kong',
            'url' => 'http://kong:8001/upstreams/tars_mysql8/targets', //Fill in according to the actual situation
        ]
   ]
   ```

5. TarsConfig & TarsLog，edit `tars` in src/config/params.php

   The configuration will be pulled automatically when the service starts，If you need to log，can use ```Yii::info('test log');```
   
   If you need to specify the minimum log level for Tarslog logging，edit src/config/params.php
   
   ```php
   'log_level' => ['info']
   ```

   If you need to specify the recording frequency of tarslog，edit `tars` in src/config/params.php

   ```php
   'log_interval' => 1000
   ```

6. If you use HTTP protocol and write code in the framework native way, there is no special requirement for routing

7. If you use the tasrsprotocol

   Write the tars interface description file in the tars directory，edit proto in src/config/params.php,add tarsFiles

   Executing compilation script in scripts directory to generate interface code

   ```shell
   /bin/bash tars2php.sh
   ```

   Create interface implementation class in src/app/Tars/impl directory and write business logic code

   Edit services in src/config/params.php，replace interface and interface implementation namespace


8. Setting up the development environment of TarsPHP

   If using the HTTP protocol, refer to[TARS-PHP-HTTP Server and client development](https://tangramor.gitlab.io/tars-docker-guide/3.TARS-PHP-HTTP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)

   If using the tars protocol, refer to[TARS-PHP-TCP Server and client development](https://tangramor.gitlab.io/tars-docker-guide/2.TARS-PHP-TCP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/)

9. Package the project in the tars PHP development environment(run ```./yii tars/deploy``` in src)

10. Publish the project in the tars management background, please refer to [TARS-PHP-TCP Server and client development](https://tangramor.gitlab.io/tars-docker-guide/2.TARS-PHP-TCP%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BC%80%E5%8F%91/))，测试```curl 'http://{ip}:{port}/{api_route}'```

# Continuous integration
Jenkins Pipeline Configuration example (modified according to actual situation)
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
        def PROJECT_ROOT = "$JENKINS_HOME/workspace/yii2-tars-demo"
        def APP_NAME = "PHPTest"
        def SERVER_NAME = "Yii2Tars"
    }
    stages {
        stage('codePull'){
            steps {
                echo "checkout from git"
                git credentialsId:'2', url: 'https://gitee.com/lb002/yii2-tars-demo', branch: "${env.BRANCH_NAME}"
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "Composer Install"
                        sh "composer clearcache"
                        sh "composer install -vvv"
                    }
                }
            }
        }
        stage('unitTest') {
            steps {
                script {
                    dir("$PROJECT_ROOT/src") {
                        echo "phpunit test"
                        //sh "vendor/bin/phpunit tests/"
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
                        sh "./yii tars/deploy"
                        echo "publishing"
                        sh "ls *.tar.gz > tmp.log"
                        echo "upload build tgz"
                        def packageDeploy = sh(script: "head -n 1 tmp.log", returnStdout: true).trim()
                        sh "curl -H 'Host:172.18.0.3:3000' -F 'suse=@./${packageDeploy}' -F 'application=${APP_NAME}' -F 'module_name=${SERVER_NAME}' -F 'comment=${env.TAG_DESC}' http://172.18.0.3:3000/pages/server/api/upload_patch_package > curl.log"
                        echo "package build tagz"
                        def packageVer = sh(script: "jq '.data.id' curl.log", returnStdout: true).trim()
                        def postJson = '{"serial":true,"items":[{"server_id":"35","command":"patch_tars","parameters":{"patch_id":' + packageVer + ',"bak_flag":false,"update_text":"${env.TAG_DESC}"}}]}'
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
                subject: "[jenkins]Construction notice：${env.JOB_NAME} brunch: ${env.BRANCH_NAME} - Build# ${env.BUILD_NUMBER} fail** !",
                body: '${SCRIPT, template="groovy-html.template"}',
                mimeType: 'text/html',
                to: "luoxiaojun1992@sina.cn",
            )
            cleanWs()
        }
    }
}
```
