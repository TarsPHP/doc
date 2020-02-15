## 关于Tars协议和Tup协议的说明

## Notes on Tars agreement and Tup agreement

`TARS编码协议`、`TUP组包协议`、`TARS组包协议`三者之间的关系：
The relationship between `TARS encoding protocol`,` TUP packet protocol`, and `TARS packet protocol`:

- `TARS编码协议`是一种数据编解码规则，它将整形、枚举值、字符串、序列、字典、自定义结构体等数据类型按照一定的规则编码到二进制数据流中。对端接收到二进制数据流之后，按照相应的规则反序列化可得到原始数值。
- `TARS encoding protocol` is a data encoding and decoding rule, which encodes data types such as integer, enumeration values, strings, sequences, dictionaries, and custom structures into binary data streams according to certain rules. The reveicing end deserialize received data according to the certain rules to obtain the original value after accepting binary stream data.

- `TARS编码协议`使用一种叫做`TAG`的整型值(unsigned char)来标识变量，比如某个变量A的`TAG`值为100(该值由开发者自定义)，我们将变量值编码的同时，也将该`TAG`值编码进去。对端需要读取变量A的数值时，就到数据流中寻找TAG值为100的数据段，找到后按规则读出数据部分即是变量A的数值。
- `TARS encoding protocol` uses an integer value (unsigned char) named` TAG` to identify variables. For example, the `TAG` value of a variable A is 100 (the value is customized by developer). When encoding the value, the `TAG` value is also encoded. The receiving end looks for a data segment with a TAG value of 100 in the data stream, and reads out the data part according to the rule and then get the the value of the variable A.

- `TARS编码协议`的定位是一套编码规则。`tars`协议序列化之后的数据不仅可以进行网络传输，同时还可以存储到数据库中。
- `TARS encoding protocol` is an encoding rule. The data serialized based on the `tars encoding protocol` can both be transmitted over the network or stored in a database.

- `TUP组包协议`是TARS编码协议的上层封装，定位为通信协议。它使用变量名作为变量的关键字，编码时，客户端将变量名打包到数据流中；解码时，对端根据变量名寻找对应的数据区，然后根据数据类型对该数据区进行反序列化得到原始数值。
- `TUP packet protocol` is a communication protocol,which is an upper-layer encapsulation of the TARS encoding protocol. It uses the variable name as the key of the variable.The client packs the variable name into the data stream while encoding, the receiving end finds the corresponding data area according to the variable name, and then deserializes the data area according to the data type.

- `TUP组包协议`内置一个TARS编码协议的Map类型，该Map的键值是变量名，Map的值是将变量的数据值经过TARS编码序列化的二进制数据。
- `TUP packet protocol` has a built-in Map based on TARS encoding protocol. The key of the map is the variable's name, and the value of the map is the binary data of the variable's value which serialized through the TARS encoding.

- `TUP组包协议`封装的数据包可以直接发送给`Tars`服务端，而服务端可以直接反序列化得到原始值。
- Packets encapsulated by `TUP packet protocol` can be sent to Tars server directly, and the server can get the original value through deserialize.

- `TARS组包协议`是对RequestPacket（请求结构体）和ResponsePacket（结果结构体）使用TARS编码协议封装的通信协议。结构体包含比如请求序列号、协议类型、RPC参数序列化之后二进制数据等重要信息。
- `TARS packet protocol` is a communication protocol, which package RequestPacket (request structure) and ResponsePacket (result structure) based on the `TARS encoding protocol`. The structures contain important information include the request sequence number, protocol type, and binary data of serialized RPC parameter.

