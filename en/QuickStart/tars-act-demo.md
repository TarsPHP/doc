# What is this

This is a bullet activity project deployed with Tars, which contains 3 services.
Covers Http, Tcp services, uses Mysql, Redis storage, including user login, session verification, user information acquisition, bullet creation, bullet acquisition and other functions. Many aspects of normal business development have been covered.

QD.ActHttpServer.obj is an http service that provides a fixed port to the outside. This is a business service. It calls the back-end User and Comment services to complete the business.
QD.ActCommentServer.CommentObj is a tcp service, which mainly provides the writing and obtaining of comments and is stored in Redis.
QD.UserService.UserObj is a TCP, which mainly provides user information acquisition, user login, and user session verification. Relevant data is saved in Mysql.

index.html is a front-end page that connects to the http service to achieve a barrage display.
act.tars.local.conf is the nginx configuration
user_info.sql is the user information table for QD.UserServer service


# How to deploy

1. Need to create the db configuration on the tars platform. On the tars platform, find QD.UserService and click on service configuration, add configuration, file name db.json, content:

~~~json
[
  {
    "host": "mysql.tarsActDemo.local", //This is your mysql address
    "port": 3306,
    "username": "root",
    "password": "password",
    "db": "tars_test",
    "charset": "utf-8",
    "instanceName": "default"
  }
]
~~~

2. Import user_info.sql into your mysql.

3. Modify the redis configuration in src/ENVConf.php in QD.ActCommentServer and set your own redis ip and port

4. In turn cd to the src directory of each service, execute composer install, and install composer dependencies

5. In turn cd to the src directory of each service and execute composer run-script deploy to package the code

6. Deploy three services, QD.ActHttpServer is an http service, you need to fix a port, choose non-tars service, the other two are tcp services, the port is arbitrary, choose tars service.
(Note that the ServantName configured on the platform during deployment must be the same as the full name I wrote above. If you need to modify it, please modify tars.proto.php, actComment.proto.php and userInfo.proto.php and generate a new protocol (Medium code)
7. Configure your nginx and proxy_pass to the IP address and port of your QD.ActHttpServer service

8. Copy index.html to your /data/website/tarsact/

9. Open act.tars.local to see if it is accessible

10. Enter a username yong password 123456 and perform the following login

11. Try a barrage

12. Refresh, has the barrage you sent out? Congratulations O(∩_∩)O 


# Multiple servant support
1. Tars-server 0.3 or higher supports multiple servants, which can achieve mixed deployment of tars and http protocols
2. The QD.UserService service has implemented an example of mixed deployment of tars and http protocols. According to the instructions above, you can deploy the tars protocol first.
3. Find the QD.UserService service in the management interface, click Manage servant, and click Add.
4. obj name HttpObj, arbitrary number of threads, binding address tcp -h {your ip} -t 60000 -p {http service port} -e 0 Select non-tars
5. Restart the service after saving, try to visit http://{your ip}:{http service port}/user/index to return hello word
6. Can access Try to access http://{your ip}:{http service port}/user/ getUsersInfoByIds?ids=8001, Test your service logic
7. Usually the tars protocol is provided for business calls, and the http protocol is used for testing. The business logic is recommended to be encapsulated into the service layer uniformly, with http and tars as two entrances

# Other

1. There is no logout function, if you need to logout, you can clear the cookie
2. The page is a bit ugly
3. tars file in the tars directory of each service