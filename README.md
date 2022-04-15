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



