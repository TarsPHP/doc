# tars-deploy document

## Introduction

**Tars-deploy** is used to package the project and facilitate the distribution of tool components on the tars management platform.

## Instructions

1. Dependence
   - Phar extension
2. Make sure the `./tars/tars.proto.php` file under the project exists and contains the following

```php
return array(
    'appName' => 'APPNAME', //app名称 如tars.tarsconfig中的tars
    'serverName' => 'SERVERNAME', //服务名 如tars.tarsconfig中的tarsconfig
    //...
);
```

3. Introduce **tars-deploy** to the project's composer.json

4. Add the following to the project composer.json

```json
"scripts": {
    "deploy": "\\Tars\\deploy\\Deploy::run"
}
```

5. Execute the command `composer run-script deploy` in the project directory to generate a compressed package of the project to be uploaded