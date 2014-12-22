[[_TOC_]]

### Open Source Projects

[BeaconScanner](https://github.com/mlwelles/BeaconScanner), is an iBeacon scanner utility app for Mac OS X.

[BeaconEmitter](https://github.com/lgaches/BeaconEmitter), turn your Mac as an iBeacon
![](https://github.com/lgaches/BeaconEmitter/raw/master/assets/BeaconEmitter.jpg)

[bluepy](https://github.com/IanHarvey/bluepy), the python interface to Bluetooth LE on Linux. (using bluez 5.4)

[bleno](https://github.com/sandeepmistry/bleno), a node.js module for implementing BLE peripherals.

[noble](https://github.com/sandeepmistry/noble), a node.js module for implementing BLE central.

[node-beacon](https://github.com/sandeepmistry/node-bleacon), a node.js library for creating, discovering, and configuring iBeacons.

**start advertising**:

```javascript
var Bleacon = require('bleacon');
var uuid = 'e2c56db5dffb48d2b060d0f5a71096e0';
var major = 0; // 0 - 65535
var minor = 0; // 0 - 65535
var measuredPower = -59; // -128 - 127 (measured RSSI at 1 meter)

Bleacon.startAdvertising(uuid, major, minor, measuredPower);
```

**start scanning**

```javascript
var uuid = 'e2c56db5dffb48d2b060d0f5a71096e0';
var major = 0; // 0 - 65535
var minor = 0; // 0 - 65535

Bleacon.startScanning([uuid], [major], [minor]);

// scan for any bleacons
Bleacon.startScanning(); 

// scan for bleacons with a particular uuid
Bleacon.startScanning(uuid); 

// scan for bleacons with a particular uuid and major
Bleacon.startScanning(uuid, major); 

// scan for bleacons with a particular uuid. major, and minor
Bleacon.startScanning(uuid, major, minor); 
```

[node-sensortag](https://github.com/sandeepmistry/node-sensortag), node.js lib for TI SensorTag
  - discover
  - connect / disconnect
  - discover services and characteristics
  - device info
  - IR temperature sensor
  - accelerometer
  - humidity
  - magnetometer
  - barometric pressure(?)
  - gyroscope
  - simple

### Utilities

hcidump (refer to [here](http://stackoverflow.com/questions/20252587/raspberry-pi-ibeacon-connection-timing-out))

```text
HCI sniffer - Bluetooth packet analyzer ver 5.11
device: hci0 snap_len: 1500 filter: 0xffffffff
< HCI Command: LE Set Advertise Enable (0x08|0x000a) plen 1
> HCI Event: Command Complete (0x0e) plen 4
    LE Set Advertise Enable (0x08|0x000a) ncmd 1
    status 0x0c
    Error: Command Disallowed
< HCI Command: LE Set Advertising Data (0x08|0x0008) plen 44
> HCI Event: Command Complete (0x0e) plen 4
    LE Set Advertising Data (0x08|0x0008) ncmd 1
    status 0x00
< HCI Command: LE Set Advertising Parameters (0x08|0x0006) plen 15
    min 1280.000ms, max 1280.000ms
    type 0x00 (ADV_IND - Connectable undirected advertising) ownbdaddr 0x00 (Public)
    directbdaddr 0x00 (Public) 00:00:00:00:00:00
    channelmap 0x07 filterpolicy 0x00 (Allow scan from any, connection from any)
> HCI Event: Command Complete (0x0e) plen 4
    LE Set Advertising Parameters (0x08|0x0006) ncmd 1
    status 0x00
< HCI Command: LE Set Advertise Enable (0x08|0x000a) plen 1
> HCI Event: Command Complete (0x0e) plen 4
    LE Set Advertise Enable (0x08|0x000a) ncmd 1
    status 0x00
> HCI Event: LE Meta Event (0x3e) plen 19
    LE Connection Complete
      status 0x00 handle 64, role slave
      bdaddr B8:F6:B1:1C:15:C8 (Public)
> ACL data: handle 64 flags 0x02 dlen 11
    ATT: Read By Type req (0x08)
      start 0x0001, end 0xffff
      type-uuid 0x2a00
> HCI Event: Disconn Complete (0x05) plen 4
    status 0x00 handle 64 reason 0x13
    Reason: Remote User Terminated Connection
```


### Notes

[Bluetooth Low Energy (4.0) ON UBUNTU 13.10: Advertisements, Sending and Receiving](http://www.orangenarwhals.com/?p=887)

- ubuntu 13.10 and two CSR 4.0 BTLE dongles

```bash
# install things
sudo apt-get install libusb-dev libdbus-1-dev libglib2.0-dev libudev-dev \
        libical-dev libreadline-dev bluez wireshark

# plug in the first dongle (it should show up in hciconfig as “hci0″)
# start LE scan capture (you’ll see it spit out things such as “00:1A:7D:DA:71:0D (unknown)”)
sudo hcitool lescan

# start wireshark as root
sudo wireshark

# In wireshark, start capture on bluetooth0

# plug in the second dongle, should show up as hci1
# program hci1 with hciconfig
# 
# (sometimes you can skip this step, sometimes the next step, leadv, will throw up “LE set advertise enable on hci1 returned status 12″ if you don’t do noleadv first)
sudo hciconfig hci1 noleadv
sudo hciconfig hci1 leadv
sudo hciconfig hci1 noscan

sudo hcitool -i hci1 cmd 0x08 0x0008 1E 02 01 1A 1A FF 4C 00 02 15 E2 0A 39 F4 73 F5 4B C4 A1 2F 17 D1 AD 07 A9 61 00 00 00 00 C8 00
```

![](http://www.orangenarwhals.com/wp-content/uploads/2014/06/Screenshot-from-2014-06-29-021620.png)

(todo: get ble-python.py from [stackoverflow](http://stackoverflow.com/questions/23788176/finding-bluetooth-low-energy-with-python))
```python
print(':'.join("{0:02x}".format(x) for x in data[44:13:-1]))
```

how to write data using the second CSR4.0 dongle and python:

```python
import subprocess
dev = 'hci1'
adr = '0x08 0x0008 1E 02 01 1A 1A FF 4C 00 02 15 E2 0A 39 F4 73 F5 4B C4 A1 2F 17 D1 AD 07 A9 61 05 06 07 08 C8 00'
cmd_cc = "hcitool -i %s cmd %s" % ( dev, adr )

subprocess.Popen(cmd_cc.split(),
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE).communicate()
```

- [how ibeacon work](http://www.warski.org/blog/2014/01/how-ibeacons-work/)
- http://hackaday.com/2013/12/05/turning-a-pi-into-an-ibeacon/ 
- https://learn.adafruit.com/pibeacon-ibeacon-with-a-raspberry-pi/adding-ibeacon-data
- http://stackoverflow.com/questions/22568232/how-to-retrieve-advertising-payload-from-ibeacon-ble 
- http://stackoverflow.com/questions/23788176/finding-bluetooth-low-energy-with-python 

[Is there a way to increase BLE advertisement frequency in BlueZ?](http://stackoverflow.com/questions/21124993/is-there-a-way-to-increase-ble-advertisement-frequency-in-bluez)

```bash
sudo hciconfig hci0 up
sudo hcitool -i hci0 cmd 0x08 0x0008 1e 02 01 1a 1a ff 4c 00 02 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 00 00 00 00 c5 00 00 00 00 00 00 00 00 00 00 00 00 00
sudo hcitool -i hci0 cmd 0x08 0x0006 A0 00 A0 00 03 00 00 00 00 00 00 00 00 07 00
sudo hcitool -i hci0 cmd 0x08 0x000a 01
```

[Bluetooth Low Energy Connection Parameters for Android, iOS and Win8](http://stackoverflow.com/questions/22514333/bluetooth-low-energy-connection-parameters-for-android-ios-and-win8)

Parameters:
- Connection Interval Min
- Connection Interval Max
- Slave Latency
- Supervision Timeout
- Advertising Interval Min
- Advertising Interval Max

References:
- [Bluetooth Design Guide](https://developer.apple.com/hardwaredrivers/bluetoothdesignguidelines.pdf)
