# tars-extension document

## Intrudoction
`tars-extension` is a php extension for high-performance tars package unpacking and tup encoding and decoding implemented in c, which is the basic dependency of the entire phptars.

## Instruction
Like other php extensions, enter the extension directory to execute：
```bash
phpize
./configure
make
make test
make install
```
Modify the `php.ini` file to load the .so file. Use the `php -m` command to see if the extension is successfully installed.


## Extension description
`tars-extension` extension does three main things:

* All data structures of tars are mapped with extended types
* Three complex types of tars are mapped with special extension types
* Provides the ability to package, unpack, encode and decode tup and tars protocols

### Mapping of basic types
```php
    bool => \TARS::BOOL
    char => \TARS::CHAR
    uint8 => \TARS::UINT8
    short => \TARS::SHORT
    uint16 => \TARS::UINT16
    float => \TARS::FLOAT
    double => \TARS::DOUBLE
    int32 => \TARS::INT32
    uint32 => \TARS::UINT32
    int64 => \TARS::INT64
    string => \TARS::STRING
    vector => \TARS::VECTOR
    map => \TARS::MAP
    struct => \TARS::STRUCT
```
When we need to identify specific variable types, we need to use these basic types, these types are constants, from 1-14.

### Mapping of complex types
For the three basic types of vector, map, and struct, there are some special packaging and unpacking mechanisms, so special data types need to be introduced:

#### vector:

```php
    //vector => \TARS_VECTOR
    //It has both member functions pushBack () and push_back ()
    //The input parameter depends on what type of the vector contains

	//Example：
    $shorts = ["test1","test2"];
    $vector = new \TARS_VECTOR(\TARS::STRING); //Define a vector of type string
    foreach ($shorts as $short) {
        $vector->pushBack($short); //Put test1 and test2 into the vector
    }
```
#### map:

```php
    //map => \TARS_MAP
    //It has both member functions pushBack () and push_back ()
    //The input parameter depends on what type of the map contains

    //Example：
    $strings = [["test1"=>1],["test2"=>2]];
    $map = new \TARS_MAP(\TARS::STRING,\TARS::INT64); //Define a map with key as string and value as int64
    foreach ($strings as $string) {
        $map->pushBack($string); //Push the two elements into the map in turn. Note that pushBack receives an array, and the array has only one element.
    }
```

#### struct:

```php
    //struct => \TARS_Struct
    //The struct constructor is special, receiving two parameters of classname and classfields
    //The first describes the name, and the second describes the variables in the struct.

    //Example：
	class SimpleStruct extends \TARS_Struct {
		const ID = 0; //The tag of each struct in the tars file
		const COUNT = 1;

		public $id; //The value of each element in strcut is saved here
		public $count; 

		protected static $fields = array(
			self::ID => array(
				'name'=>'id',//the name of each element in the struct
				'required'=>true,//Whether each element in the struct is required, corresponding to require and optional in the tars file
				'type'=>\TARS::INT64,//the type of each element in the struct
				),
			self::COUNT => array(
				'name'=>'count',
				'required'=>true,
				'type'=>\TARS::UINT8,
				),
		);

		public function __construct() {
			parent::__construct('App_Server_Servant.SimpleStruct', self::$fields);
		}
	}
   
```

### Packing and encoding
As the core function of the extension, it is the ability to provide tars encoding and decoding and packaging and unpacking:

