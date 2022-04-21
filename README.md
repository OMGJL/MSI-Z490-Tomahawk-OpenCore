# MSI-Z490-Tomahawk-OpenCore

Hi, this is my own take of OpenCore configuration with MSI Z490 Tomahawk

------------

## System Spec:

​	CPU: 				 i9 10858k (OCed to 4.9Ghz All Core, 4.7Ghz Ring Clock)

​	Cooling: 		   Custom Water Cooling (WIP, doesn't affect the scope of this project)

​	Motherboard:  MSI Z490 Tomahawk (BIOS Ver: E7C80IMS.190)

​	RAM:				  Crucial Ballistix 16G*2 3600 (C9BLH) OCed to 4400Mhz 17-21-21-38-660 All 2nd/territary subtiming tweaked (doesn't affect this project)

​	SSD:					WD SN750 1TB(Windows), Samsung 980 1TB(Macos), WD40EZRZ

​	GPU:					Reference AMD RX 6800 XT (Manufacturered by MSI)

​	WiFi:					BCM94360 Apple Original With PCIe Adapter

​	PSU:					Thermaltake GF1 750w (Doesn't affect this project)

\*\*My OC Setting is listed on the bottom of this Guide if you are intersted, **DO NOT Copy my setting** as it is very unlikely going to work even if you have exactly the same setup, **unless your silicon lottery is better than mine in everyway**. As all settings are tweaked to exactly the highest/lowest value that is still stable.

 (I was using Intel AX200 for WiFi/Bluetooth, however I was not able to get bluetooth working on this setup, even with different versions of IntelBluetoothFirmware.kext and BlueToolFixup.kext, changing USB Port Mapping/type won't work either, if someone could let me know why it would be great) -- The Very Same Intel AX200 card I pulled out of this PC worked in my secondary PC (3770k + ASUS z77) with Monterey for BOTH bluetooth and WiFi, strange.

----------------

## What's Working and what's not:

OpenCore: 0.7.7

OS: Monterey 12.3 Latest Update (As of 2022/Mar/26)

Everything is working (WiFi, bluetooth, USB, Sleep, Wake, dGPU, iGPU, Audio)

PS2 port has not been taken care of, but should work after apply VoodooPS2.Kext

**note: when connecting to 1Gbps Ethernet port with RTL 2.5Gbps Ethernet port. It will only work after go into Network -> Ethernet 2 -> Advanced -> Hardware -> Speed -> 1000baseT 

----------------

## Instruction

Note: Please Cross Reference my config with [Dortania's Guide](https://dortania.github.io/OpenCore-Install-Guide/), you should NEVER trust word from one person only, including me.

**Some of the config has been setup according to my own requirement which might not suit you**, for example USB Mapping would be slightly different if your computer case IO is different to mine (I have 1x USB3.0 port and 2x USB2.0 port on the Case's front IO) **My Guide will hopefully cover all the customize part, let me know if I missed anything.**

#### 1.  Necessary BIOS Config

- Legacy USB Support = **Disable**
- Above 4GB MMIO BIOS Assignment/ Above 4G Memory = **Enable**
- Initiate Graphic Adapter = **IGD** (this is to get Intel HD Graphic working in MacOS, doesn't affect anything even if you have dGPU, you can still access BIOS with dGPU and everything. Note I have no monitor plugged into iGPU, all monitor is connected via dGPU)
- IGD Multi-Monitor = **Enabled**
- ResizebleBar = **Enable**
- iGPU Memory = **64MB**
- Fast Boot = **Disabled**
- CFG Lock = **Disabled**
- Intel C-State = **Disabled** (I honestly don't think this has to do with Hackintosh, I think I did it for Overclock stability)
- Enable XMP Profile for RAM (Or Manually OC your memory like what I did)

#### 2. Prepare your own USB Installer, and copy my EFI folder to the EFI Partition of the USB. 

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

Note After mount the partition is won't be accessible with explorer unless you kill the explorer process and launch it again with Admin prevelidge, a simple workaround is to **use a alternative file explorer** like **Explorer++** with **Admin prevelidge**.

#### 3. Setup the SMBIOS with the Generator

To Edit the config.plist, you can use [ProperTree](https://github.com/corpnewt/ProperTree) (Note, to use this app in MacOS, you will need to install [Python](https://www.python.org/downloads/) 3.9+ manually first, the MacOS pre-installed Python isn't sufficient enough. Then navigate to ~/ProperTree/Scripts, and run buildapp-select.command to build a MacOS app.)

To Generate SMBIOS, you can use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS), it is pretty straight forward(Press 1 to Install MacSerial first, then follow the instruction, you want to generate SMBIOS for **iMac20,2**). Place the generated data into PlatformInfo/Generic of your config.plist

Short Direction of using GenSMBIOS:

1. Run GenSMBIOS.command
2. Press 1, install MacSerial
3. Press 2, drag my config.plist into it
4. press 3, then type exactly "**iMac20,2**" , press enter.
5. press 4,
6. press 5

now go to [Apple's Warranty Check website](https://checkcoverage.apple.com) and Enter the serial number you just generated, make sure it says: "We’re sorry, we’re unable to check coverage for this serial number. Please check your information and try again. For help with Apple accessories, please"

If it say anything else, **YOU MUST** generate a new serial number follow the instruction above **AGAIN**.

**After using GenSMBIOS**, open the config.plist, change The **Root/PlatformInfo/Generic/ROM** value to your WiFi adapter's MAC address. 

**Example:** if your WiFi mac address is AC:BC:00:11:22:33, then the ROM entry should have value of: acbc00112233

**NOTE:** iMac20,2 should be good for Z490 platform, if you have a different motherboard please check other guide, YMMV.

#### 4. Install MacOS 

Detail won't be provided, please refer to Dortania's Guide (Make sure you have made a 200MB EFI partition as a first partition of the SSD you are about to install MacOS into) 

I have found that the installer enviornment takes longer to be fully initialized than actual MacOS, so if have a black screen or your mouse isn't responding and such, be more patient and give it 2 more minute before you restart.

#### 5. Post-Install

[Place your EFI Folder to MacOS drive](https://dortania.github.io/OpenCore-Post-Install/universal/oc2hdd.html#grabbing-opencore-off-the-usb)

#### 6. Do your own USBMap

I used [USBToolBox](https://github.com/USBToolBox/tool) for convenience. It works out perfect for this board, if you care about every second of your boot time, maybe use DSDT Patch method instead. 

[Download](https://github.com/USBToolBox/tool/releases/tag/0.0.9) Windows.zip, unzip and run **windows.exe** .

press D, it will load into a port detection interface.

You will want to grab a pen and paper, then 

​		plug in a USB Drive to a port, note down its port number, then 

​		plug it to the another port, note down its port number, 

until you go through EVERY PORT.

**Once you done**, press Q and then press S, you can try to enable/disable ports as you wish.

![](z490Tomahawk.png)

Above is my port discovery FYI, I do not know the Internal Type-C port number, and I only know port number for 1 out of 2 of the Internal USB 3.0 Header. Please test yourself.



(I have changed my PC Case for water cooling now, and my new case have 2* USB3.0 port on the front), the new USB map I uploaded have enabled both USB port on the front, but since macOS have 15 USB port limit, I have no choice but to disable Port 26/15 (the USB port next to 2.5G Ethernet)



**To enable/disable a port**, just key in its port number,

**To change a port's type**, type "T:PortNumber:xxx"    Example: to change Port 11 to type 255, "T:11:255" 

then generate UTBMap.kext, put [USBToolBox.kext](https://github.com/USBToolBox/kext/releases) along with UTBMap.kext and place them both into your EFI/OC/Kexts folder (and add to your config.plist, my config.plist have it added already) 

**Make sure on config.plist**, USBToolBox.kext entry **is ABOVE** UTBMap.kext or it won't load properly, check my config.plist for reference.



#### 7. Good to know(My personal Experience)

When you add kext entry to your config.plist, **the order matters**

For example, you need to put VirtualSMC.kext before SMCProcessor.kext, otherwise SMCProcessor.kext won't load properly as its dependency isn't loaded yet.

Same goes with Whatevergreen and AppleALC, same goes with USBToolBox and its mapping



#### 8. Nice to have

**Sync Time Between Windows and Mac**

open CMD as admin, copy and run the following line:

Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1



**Allow your bluetooth device to connect to both your Windows/Mac without needing to pair them again.** Credit to [Dortania](https://github.com/dortania/clover-laptop-guide/blob/master/extras/dual-booting-with-bluetooth-devices.md) and u/TheVoyvode from [Reddit](https://www.reddit.com/r/hackintosh/comments/p5ost3/macos_monterey_and_windows_bluetooth_pairing/) 

Todo:

1. Pair your BT device in Windows
2. Reboot, Pair the same device in Mac again.
3. Open **KeyChain Access** search "bluetooth" on top right search box, find MobileBluetooth on the bottom of the list, double click it, make sure the MAC address is the device you want to play with.
4. Check "show password" checkbox, enter User name/password twice, then Command+A, Command + C the text box. The **LinkKey** Attribute is what you want to take note of (Write it down, take a photo, send yourself a email, etc.)
5. Reboot to Windows, open **regedit**, navigate to HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\BTHPORT\Parameters\Keys\
6. You probably won't see any entries under Keys, you want to right click Keys -> Permissions -> Add -> enter your user name in the bottom text box -> press "Check Names" -> Press Ok -> Click your User Name -> check every checkbox under allow -> Press OK -> Close Regedit and reopen it again.
7. Now you will be able to see all attributes named by their MAC address, copy the LinkKey over as-is (you don't need to change the order like what Dortania's guide said, just straight copy from begin to end)
8. All done



**Enable ResizebleBar support**

in config.plist, change:

<key>ResizeAppleGpuBars</key>
			<integer>0</integer>

(already added in my GitHub repo)



**Mouse Accerlation Off**

[LinerMouse](https://github.com/linearmouse/linearmouse)



#### 9. Overclock I Run

1. Voltage/Power Related: 

​		vCore:	1.345v (Override Mode)

​		SA:		  1.4v

​		IO:		   1.36v

​		DRAM:	1.44v

​		CPU Loadline Calibration: Mode 6

​		CPU Over Temperature Protection: 95 DegreesC

​		CPU Switching Frequency: 630KHz (I don't think this matters much but I had it anyway)

​		Intel C-State: Disabled

2. Core:

   ​	CPU Ratio: 49

   ​	Ring Ratio: 47

3. RAM:

   **Primary:**

   ​	4400@ 17-21-21-38-660 2T

   **Secondary:**

   ​	tREFI 65535
   ​	tWR 10 tWR_MR auto tRTP 5 RTP_MR auto
   ​	tWTR 3 WTR_L 6
   ​	tRRD 4 RRD_L 4 tFAW 16
   ​	tCWL 18

   **Territory:**

   ​	RDRD SG 6 DG 4
   ​	WRWR SG 6 DG 4
   ​	RDWR SG 10 DG 10
   ​	WRRD SG 29 DG 27

   (My Kit was 16G*2 with Micron C9BLH IC, so they are single rank and is 1DPC, hence DR DD setting not required)

   **RTL/IOL:**

   Latency Timing Setting Mode = Fixed Mode

   RTL init Value: 66 (both dimm) 

   IOL Init Value: 2 (never had any issue with cold boot yet)

   which gives me: RTL 67 on both channel, IOL 9/7 

   **ODT** 

   (same on both Channel)
   Rtt wr 80
   Rtt Norm 34
   Rtt Park 240

   

   ​	
