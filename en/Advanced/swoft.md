
# Introduction
swoft in tars
* Support Packing code
* Support tars register

# Related

* SwoftInTars (https://github.com/dpp2009/swoftInTars)
* SWOFT (https://github.com/swoft-cloud/swoft)   
* TARSPHP (https://github.com/TarsPHP)
* TARSPHP DOCKER (https://github.com/tangramor/docker-tars)


# Use

* Clone (https://github.com/dpp2009/swoftInTars)
* Install `composer install`
* Packing `composer run-script deploy`
* Upload tars tgz to tars web

# Configuration description

* server name write correct in ./tars/tars.proto.php （appName&serverName） & ./src/.env （PNAME） 
* swoft's prot do not use tarsweb's port


# resta
* use docker-tars，pcre may not open。run php --ri swoole，check pcre => enabled。(If pcre not open,run yum install pcre-devel，rebuild swoole)
