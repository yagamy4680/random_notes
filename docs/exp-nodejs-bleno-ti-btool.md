# Experiment with TI BTool

## Write Characteristic

Here are the nodejs sample codes with [bleno](https://github.com/sandeepmistry/bleno)

```javascript
uuid = 'FD92'
desc = new Descriptor uuid: '2901', value: "write FD92"
writeCB = (data, offset, withoutResponse, callback) -->
  console.log "BLE write field `FD92`"
  console.log "value: #{data.toString 'hex'}, offset: #{offset}, withoutResponse: #{withoutResponse}"
  callback Characteristic.RESULT_SUCCESS

writeChar = new Characteristic \
  uuid: uuid, \
  properties: ['write', 'writeWithoutResponse'], \
  descriptors: [desc], \
  onWriteRequest: writeCB
  
ble.characteristics.push writeChar
console.log "register #{uuid} for writing GATT"
```

Here are testing steps:

0. Open TI BTool (Bluetooth Low Energy PC Application - v1.40.5)
1. connect to my peri
2. use `Discover Characteristic by UUID`
3. input uuid `92:FD`. Note, the nodejs app registers `0xFD92`, but please use inverse string
4. click `Read` button.
5. then, the `characteristic value handle` will be discovered. => it shall be `0x005B`
6. use "Characteristic Write"
7. input value handle `0x005B`
8. input value as ASCII `aabbcc`
9. click `Write` button, then successful write

At step4, here are the outputs of BTool:

```text
[156] : <Tx> - 08:49:53.764
-Type		: 0x01 (Command)
-Opcode		: 0xFD88 (GATT_DiscCharsByUUID)
-Data Length	: 0x08 (8) byte(s)
 ConnHandle	: 0x0001 (1)
 StartHandle	: 0x0001 (1)
 EndHandle	: 0xFFFF (65535)
 Type		: 92:FD
Dump(Tx):
01 88 FD 08 01 00 01 00 FF FF 92 FD 
---------------------------------------------------------------------------------
[157] : <Rx> - 08:49:53.776
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x067F (GAP_HCI_ExtentionCommandStatus)
 Status		: 0x00 (Success)
 OpCode		: 0xFD88 (GATT_DiscCharsByUUID)
 DataLength	: 0x00 (0)
Dump(Rx):
04 FF 06 7F 06 00 88 FD 00 
---------------------------------------------------------------------------------
[158] : <Rx> - 08:50:02.146
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x0E (14) bytes(s)
 Event		: 0x0509 (ATT_ReadByTypeRsp)
 Status		: 0x00 (Success)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x08 (8)
 Length		: 0x07 (7)
 Handle		: 0x005B
 Data		: 0C:5C:00:92:FD
Dump(Rx):
04 FF 0E 09 05 00 01 00 08 07 5B 00 0C 5C 00 92 
FD 
---------------------------------------------------------------------------------
[159] : <Rx> - 08:50:02.436
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x0509 (ATT_ReadByTypeRsp)
 Status		: 0x1A (The Procedure Is Completed)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x00 (0)
Dump(Rx):
04 FF 06 09 05 1A 01 00 00 
```

At step9, here are the outputs of BTool:

```text
-----------------------------------------------------------------------------------
[160] : <Tx> - 08:52:13.772
-Type		: 0x01 (Command)
-Opcode		: 0xFD92 (GATT_WriteCharValue)
-Data Length	: 0x0C (12) byte(s)
 ConnHandle	: 0x0001 (1)
 Handle		: 0x005B (91)
 Value		: 61:61:62:62:63:63:64:64
Dump(Tx):
01 92 FD 0C 01 00 5B 00 61 61 62 62 63 63 64 64 

-----------------------------------------------------------------------------------
[161] : <Rx> - 08:52:13.784
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x067F (GAP_HCI_ExtentionCommandStatus)
 Status		: 0x00 (Success)
 OpCode		: 0xFD92 (GATT_WriteCharValue)
 DataLength	: 0x00 (0)
Dump(Rx):
04 FF 06 7F 06 00 92 FD 00 
-----------------------------------------------------------------------------------
[162] : <Rx> - 08:52:14.044
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x0513 (ATT_WriteRsp)
 Status		: 0x00 (Success)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x00 (0)
Dump(Rx):
04 FF 06 13 05 00 01 00 00 
-----------------------------------------------------------------------------------
```

And the output from Nodejs application:

```text
BLE write field `FD92`:
value: 6161626263636464, offset: 0, withoutResponse: false
```


## Read Characteristic with Static Value

Here are the nodejs sample codes with [bleno](https://github.com/sandeepmistry/bleno)

```javascript
uuid = '0102'
desc = new Descriptor uuid: '2901', value: "read test2"
readChar = new Characteristic \
  uuid: uuid, \
  properties: ['read'], \
  descriptors: [desc], \
  value: new Buffer "AA"
ble.characteristics.push readChar
console.log "register #{uuid} for reading test"
```

Here are testing steps:

0. Open TI BTool (Bluetooth Low Energy PC Application - v1.40.5)
1. connect to my peri
2. use `Read Using Characteristic UUID`
3. input uuid `02:01`. Note, the nodejs app registers `0x0102`, but please use inverse string
4. click `Read` button.
5. the value outputs `AA`, same as the sample codes

Here are the outputs of BTool:

```text
[212] : <Tx> - 10:08:09.668
-Type		: 0x01 (Command)
-Opcode		: 0xFDB4 (GATT_ReadUsingCharUUID)
-Data Length	: 0x08 (8) byte(s)
 ConnHandle	: 0x0001 (1)
 StartHandle	: 0x0001 (1)
 EndHandle	: 0xFFFF (65535)
 Type		: 02:01
Dump(Tx):
01 B4 FD 08 01 00 01 00 FF FF 02 01 
---------------------------------------------------------------------------------
[213] : <Rx> - 10:08:09.679
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x067F (GAP_HCI_ExtentionCommandStatus)
 Status		: 0x00 (Success)
 OpCode		: 0xFDB4 (GATT_ReadUsingCharUUID)
 DataLength	: 0x00 (0)
Dump(Rx):
04 FF 06 7F 06 00 B4 FD 00 
---------------------------------------------------------------------------------
[214] : <Rx> - 10:08:09.950
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x0B (11) bytes(s)
 Event		: 0x0509 (ATT_ReadByTypeRsp)
 Status		: 0x00 (Success)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x05 (5)
 Length		: 0x04 (4)
 Handle		: 0x0062
 Data		: 41:41
Dump(Rx):
04 FF 0B 09 05 00 01 00 05 04 62 00 41 41 
---------------------------------------------------------------------------------
[215] : <Rx> - 10:08:10.250
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x0509 (ATT_ReadByTypeRsp)
 Status		: 0x1A (The Procedure Is Completed)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x00 (0)
Dump(Rx):
04 FF 06 09 05 1A 01 00 00 
---------------------------------------------------------------------------------
```



## Read Characteristic with Dynamic Value

Here are the nodejs sample codes with [bleno](https://github.com/sandeepmistry/bleno)

```javascript
readCB = (offset, cb) ->
  console.log "BLE read field `0103`"
  data = new Buffer("aa")
  if offset > data.length
    return cb Characteristic.RESULT_INVALID_OFFSET, null
  else
    return cb Characteristic.RESULT_SUCCESS, data

uuid = '0103'
desc = new Descriptor uuid: '2901', value: "read test"
readChar = new Characteristic \
  uuid: uuid, \
  properties: ['read'], \
  descriptors: [desc], \
  onReadRequest: readCB
ble.characteristics.push readChar
  
console.log "register #{uuid} for reading test"
```

Here are testing steps:

0. Open TI BTool (Bluetooth Low Energy PC Application - v1.40.5)
1. connect to my peri
2. use `Discover Characteristic by UUID`
3. input uuid `03:01`. Note, the nodejs app registers `0x0103`, but please use inverse string
4. click `Read` button.
5. then, the `characteristic value handle` will be discovered. => it shall be `0x005E`.
6. use `Read Characteristic Value / Descriptor`
7. input value handle `0x005F` (Note, the discovered value handle is `0x005E`, but the actual read handle is the next one `0x005F`
8. select value type as `ASCII`
9. click `Read` button, then the data `AA` is read.


Here are the outputs of BTool for step4. You can find the packet [3] mentions the actual read handle is `5F 00` => `0x005F`.

```text
[1] : <Tx> - 11:21:58.165
-Type		: 0x01 (Command)
-Opcode		: 0xFD88 (GATT_DiscCharsByUUID)
-Data Length	: 0x08 (8) byte(s)
 ConnHandle	: 0x0001 (1)
 StartHandle	: 0x0001 (1)
 EndHandle	: 0xFFFF (65535)
 Type		: 03:01
Dump(Tx):
01 88 FD 08 01 00 01 00 FF FF 03 01 
--------------------------------------------------------------------------------------
[2] : <Rx> - 11:21:58.174
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x067F (GAP_HCI_ExtentionCommandStatus)
 Status		: 0x00 (Success)
 OpCode		: 0xFD88 (GATT_DiscCharsByUUID)
 DataLength	: 0x00 (0)
Dump(Rx):
04 FF 06 7F 06 00 88 FD 00 
--------------------------------------------------------------------------------------
[3] : <Rx> - 11:22:06.644
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x0E (14) bytes(s)
 Event		: 0x0509 (ATT_ReadByTypeRsp)
 Status		: 0x00 (Success)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x08 (8)
 Length		: 0x07 (7)
 Handle		: 0x005E
 Data		: 02:5F:00:03:01
Dump(Rx):
04 FF 0E 09 05 00 01 00 08 07 5E 00 02 5F 00 03 
01 
--------------------------------------------------------------------------------------
[4] : <Rx> - 11:22:06.944
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x0509 (ATT_ReadByTypeRsp)
 Status		: 0x1A (The Procedure Is Completed)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x00 (0)
Dump(Rx):
04 FF 06 09 05 1A 01 00 00 
--------------------------------------------------------------------------------------
```



Here are the outputs of BTool for step9:

```text
[1] : <Tx> - 11:22:54.661
-Type		: 0x01 (Command)
-Opcode		: 0xFD8A (GATT_ReadCharValue)
-Data Length	: 0x04 (4) byte(s)
 ConnHandle	: 0x0001 (1)
 Handle		: 0x005F (95)
Dump(Tx):
01 8A FD 04 01 00 5F 00 
--------------------------------------------------------------------------------------------
[2] : <Rx> - 11:22:54.674
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x06 (6) bytes(s)
 Event		: 0x067F (GAP_HCI_ExtentionCommandStatus)
 Status		: 0x00 (Success)
 OpCode		: 0xFD8A (GATT_ReadCharValue)
 DataLength	: 0x00 (0)
Dump(Rx):
04 FF 06 7F 06 00 8A FD 00 
--------------------------------------------------------------------------------------------
[3] : <Rx> - 11:22:54.944
-Type		: 0x04 (Event)
-EventCode	: 0xFF (HCI_LE_ExtEvent)
-Data Length	: 0x08 (8) bytes(s)
 Event		: 0x050B (ATT_ReadRsp)
 Status		: 0x00 (Success)
 ConnHandle	: 0x0001 (1)
 PduLen		: 0x02 (2)
 Value		: 61 61 
Dump(Rx):
04 FF 08 0B 05 00 01 00 02 61 61 
--------------------------------------------------------------------------------------------
```

Here are the output from nodejs app:

```text
  l2cap-ble buffer = "data 0a5f00\n" +48s
  l2cap-ble line = data 0a5f00 +0ms
  l2cap-ble handing request: 0a5f00 +0ms
BLE read field `0103`
  l2cap-ble read response: 0b6161 +1ms
  l2cap-ble send: 0b6161 +0ms
```

Note, I run the nodejs app `DEBUG=descriptor,primary-service,bindings,hci-ble,l2cap-ble ./peri` to enable debug output from [bleno](https://github.com/sandeepmistry/bleno) module.


## Misc

### Maximum Data for Writing Characteristic

BTool allows to send `20` ascii characters to nodejs app. Sending more than `20` ascii characters will fail with error `The Attribute PDU is invalid`.

When sending `12345678901234567890` ascii characters to nodejs app, here are nodejs output:

```text
  l2cap-ble buffer = "data 125b003132333435363738393031323334353637383930\n" +36s
  l2cap-ble line = data 125b003132333435363738393031323334353637383930 +0ms
  l2cap-ble handing request: 125b003132333435363738393031323334353637383930 +0ms
BLE write field `0104`: value: 0x31,0x32,0x33,0x34,0x35,0x36,0x37,0x38,0x39,0x30,0x31,0x32,0x33,0x34,0x35,0x36,0x37,0x38,0x39,0x30, offset: 0, withoutResponse: false
text = 12345678901234567890
  l2cap-ble write response: 13 +2ms
  l2cap-ble send: 13 +0ms
```

### Maximum Data for Reading Characteristic

[bleno](https://github.com/sandeepmistry/bleno) module only allows to send `22` ascii characters to BTool when reading characteristic.

When I try to send out 40 ascii characters as following sample code:

```javascript
readCB = (offset, cb) ->
  console.log "BLE read field `0103`"
  data = new Buffer("1234567890123456789012345678901234567890")
  if offset > data.length
    return cb Characteristic.RESULT_INVALID_OFFSET, null
  else
    return cb Characteristic.RESULT_SUCCESS, data
```

Only 22 ascii characters are sent to BTool. BTool only receives `123456789012345678901234567890`

```text
  l2cap-ble line = data 0a6200 +0ms
  l2cap-ble handing request: 0a6200 +1ms
BLE read field `0103`
  l2cap-ble read response: 0b31323334353637383930313233343536373839303132 +0ms
  l2cap-ble send: 0b31323334353637383930313233343536373839303132 +0ms
```