#### Packing
```php
// Package and unpack methods for basic types, output binary buf
// iVersion has only 1 and 3 versions. In version 1, $nameOrTagNum needs to pass in tagNum. The first parameter in the method is 1 and the second parameter is 2 and so on.In version 3, $nameOrTagNum needs to be passed in name and parameter name
$buf = \TASAPI::put*($nameOrTagNum, $value, $iVersion = 3)
$value = \TUPAPI::get*($nameOrTagNum, $buf, $isRequire = false, $iVersion = 3)

// For struct, transfer the object, when returning the result, return it as an array, whose elements correspond to the member variables of the class one by one
$buf = \TUPAPI::putStruct($nameOrTagNum, $clazz, $iVersion = 3)
$result = \TUPAPI::getStruct($nameOrTagNum, $clazz, $buf, $isRequire = false, $iVersion = 3)

// For the vector, pass in the vector that completes pushBack
$buf = \TUPAPI::putVector($nameOrTagNum, TARS_Vector $clazz, $iVersion = 3)
$value = \TUPAPI::getVector($nameOrTagNum, TARS_Vector $clazz, $buf, $isRequire = false, $iVersion = 3)

// For map, pass in the map that completes pushBack
$buf = \TUPAPI::putMap($nameOrTagNum, TARS_Map $clazz, $iVersion = 3)
$value = \TUPAPI::getMap($nameOrTagNum, TARS_Map $clazz, $buf, $isRequire = false, $iVersion = 3)

// Need to put the above packed data together for encoding
$inbuf_arr[$nameOrTagNum] = $buf
```
#### 编码解码
```php
// For the case of the tup protocol (iVersion = 3):
// In this case, the client sends the packet using encode function encoding, the server receives the packet using decode function decoding, the server returns the packet using encode function encoding, the client receives the packet using decode function decoding, and encodes the tup protocol. The returned results can be used for transmission and persistence.
$reqBuffer = \TUPAPI::encode(
                         $iVersion = 3,
                         $iRequestId,
                         $servantName,
                         $funcName,
                         $cPacketType=0,
                         $iMessageType=0,
                         $iTimeout,
                         $context=[],
                         $statuses=[],
                         $bufs)
// Decode with the TUP protocol
$ret = \TUPAPI::decode($respBuffer, $iVersion = 3)
$code = $ret['iRet']
$buf = $ret['sBuffer']

//For the tars protocol (iVersion = 1):
//In this case, the client sends the packet using encode function, the server receives the packet using decodeReqPacket function, the server returns the packet using encodeRspPacket function, and the client receives the packet using decode function.

//Client sends packets
$reqBuffer = \TUPAPI::encode(
                         $iVersion = 1,
                         $iRequestId,
                         $servantName,
                         $funcName,
                         $cPacketType=0,
                         $iMessageType=0,
                         $iTimeout,
                         $context=[],
                         $statuses=[],
                         $bufs)
//Server receives and unpacks
$ret = \TUPAPI::decodeReqPacket($respBuffer)
$code = $ret['iRet']
$buf = $ret['sBuffer']

//Server back package encodeRspPacket
$reqBuffer = \TUPAPI::encodeRspPacket(
                         $iVersion = 1,
                         $cPacketType,
                         $iMessageType,
                         $iRequestid,
                         $iRet=0,
                         $sResultDesc='',
                         $bufs,
                         $statuses=[])
                         
//Client receives and unpacks
$ret = \TUPAPI::decode($respBuffer, $iVersion = 1)
$code = $ret['iRet']
$buf = $ret['sBuffer']
```

For a richer use of packing and unpacking of different types of structures, please refer to **tests/**

### tars2php (Automatically generate php class tools) Instructions for use
Refer to the tars2php module:
[Detailed description](./tars2php.md)

### Test case

#### phpunit
For common use of extensions, add test cases, located under `/ext/testcases` folder,
You only need to execute `php phpunit-4.8.36.phar test.php` when testing to complete the execution of all test cases. Which covered:

* All basic types of unpacking and coding tests
* Simple struct type package unpacking and encoding test
* Packaging and unpacking and encoding tests for simple vector types
* Simple map type unpacking and encoding test
* Packaging and encoding tests for complex vector types (including non-primitive data types)
* Packaging and encoding tests for complex map types (including non-primitive data types)
* Packaging and encoding tests for complex struct types (nested vectors and maps)

In addition, `testTARSClient.php` and `testTARSServer.php` are test cases where the client sends packets, the server unpacks and the server returns packets, and the client unpacks under the tars protocol (iVersion = 1).

Note that you need to download the phpunit executable file yourself, or use the pre-installed phpunit tool directly for unit testing.

#### phpt
After installing the extension, run `make test`