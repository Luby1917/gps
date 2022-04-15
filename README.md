# GPS
GPS system for Regularity Rallyes


GPS system used is Ardusimple RTK Mni
THe bluetooth sistem included its not cappable to 10Hz so a Raspberry Pi will be involved

# Config

## USB
If we plug the GPS via USB to the pi usually the serial port will be available on /dev/ttyUSB0 (if its the first device plugged)

We can check the GPS output via `cat < /dev/ttyUSB0` if no output its see you must to change the baudrate `stty -F /dev/ttyUSB0 ispeed 115200 ospeed 115200`

## Bluetooth

Here comes the tricky part the BT config.

First, we need to check the the BT controller:

`bluetooth`
discoverable on
scan on

pair [MAC]
connect [MAC] // optional


To accept connections we need:

Modify the bluetooth service config: 
`vi  /etc/systemd/system/dbus-org.bluez.service`

And change these line: 

```
ExecStart=/usr/libexec/bluetooth/bluetoothd -C  --noplugin=sap
ExecStartPost=/usr/bin/sdptool add SP
```
Then execute (best if incluede as service)
rfcomm watch hci0

Optionally, may be the `sudo rfcomm bind /dev/rfcomm0 [MAC] 1` command be in help

this will make the connected devices be available on `/dev/rfcomm0`

Now we have availble both data streams, now we must to sync them in order to use the GPOS via BT

## Data sync


To to send the BT data to USB and backwards we use th socat command (usually not installed by default)
```
 socat /dev/rfcomm0,raw,echo=0,crnl /dev/ttyUSB0,raw,echo=0,crnl
 socat /dev/rfcomm0,raw,echo=0,crnl /dev/ttyUSB0,raw,echo=0,crnl
```
