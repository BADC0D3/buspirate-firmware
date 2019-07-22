# Flash
## Find the Bus Pirate port name
Using the following cmd `before` and `after` to find the Bus Pirate port name.  
`ls /dev/tty*`

note: On your system you may have a different name. For this document I'm going to use `/dev/ttyUSB0`.
```
$ ls /dev/tty*
/dev/tty    
/dev/tty0   
...
/dev/ttyUSB0 <== Bus Pirate
```

## Enable Bootloader
There are two ways to trigger the bootloader for an upgrade. 

### Method 1
The old way is to place a jumper between the `PGC` and `PGD` pins before plugging in the USB cable. Plug it in, the MODE LED will light indicating that the bootloader is active.

### Method 2
In current firmwares (v4.1+) just type $ at the Hi-Z mode prompt. The Bus Pirate will reset into bootloader mode without a jumper. Remember to turn off your terminal to free the serial port before proceeding. 

```
HiZ>$
BOOTLOADER
```

## Update Firmware
./pirate-loader_lnx --dev=/dev/ttyUSB0 --hex=FIRMWARE.hex

You should see pirate-loader report it's progress, finishing with `Firmware updated successfully :)!`
```
./pirate-loader_lnx --dev=/dev/ttyUSB0 --hex=BPv3-frimware-v6.1.hex
+++++++++++++++++++++++++++++++++++++++++++
  Pirate-Loader for BP with Bootloader v4+  
  Loader version: 1.0.2  OS: Linux
+++++++++++++++++++++++++++++++++++++++++++

Parsing HEX file [BPv3-frimware-v6.1.hex]
Found 21502 words (64506 bytes)
Fixing bootloader/userprogram jumps
Opening serial device /dev/ttyUSB0...OK
Configuring serial port settings...OK
Sending Hello to the Bootloader...OK

Device ID: PIC24FJ64GA002 [d4]
Bootloader version: 1,02
Erasing page 0, 0000...OK
Writing page 0 row 0, 0000...OK
Writing page 0 row 1, 0080...OK
Writing page 0 row 2, 0100...OK
Writing page 0 row 3, 0180...OK
Writing page 0 row 4, 0200...OK
Writing page 0 row 5, 0280...OK
Writing page 0 row 6, 0300...OK
Writing page 0 row 7, 0380...OK
Erasing page 1, 0400...OK
Writing page 1 row 8, 0400...OK
...
Writing page 41 row 334, a700...OK
Writing page 41 row 335, a780...OK

Firmware updated successfully :)!
Use screen /dev/ttyUSB0 115200 to verify
```

If you see `Error updating firmware :(`, but it progresses past `page 41`, this is actually a successful result.
```
./pirate-loader_lnx --dev=/dev/ttyUSB0 --hex=BPv3-frimware-v6.1.hex
+++++++++++++++++++++++++++++++++++++++++++
  Pirate-Loader for BP with Bootloader v4+  
  Loader version: 1.0.2  OS: Linux
+++++++++++++++++++++++++++++++++++++++++++

Parsing HEX file [BPv3-frimware-v7.1.hex]
Found 17866 words (53598 bytes)
Fixing bootloader/userprogram jumps
Opening serial device /dev/ttyUSB0...OK
Configuring serial port settings...OK
Sending Hello to the Bootloader...OK

Device ID: PIC24FJ64GA002 [d4]
Bootloader version: 1,02
Erasing page 0, 0000...OK
Writing page 0 row 0, 0000...OK
...
Writing page 35 row 287, 8f80...OK
Erasing page 41, a400...OK
Writing page 41 row 328, a400...OK
Writing page 41 row 329, a480...OK
Writing page 41 row 330, a500...OK
Writing page 41 row 331, a580...OK
Writing page 41 row 332, a600...OK
Writing page 41 row 333, a680...OK
Writing page 41 row 334, a700...OK
Writing page 41 row 335, a780...OK
Erasing page 42, a800...ERROR [50]

Error updating firmware :(
```

Now remove the jumper (if used), and reset the Bus Pirate by removing and attaching the USB cable. The firmware update is complete. 

## Check Firmware
Connect to your bus using the following command `screen /dev/ttyUSB0 115200 8N1`.  
Then check your firmware by entering `i` and hitting `enter`.

```
sudo screen /dev/ttyUSB0 115200 8N1

HiZ>i
Bus Pirate v3.5
Community Firmware v7.1 - goo.gl/gCzQnW [HiZ 1-WIRE UART I2C SPI 2WIRE 3WIRE PIC DIO] Bootloader v4.4
DEVID:0x0447 REVID:0x3046 (24FJ64GA00 2 B8)
http://dangerousprototypes.com
```

# Build
For Information on how to compile from src, visit the following URL:
https://github.com/BusPirate/Bus_Pirate/blob/master/Documentation/building-and-flashing-firmware.md

In the `Compile` folder, I have included the output of the compile code for 7.1.

## MPLAB-X
### Prerequisites
You will need to install the following libraries for MPLAB-X
```
dpkg --add-architecture i386
apt-get update
apt-get install libc6:i386 libx11-6:i386 libxext6:i386 libstdc++6:i386 libexpat1:i386
```