# ERPROG
This is a JTAG-Programmer developed by the Robot Soccer Team ER-Force from Erlangen, Germany.
The device is using a FTDI chip for communication and a digital isolator for the target.
All necessary files to build and control this device can be found within this repository.

The benefits of this hardware are the easy USB connection with EMC protection and over-current protection. Furthermore the standard JTAG output with a complete isolation up to 4kV. The target isolation is done by two ISO7231M from Texas Instruments. This device also does a voltage translation, which adapts to the target voltage-level. Working is guaranteed from 3.2 to 5.4V.
With this circuit the computer is never effected, if the programming target fails in any way.

Project origin: https://www.robotics-erlangen.de/publications/jtag-programmer

## Hardware

The PCB is designed with EAGLE. The schematic can be found in `erprog_1504-1.sch` and the board in `erprog_1504-1.brd`. The dimension of the PCB is 850mil x 2300mil.

The main ICs are

1.  FT4232HL does the USB to JTAG conversation
2.  ISO7231 is the digital isolator

## Software

The software readme is in url{https://github.com/robotics-erlangen/erprog/tree/master/software}

## Computer connection

**via USB**

The device is connected via a Mini-USB plug to the computer.

## Target connection

**via JTAG**

This hardware uses a JTAG standard connector with a 10 pin connector with the `CoreSight 10` pinout.
Follwoing the pinout is described based on a standard 10 pin 100mil plug with 2 rows.

1.  VTREF
2.  TMS
3.  GND
4.  TCK
5.  GND
6.  TDO
7.  NC
8.  TDI
9.  GND
10. nSRST

## ToDo

For the next release it is planned to allow for controlling the isolator's enable-pin. Thus, the programmer can be put to high-impedance mode so it does not interfere with the target in normal operation.
