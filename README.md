# MSI-Z490-Tomahawk-OpenCore

Hi, this is my own take of OpenCore configuration with MSI Z490 Tomahawk

------------

## System Spec:

​	CPU: 				 i9 10858k (OCed to 4.9Ghz All Core, 4.7Ghz Ring Clock)

​	Cooling: 		   Custom Water Cooling (WIP, doesn't affect this project)

​	Motherboard:  MSI Z490 Tomahawk (BIOS Ver: E7C80IMS.190)

​	RAM:				  Crucial Ballistix 16G*2 3600 (C9BLH) OCed to 4400Mhz 17-21-21-38-660 All 2nd/territary subtiming tweaked (doesn't affect this project)

​	SSD:					WD SN750 1TB(Windows), Samsung 980 1TB(Macos), WD40EZRZ

​	GPU:					Reference AMD RX 6800 XT (Manufacturered by MSI)

​	WiFi:					Intel AX200 (By Fenvi)

​	PSU:					Thermaltake GF1 750w (Doesn't affect this project)

-------

## What's Working and what's not:

OpenCore: 0.7.7

OS: Monterey 12.1 Latest Update (As of 2022/Jan/23)

Everything seems to be working (WiFi, bluetooth, USB, Sleep, dGPU, iGPU, Audio)

What's not tested(but should be working): Both Lan Port, Rear Audio

PS2 port has not been taken care of, but should work after apply VoodooPS2.Kext

----------------

## Instruction

Note: Please Cross Reference my config with [Dortania's Guide](https://dortania.github.io/OpenCore-Install-Guide/), you should NEVER trust word from one person only, including me.

**Some of the config has been setup according to my own requirement which might not suit you**, for example USB Mapping would be slightly different if your computer case IO is different to mine (I have 1x USB3.0 port and 2x USB2.0 port on the Case's front IO)

##### 1.  Necessary BIOS Config

- Disable CFG Lock
- Disable Fastboot
- Enable XMP Profile for RAM
- Enable Above 4G decoding
- Enable iGPU 64MB
- Actually a lot more ---- WIP

##### 2. Prepare your own USB Installer, and copy my EFI folder to the EFI Partition of the USB. 

To Mount EFI Folder:

On Mac: [MountEFI](https://github.com/corpnewt/MountEFI) 

On Windows: 

    diskpart
    lis dis
    sel dis X
    lis par
    sel par Y
    assign letter=Z

X refers to the Disk your EFI partition is located at

Y refers to the Partition of your EFI Partition (usually 1)

Z refers to the Letter you want to mount your drive

Note After mount the partition is won't be accessible with explorer unless you kill the explorer process and launch it again with Admin prevelidge, a simple workaround is to use a alternative file explorer like Explorer++ with Admin prevelidge.

##### 3. Setup the SMBIOS with the Generator

To Edit the config.plist, you can use [ProperTree](https://github.com/corpnewt/ProperTree) (Note, to use this app, you will need to install [Python](https://www.python.org/downloads/) 3.9+ first, the MacOS pre-installed Python isn't sufficient enough. Then navigate to ~/ProperTree/Scripts, and run buildapp-select.command to build a MacOS app.)

To Generate SMBIOS, you can use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS), it is pretty straight forward(Press 1 to Install MacSerial first, then follow the instruction, you want to generate SMBIOS for iMac20,2). Place the generated data into PlatformInfo/Generic of your config.plist

##### 3. Install MacOS 

Detail won't be provided, please refer to Dortania's Guide

##### 4. Post-Install

[Place your EFI Folder to MacOS drive](https://dortania.github.io/OpenCore-Post-Install/universal/oc2hdd.html#grabbing-opencore-off-the-usb)



