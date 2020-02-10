## 关于Tars协议和Tup协议的说明

## Notes on Tars agreement and Tup agreement

`TARS编码协议`、`TUP组包协议`、`TARS组包协议`三者之间的关系：
The relationship between `TARS encoding protocol`,` TUP packet protocol`, and `TARS packet protocol`:

- `TARS`编码协议是一种数据编解码规则，它将整形、枚举值、字符串、序列、字典、自定义结构体等数据类型按照一定的规则编码到二进制数据流中。对端接收到二进制数据流之后，按照相应的规则反序列化可得到原始数值。
-`TARS` encoding protocol is a data encoding and decoding rule, which encodes data types such as shaping, enumeration values, strings, sequences, dictionaries, and custom structures into binary data streams according to certain rules. After the peer end receives the binary data stream, deserialize it according to the corresponding rules to obtain the original value.

- `TARS`编码协议使用一种叫做`TAG`的整型值(unsigned char)来标识变量，比如某个变量A的`TAG`值为100(该值由开发者自定义)，我们将变量值编码的同时，也将该`TAG`值编码进去。对端需要读取变量A的数值时，就到数据流中寻找TAG值为100的数据段，找到后按规则读出数据部分即是变量A的数值。
-The `TARS` encoding protocol uses an integer value (unsigned char) called` TAG` to identify variables. For example, the `TAG` value of a variable A is 100 (the value is customized by the developer). When encoding the value, the `TAG` value is also encoded. When the opposite end needs to read the value of the variable A, it looks for a data segment with a TAG value of 100 in the data stream, and after finding it, reads out the data part according to the rule is the value of the variable A.

- `TARS`编码协议的定位是一套编码规则。`tars`协议序列化之后的数据不仅可以进行网络传输，同时还可以存储到数据库中。
-The positioning of the `TARS` encoding protocol is a set of encoding rules. The data serialized by the `tars` protocol can not only be transmitted over the network, but also stored in a database.

- `TUP`组包协议是TARS编码协议的上层封装，定位为通信协议。它使用变量名作为变量的关键字，编码时，客户端将变量名打包到数据流中；解码时，对端根据变量名寻找对应的数据区，然后根据数据类型对该数据区进行反序列化得到原始数值。
-The `TUP` packet protocol is an upper-layer encapsulation of the TARS encoding protocol and is positioned as a communication protocol. It uses the variable name as the keyword of the variable. When encoding, the client packs the variable name into the data stream; when decoding, the peer finds the corresponding data area according to the variable name, and then deserializes the data area according to the data type Get the original value.

- `TUP`组包协议内置一个TARS编码协议的Map类型，该Map的关键字就是变量名，Map的值是将变量的数据值经过TARS编码序列化的二进制数据。
-The `TUP` packet protocol has a built-in map type of the TARS encoding protocol. The key of the map is the variable name, and the value of the map is the binary data serialized by the data value of the variable through the TARS encoding.

- `TUP`组包协议封装的数据包可以直接发送给`Tars`服务端，而服务端可以直接反序列化得到原始值。
-Packets encapsulated by the `TUP` packet protocol can be sent directly to the` Tars` server, and the server can directly deserialize to get the original value.

- `TARS`组包协议是对RequestPacket（请求结构体）和ResponsePacket（结果结构体）使用TARS编码协议封装的通信协议。结构体包含比如请求序列号、协议类型、RPC参数序列化之后二进制数据等重要信息。
-The `TARS` packet protocol is a communication protocol encapsulated by the RequestPacket (request structure) and ResponsePacket (result structure) using the TARS encoding protocol. The structure contains important information such as the request sequence number, protocol type, and binary data after RPC parameter serialization.

