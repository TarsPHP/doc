# TARS-TIMER-SERVER Description

## Service Deployment Guideline

1. Enter Operations Management => Template Management

The platform will provide a new template for PHP, named tars.tarsphp.default, !!!!!!! You must first modify the PHP execution path !!!!!!!
 
Template related modifications:
     a. Create a new tars.tarsphp.timer service template, compared to tars.tarsphp.default and add an isTimer = 1. The PHP service selects the tars.tarsphp.timer template.
     b. Or use the tars.tarsphp.default template and add the following to the private template under the editing service:

```xml
<tars>
 <application>
   <server>
      isTimer=1
    </server>
 </application>
</tars>
```


2. Enter Operations Management => Deployment Services on the platform, fill in the corresponding application name and service name, note: that this is the same as tars.proto.php in the tars folder below
Need to be completely consistent.

3. Select the service type as tars_php

4. Select the template as the timer service template just created, set is not enabled by default

5. Select an available port and fill in the server's intranet IP

6. The port type is TCP
!!!! Protocol type TIMER service must choose non-TARS !!!!!!

7. The number of threads corresponds to the number of SWOOLE processes, that is, the number of timer services that can be started under the timer directory

8. The maximum number of connections, the maximum queue length, and the queue timeout period are not effective for the PHP service.

9. Click Add and Submit, then please enter the development guidline

## Develop Guidline 

 1. Example of a scheduled task:
  ./src/timer/TenSecondsTimer.php

```php
<?php
namespace TimerServer\timer;
class TenSecondsTimer {
    public $interval;
    public function __construct()
    {
        $this->interval = 10000; //Unit is millisecond
    }
    public function execute() {
        // Your business code
    }
}
```

 2. Create a new tars.proto.php file under the tars folder, which needs to include a description of your service itself:
   ```php
   <?php
   return array(
       'appName' => 'PHPTest',
       'serverName' => 'PHPTimerServer',
       'objName' => 'obj',
   );
   ```
   This name must correspond exactly to the name on the tars platform.

 3. Attention !!!!
   Each php file in the timer directory is bound to a worker process, so the number of workers must be not less than the number of timer tasks.
   You can modify the number of threads in the tars management platform Servant management (for php, it is the number of swoole workers).