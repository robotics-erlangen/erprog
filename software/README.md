
# ERPROG Software

## Setup

**Build OpenOCD:**

1.	Download OpenOCD 0.8.0 (MD5 6d83c34763a5f1d1ac7ad83c5a11f4fb) and install `libusb-1.0`
2.	Build using:

	```
	mkdir build && cd build
	../configure --enable-ftdi --prefix=../build-out
	```

**On Linux:**

1.	Install the udev config file "udev/99-robotics-usb-devices.rules"
2.	Copy into the folder "/etc/udev/rules.d/"

**Embedded Compiler / Debugger:**
Get GCC 4.9 from https://launchpad.net/gcc-arm-embedded

**Create config files:**

1.	Create board file and flash configuration (see `board/*.cfg` and `flash/*.cfg`)
2.	Make sure to add

	```
	source [find interface/oocdlink-erforce2014.cfg]
	reset_config srst_only srst_nogate srst_push_pull
	```
	at the beginning


## Flashing     

*Tested on Linux and Mac OS X* 

1.	Connect connect the JTAG adapter with the board you want to flash and your USB port.
2.	Then run

	```
	cd JTAG-software
	${path/to/openocd} -c "debug_level 0" -s openocd -c "set ::ELF_FILE \"${path/to/elf-file}\"" -f flash/${CONFIG-NAME}.cfg
	```


**Troubleshooting:**
-	Check that the JTAG-adapter is connected to USB and the board
-	Check that the board is powered on
-	Check that the udev file (udev/99-robotics-usb-devices.rules) is installed
-	OpenOCD complains about "JTAG-DP OVERRUN" -> see Emergency Rescue
-	Mac: „Error: libusb_claim_interface() failed with -3“. Run:
	```
	sudo kextunload -bundle com.apple.driver.AppleUSBFTDI
	```

## Debugging

*Only works on Linux*

1.	**Flash the board with a build with DEBUG symbols!**
2.	Then start openocd for the board

	```
	cd JTAG-software
	${path/to/openocd} -c "debug_level 0" -s openocd -f board/${BOARD-NAME}.cfg
	```
3.	Connect to the debugger using Qt Creator:
	Debug > Start debugging > Attach to Remote Debug Server ...
	Set the following values:
	
	```
	Kit: Embedded
	Server port: 3333 (This attaches to the first cpu target, use 3334 for the second, ...)
	Local executable: the elf file in bin/firmware that was flashed (debug suffix!)
	```

The Kit has to be created before:
Tools > Options ... > Build & Run > Kits

Add a new kit with name like embedded.
Device type: Generic Linux Device

Debugger > Edit... : Set path = /usr/bin/arm-none-eabi-gdb  (arm version of gdb)
Qt version: None


Have fun debugging! The debugger will attach to the running firmware. If you want
to debug the startup procedure, then edit the openocd configuration as described
in `openocd/board/all2014.cfg`
