# tars-server document

## Introduction
**Tars-server** is a basic framework for implementing tars services in PHP. It mainly includes service start and stop, route analysis, protocol processing, and event reporting.

## How to use  
**Tars-server** uses *composer* for package management. Developers only need to perform `composer install` according to the corresponding version.

For specific usage, please refer to *http-server*, *timer-server*, and *tcp-server* under [examples](https://github.com/TarsPHP/TarsPHP/tree/master/examples). 

## Framework description
**Tars-server** is implemented with **swoole** as the underlying network transceiver.The framework mainly includes the following directories:

* cmd: Responsible for the implementation of the framework's start and stop commands  
* core: Core implementation of the framework * protocol: responsible for protocol processing 
  
### cmd
For the cmd part, contains the following files:
1. `Command.php` 

   Responsible for specifying the configuration file and startup command when the service starts

2. `CommandBase.php` 

   Specifies the required implementation of a Command. All such as *START* are subclasses of *CommandBase*. It provides the getProcess method to get the currently started service process.  

3. `Restart.php` 

   Restart command, just call stop and then call start

4. `Start.php` 

  Startup command will first analyze the configuration issued by the platform and then introduce the *services.php* file necessary for the business.  

5. `Stop.php` 
  The current service stop method is violent. It will pull out all processes according to the service name, and then kill it. Reloading will be introduced later to reload the service code.


### core  
The core layer is mainly composed of `Event`,` Server`, `Request`, and` Response`.  

#### `Server.php`    
 Responsible for the initialization work before the service starts, including:
   * Determine if it is tcp or http, so register the corresponding callback and start the corresponding server
   * If it is a timer, it will start the timer scan of the corresponding directory
   * Pass the configuration of swoole 
   * Register general callback function  
   * Pass the server's *swooletable*  
   * Specify the startup file for the entire framework and force require  
   * Specify the protocol processing method of the framework, whether it is tars or http

After completing the startup of the service, you will enter    
* onMasterStart
    - Write the name of the process 
    - Write pid to file  
    - Initial report of service  
* onManagerStart
    - Rename process  
* onWorkerStart  
    - If it is tcp type, converting the comments in the interface to php data first, which is convenient for processing when routing.  
    - If it is http, specify the corresponding namespace  
    - Set the name of worker  
    - If it is a timer, start the corresponding timer  
    - When workerId = 0 (guaranteed to be triggered only once), submit the keepalive report task of the service to task worker
* onTask
    - Heartbeat report
    - Stat report
    - Feature report  
- onReceive/onRequest  
    + For tcp server, follow `onReceive`
        *  Initialize the Request object, pass the sw object into the super global variable **$ _SERVER**  
        * Set protocol to TARSProtocol
        * Perform protocol processing and return the package  
        * Clear global variables
    
    + For http server, follow `onRequest`
        * Handle parameters for cookies, get, and post  
        * Initialize the Request object, pass the sw object into the super global variable **$ _SERVER**  
        * Perform protocol processing and return the package  
        * Clear global variables  

#### Event.php    
onReceive method:  
   * TCP requests will first enter the route method of TARSProtocol for routing  
   * After routing, make the actual function call  
   * Pack back  
   * Send back package  

onRequest method:  
   * Provide a default detection interface
   * Perform basic routing protocol analysis 
   * Call the corresponding controller method  
   * Send back package  

#### Request.php  
- Store some necessary request data  
- Set and remove global variables  
  
#### Response.php  
Responsible for send back package 


### Service startup process  
The service startup is initiated by Start cmd, After the creation of the Server object is called, Then perform the initialization of swoole in turn, After starting the service, you only need to handle the monitoring of onReceive or onRequest. 

## dependency  
* phptars/tars-client: Make a call to the tars service  
* phptars/tars-report: Report the running status of the service itself  
* phptars/tars-config: Pull the configuration uploaded by the platform