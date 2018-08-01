# Serial Connections using an FTDI UART cable 
How to open a serial connection to an FTDI UART cable using 'screen' on a linux.

Gunnar Pope

8/1/2018


* Find out the information relating to your serial cable with the `dmesg` command.
```
$ dmesg
[40296.155513] usb 1-1: new full-speed USB device number 14 using xhci_hcd
[40296.309797] usb 1-1: New USB device found, idVendor=0403, idProduct=6001
[40296.309803] usb 1-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[40296.309806] usb 1-1: Product: FT232R USB UART
[40296.309810] usb 1-1: Manufacturer: FTDI
[40296.309813] usb 1-1: SerialNumber: AI04QLKZ
[40296.312802] ftdi_sio 1-1:1.0: FTDI USB Serial Device converter detected
[40296.312899] usb 1-1: Detected FT232RL
[40296.313215] usb 1-1: FTDI USB Serial Device converter now attached to ttyUSB0
```

Notice the idVendor number and idProduct information. We need to add this to your udev rules to open a connection with the device.

* Navigate to your udev rules folder and create a file named 62-FTDI-UART.rules.
```
$ cd /etc/udev/rules.d
/etc/udev/rules.d $ vim 62-FTDI-UART.rules
```


* Write the following rules into this file and close it.
```
62-FTDI-UART.rules
ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",MODE="0666"
```

* The final file should look like:
```
/etc/udev/rules.d $ cat 62-FTDI-UART.rules
# FTDI USB serial device rules on ttyUSB0
ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",MODE="0666"
```

* Now, unplug your FTDI cable and replug it again. Open a serial session with a your device using the screen command `$ screen /dev/ttyUSB0 <baudrate>` where `<baudrate>` is the datarate that you have configured your peripheral microcontroller to operate at. Usually, a baudrate of 9600 or 115200 are most common.
```
$ screen /dev/ttyUSB0 115200
```
You should be able to communicate with your device now using a serial connection.


* Press `Ctrl-a` then `k` to quit the serial session.
