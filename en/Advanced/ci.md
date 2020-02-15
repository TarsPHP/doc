# Continuous integration solution
Tars web not yet direct supported git/svn, need upload zip file every time，Here is a continuous integration scheme based on 'Jenkins' for your reference.
Specific business may need to be adjusted according to the actual situation.

# Steps
> Here we use the pipeline of 'Jenkins' to build a continuous integration environment(use tars web http api).
Take 'QD. Acthttpserver' as an example from [github examples](https://github.com/TarsPHP/TarsPHP/tree/master/examples)

1. Create a new build node **phpenv**，node can use docker，contain：
    - `php` 
    - `composer`
    - `jq` The command line tool of JSON under Linux is convenient for parsing JSON returned from HTTP interface
    - `phpunit` non essential
    - `valgrind` non essential
                 
2. Install the following `plugin`
    - Valgrind Plug-in
    - Pipeline
    - Workspace Cleanup Plugin
3. Create a new pipeline job: **QD.ActHttpServer**
3. **Pipeline**  script：
    ```
         pipeline {
                    agent {
                        node {
                            label 'phpenv'
                        }
                    }
                    parameters { 
                        string(defaultValue: 'upload_from_jenkins', name: 'TAG_DESC', description: 'Release description' )
                        string(defaultValue: 'master', name: 'BRANCH_NAME', description: 'git branch，such as：develop,master  default: master')
                    }
                    environment {
                        def JENKINS_HOME = "/home/jenkins"
                        def PROJECT_ROOT = "$JENKINS_HOME/workspace/QD.ActHttpServer"
                        def APP_NAME = "QD"
                        def SERVER_NAME = "ActHttpServer"
                    }
                    stages {
                        stage('codePull'){
                            steps {
                                echo "checkout from git"
                                git credentialsId:'2', url: 'https://github.com/TarsPHP/TarsPHP', branch: "${env.BRANCH_NAME}"
                            }
                        }
                        stage('unitTest') {
                            steps {
                                echo "phpunit test"
                                echo "valgrind test"
                            }
                        }
                        stage('coverageTest') {
                            steps {
                                echo "LCOV Coverage test"
                            }
                        }
                        stage('Compiling and publishing') {
                            steps {
                                script {
                                    dir("$PROJECT_ROOT/examples/TarsActDemo/QD.ActHttpServer/src") {
                                        echo "QD.ActHttpServer Compiling and publishing"
                                        sh "composer install -vvv"
                                        sh "composer run-script deploy"
                                        sh "ls *.tar.gz > tmp.log"
                                        def packageDeploy = sh(script: "head -n 1 tmp.log", returnStdout: true).trim()
                                        sh "curl -H 'Host:tars.qidian.local' -F 'suse=@./${packageDeploy}' -F 'application=${APP_NAME}' -F 'module_name=${SERVER_NAME}' -F 'comment=${env.TAG_DESC}' http://tars.qidian.local/pages/server/api/upload_patch_package > curl.log"
                                        def packageVer = sh(script: "jq '.data.id' curl.log", returnStdout: true).trim()
                                        def postJson = '{"serial":true,"items":[{"server_id":28,"command":"patch_tars","parameters":{"patch_id":' + packageVer + ',"bak_flag":false,"update_text":"${env.TAG_DESC}"}}]}'
                                        echo postJson
                                        sh "curl -H 'Host:tars.qidian.local' -H 'Content-Type:application/json' -X POST --data '${postJson}' http://tars.qidian.local/pages/server/api/add_task"
                                    }
                                }
                            }
                        }
                    }
                    post {
                        success {
                            emailext (
                                subject: "[jenkins] Construction notice：${env.JOB_NAME} brunch: ${env.BRANCH_NAME} - Build# ${env.BUILD_NUMBER} success  !",
                                body: '${SCRIPT, template="groovy-html.template"}',
                                mimeType: 'text/html',
                                to: "liujingfeng.a@yuewen.com",
                            )
                            cleanWs()
                        }
                        failure {
                            emailext (
                                subject: "[jenkins] Construction notice：${env.JOB_NAME} brunch: ${env.BRANCH_NAME} - Build# ${env.BUILD_NUMBER} fail !",
                                body: '${SCRIPT, template="groovy-html.template"}',
                                mimeType: 'text/html',
                                to: "liujingfeng.a@yuewen.com",
                            )
                            cleanWs()
                        }
                    }
                }
    ```
4. Execution Construction
    ![PNG](../image/build_with_parameters.png)
5. View build results (mail notification)
    ![PNG](../image/stage_view.png)
5. tars web
    ![PNG](../image/tars_web.png)