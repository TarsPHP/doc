## Notes on Tars agreement and Tup agreement

The relationship between `TARS encoding protocol`,` TUP packet protocol`, and `TARS packet protocol`:

- `TARS encoding protocol` is a data encoding and decoding rule, which encodes data types such as integer, enumeration values, strings, sequences, dictionaries, and custom structures into binary data streams according to certain rules. The reveicing end deserialize received data according to the certain rules to obtain the original value after accepting binary stream data.

- `TARS encoding protocol` uses an integer value (unsigned char) named` TAG` to identify variables. For example, the `TAG` value of a variable A is 100 (the value is customized by developer). When encoding the value, the `TAG` value is also encoded. The receiving end looks for a data segment with a TAG value of 100 in the data stream, and reads out the data part according to the rule and then get the the value of the variable A.

- `TARS encoding protocol` is an encoding rule. The data serialized based on the `tars encoding protocol` can both be transmitted over the network or stored in a database.

- `TUP packet protocol` is a communication protocol,which is an upper-layer encapsulation of the TARS encoding protocol. It uses the variable name as the key of the variable.The client packs the variable name into the data stream while encoding, the receiving end finds the corresponding data area according to the variable name, and then deserializes the data area according to the data type.

- `TUP packet protocol` has a built-in Map based on TARS encoding protocol. The key of the map is the variable's name, and the value of the map is the binary data of the variable's value which serialized through the TARS encoding.

- Packets encapsulated by `TUP packet protocol` can be sent to Tars server directly, and the server can get the original value through deserialize.

- `TARS packet protocol` is a communication protocol, which package RequestPacket (request structure) and ResponsePacket (result structure) based on the `TARS encoding protocol`. The structures contain important information include the request sequence number, protocol type, and binary data of serialized RPC parameter.

