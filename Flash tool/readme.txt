Quick guide for updating C94 radio:

Run FlashUtilCL.exe with the CMD line and place the firmware that you need to flash in the same folder. You need the Boot loader and the executable. 

Use the Silicon labs EC2 or EC3 USB DEBUG ADAPTER that plugs into the C94 radio debug connector. 

First load the Boot loader by typing the following at the CMD line:


 FlashUtilCL.exe downloadusb -E BLxxx.hex "" 0 1

 BLxxx.hex will be for example BL_HM_TRP_007_03_39d8435639799b2afce04d78f19683e569107381.hex

--------------------------------------------------------------------------------------------

Then load the FW itself by running the follwing command:

 FlashUtilCL.exe downloadusb -R FWxxx.hex "" 0 1

 FWxxx.hex will be for example FW_HM_TRP_007_03_341a4c5601cb769a915aaba95419c1bbf6333a8e.hex




--------------------------------------------------------------------------------------------





Command-Line (32 bit) FLASH Programing Utility Readme File
Copyright (C) 2011 Silicon Laboratories, Inc.


This document provides information for using the Silicon Labs Command Line FLASH Programming Utility. 
Place SiUtil.lib and SiUtil.dll in the same directory as the FlashUtilCL.exe program.

The Programming Utility provides three different functions for use with the EC2 serial adapter and
three funcions for use with the USB serial adapter. If a USB debug adapter is used the USBHID.DLL driver
file must be located in the same directory as the SiUtil.dll. The "download" function 
provides for the download of a single Intel type hex file. The "FLASHErase" function will erase
the FLASH of a device, including devices that have been locked. The "MultiDevice" function will program 
multiple Silicon Labs JTAG devices arranged in a JTAG chain. The function programs one device at a time 
in the chain. It will connect, download, and then disconnect from the specified device. More information 
on Multi-device JTAG programming can be found in Application Note AN117 "Using the C8051Fxxx On-Chip 
Interface Utilities DLL".

Call FLASHUtilCL with no arguments for version and function usage information.

USAGE: 
	FLASHUtilCL.exe [Function] [Argument 1] [Argument 2] [Argument 3]...


Functions that require an EC2 Serial Adapter:

	Download [-Option Flags] [file.hex] [COM Port] [Debug Interface] [Baud Rate Index]

	FLASHErase [COM Port] [Debug Interface]
	
	MultiDevice [-Option Flags] [file.hex] [COM Port] [Debug Interface] [DevicesBeforeTarget] 
		    [DevicesAfterTarget] [IRBitsBeforeTarget] [IRBitsAfterTarget]

	Verify [-Option Flags] [file.hex] [COM Port] [Debug Interface] [Baud Rate Index]


Functions that require a USB Debug Adapter:

	DownloadUSB [-Option Flags] [file.hex] [Serial Number String] [Power On Disconnect] [Debug Interface]

	DownloadTxt [-Option Flags] [file.hex] [Address] [Serial Number String] [Power On Disconnect] [Debug Interface]
	
	FLASHEraseUSB [Serial Number String] [Debug Interface]
	
	MultiDeviceUSB [-Option Flags] [file.hex] [Serial Number String] [Debug Interface] [DevicesBeforeTarget] 
		       [DevicesAfterTarget][IRBitsBeforeTarget] [IRBitsAfterTarget]

	VerifyUSB [-Option Flags] [file.hex] [Serial Number String] [Debug Interface]


Parameter Descriptions:

COM Port:
	Enter the COM Port number that is being used to communicate with an EC2 Serial Adapter. 

Serial Number String:

	The serial number of the USB Debug Adapter. An empty string can be used if only one USB Debug Adapter 
	is available.

Power On Disconnect:

	0 - Power will NOT be provided by the USB Debug Adapter to the target device after disconnecting.
	1 - Power will continue to be provided by the USB Debug Adapter to the target device after disconnecting.
 	
Debug Interface:

	0 - JTAG Debug Interface (F0XX, F1XX, F2XX devices)
	1 - C2 Debug Interface (F3XX devices)

Baud Rate Index:	

	0 - Autobaud
	1 - 115200 kbps
	2 - 57600 kbps
	3 - 38400 kbps
	4 - 9600 kbps
	5 - 2400 kbps

Options: 

	E - Full Erase (Erases all of User Space including Lock Bytes)
	R - Set Run State at download completion (default= halt)
	S - ONLY for use with F02X devices! Use this option if you wish
	    to download your hex file to the Scratchpad Memory on F02X devices.
            Address range is 0000 to 007f.
	L - Lock code space after download.
	B1 - Download to bank 1
	B2 - Download to bank 2
	B3 - Download to bank 3

Devices Before Target:

	The number of devices in the JTAG chain before the Target Device.

Devices After Target:

	The number of devices in the JTAG chain after the Target Device.

IRBits Before Target:

	The sum of the instruction register bits before the Target Device in the JTAG chain.

IRBits After Target:

	The sum of the instruction register bits after the Target Device in the JTAG chain.

Examples: 

	FLASHUtilCL download -RE blink.hex 1 0 0
	FLASHUtilCL download blink.hex 1 1 0
	FLASHUtilCL downloadusb blink.hex "" 0 0
	FLASHUtilCL downloadusb blink.hex "EC3000013BA" 0 0
	FLASHUtilCL flasherase 1 0
	FLASHUtilCL flasheraseusb "" 0
	FLASHUtilCL multidevice -RE blink.hex 1 0 1 0 16 0
	FLASHUtilCL download -B1R blink.hex 1 0 0
	FLASHUtilCL multidevice -REB1 blink.hex 1 0 1 0 16 0
	FLASHUtilCL DownloadTxt -E c:\test.txt 20 "EC3004BAC81" 0 1


RETURN CODES:

	0	No Errors.
	1	File name or path Error
	2	Com Port Error.
	3	Download Sequence Error.
	4	Reset Sequence Error.
	5	Device Erase Error.
	6	Set Run Mode Error.
	7	Error Closing COM Port
	8	Invalid command line options
	9	Invalid Device Number Error (Multi Device only)
	10	Invalid Instruction Register bit Error (Multi Device only)
	11      USB Debug Adapter DLL not found.
	12	USB Debug Adapter Error.
	13	ConnectUSB Command Failed.
	14	Connect comand failed.
	15	Error SetJTAGDeviceAndConnect.
	16	Error SetJTAGDeviceAndConnectUSB.
	17	Error getting number of USB Debug adapter devices.
	18	USB debug adapter out of range.
	19 	Error getting USB debug adapter device serial number.
	20	Verification error.