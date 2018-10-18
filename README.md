# Thinkpad X230 Clover Config for macOS 10.13.3

**English**



**Run macOS Sierra on your ThinkPad X230**

Video Tutorial : 
https://youtu.be/kGhZUovKYFM (10.13.1)
https://youtu.be/MkGsSwpwheQ (10.12.x)


## Introduction

**Note**

 -  Extra resources from online sources might be required.
 -  The WiFi Card used in this tutorial is Azurewave CE123-H. (BIOS whitelist removed)
 -  The resources listed here are gathered from Rehabman, tonymacx86 and different sources. It's based on Bizzaro's x230-osx project. Link here: https://github.com/Bizzaro/x230-osx
 -  Sorry for my bad english and this complicated guide.
 -  Install macOS Sierra might cause Data Loss. **REMEMBER TO BACKUP BEFORE INSTALLING**.
 -  This guide comes with absolutely no warranty. I am not responsible for any damage in hardware , software or data loss on your laptop. Take your own risk.

------

**Guides/Examples (Credit Goes to tonymacx86 , InsanelyMac , Gurus , Rehabman , etc)**

 1. Brief Introduction : https://www.tonymacx86.com/threads/unibeast-install-macos-sierra-on-any-supported-intel-based-pc.200564/
 2.  Unsolved Problems : https://www.tonymacx86.com/threads/readme-common-some-unsolved-problems-in-10-12-sierra.202316/
 3. DSDT / SSDT guides : https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/
 4. Booting the OS X Installer with Clover : https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/
 5. Airport Injection : http://www.insanelymac.com/forum/topic/292542-airport-pcie-half-mini/
 6. Toleda's Wireless_Half_Mini Repository : https://github.com/toleda/wireless_half-mini
 7. AppleHDA Patching : http://forum.osxlatitude.com/index.php?/topic/1946-complete-applehda-patching-guide/
 8.  iMessage / FaceTime : https://www.tonymacx86.com/threads/how-to-fix-imessage.110471/
 9.  iDiot's guide to iMessage / FaceTime : https://www.tonymacx86.com/threads/an-idiots-guide-to-imessage.196827/
 10. Everymac Lookup : http://www.everymac.com/ultimate-mac-lookup/
 11. Enable Intel WiFi without hardware flashing BIOS :https://www.tonymacx86.com/threads/rebranding-intel-centrino-n6205-into-ar5b95-and-fake-pci-id.203658/#post-1339270  (credit goes to Rehabman and Tasos Sofi)
 

## Summary

**Specs**

 - CPU : Intel Core i7 - 3520M
 - RAM : 8GB DDR3 1600MHz SO-DIMM
 - Storage : 180GB Intel SSD SATA 6Gb/s
 - Operating System : macOS Sierra 10.13.1
 - Bootloader : Clover v2.4k r4114 
 - EFI Firmware : 2.73 (Original)
 - EC Firmware : 1.14
 - WLAN Card : Atheros AR9285 (Broadcom BCM94352HMB) 

-----

**Working**

 - Native Power Management 
 - Ethernet 
 - USB ports 
 - Battery status
 - Keyboard, TrackPoint
 - Intel Graphics HD 4000 
 - Webcam
 - Bluetooth
 - Shutdown / Reboot
 - Audio
 - SSD Trimming
 - Microphone
-  Headset
 - Bluetooth
 - Appstore
 - iCloud
 - Siri
-  AR5B95 (AR9285)
 - AirDrop
 - HandOff (With AirPort)
 - Universal Clipboard (With AirPort)
 - BackLight
 - Facetime
 - iMessage
 - HDMI 
 - mini-DP 
 - Backlight Control
 - Sierra Wireless MC7710
 - Trackpoint (with buttons and scrolling)
-----

**Not Working**

 - VGA Port
 - Ricoh Card Reader
 - Fingerprint Reader

-----

**Bugs**

 - Boot Animation glitch 
 - Hibernation Wake


## Setup guide

**EFI Firmware Configuration**

| Item                                                    	| Setting   	| Remarks                                                                                                       	|
|---------------------------------------------------------	|-----------	|---------------------------------------------------------------------------------------------------------------	|
| Config/Network/Wake On Lan                              	| Disabled  	| Not Supported.                                                                                                	|
| Config/USB UEFI BIOS Support                            	| Enabled   	| Important for booting into USB Installer for OS X/ macOS.                                                     	|
| Config/USB 3.0 Mode                                     	| Enabled   	| Enables USB 3.0                                                                                               	|
| Config/Power/Intel SpeedStep Technology                 	| Enabled   	| Enables Intel SpeedStep.                                                                                      	|
| Config/Power/Intel Rapid Start Technology               	| Disabled  	| This feature requires a hibernation partition (0xA0). Not supported on OS X / macOS.                          	|
| Config/Power/CPU Power Management                       	| Enabled   	| Enables CPU Power Management.                                                                                 	|
| Config/Serial ATA (SATA)/SATA Controller Mode Option    	| AHCI      	| Enables AHCI which is better for SSDs and HDDs.                                                               	|
| Security/Predesktop Authentication                      	| Optional  	| Fingerprint is not supported on macOS / OS X but you can still use it for waking your ThinkPad.               	|
| Security/Security Chip                                  	| Disabled  	| Disable it if you have a modified UEFI Firmware.                                                              	|
| Security/Memory Protection/Execution Prevention         	| Enabled   	| This enables NX which is required for macOS / OS X installations.                                             	|
| Security/Virtualization/Intel Virtualization Technology 	| Enabled   	| Delete boot argument named "dart=0" in config.plist if you set this as Disabled.                              	|
| Security/Virtualization/Intel VT-d Feature              	| Enabled   	| Delete boot argument named "dart=0" in config.plist if you set this as Disabled.                              	|
| Security/Secure Boot/Secure Boot                        	| Disabled  	| Not supported by Clover.                                                                                      	|
| Startup/Boot Mode                                       	| Quick     	| This is optional.                                                                                             	|
| Startup/UEFI / Legacy Boot                              	| UEFI Only 	| Reduces Confusion.                                                                                            	|
| Startup/UEFI / Legacy Boot/CSM Support                  	| No        	| Setting this key as Yes requires a CSM Video Driver in Clover to provide proper video output on macOS / OS X. Only change this to Yes if you are dual booting with Windows 7 and installed CSMVideoDxe Driver in /EFI/CLOVER/drivers64UEFI/. 	|

---------

## Creating USB Installer

1. Read this first (https://www.tonymacx86.com/threads/unibeast-install-macos-sierra-on-any-supported-intel-based-pc.200564/). Open Clover_v2.4k_r4114.pkg in the Tools directory of this repository and install it on your USB.

2.  Mount EFI System Partition (ESP) of USB with EFI Mounter v3 (In tools folder). **Copy the whole repository to the root of your macOS Sierra USB Installer.**

3.  Open /Volumes/EFI/Clover/ , CHECK YOUR CPU MODEL. Depending on your CPU Model, Change Directory to `i7-3520M/ ` or `i5-3210M`  or `i5-3320M` folder.

4. Copy and Replace the entire `EFI/CLOVER` Folder in the ESP of your USB Drive with `EFI/CLOVER` folder in this repository.

5.  Open `EFI/CLOVER/config.plist` in the ESP of your USB Drive. Run CloverConfigurator included in the `Tools/` folder in the repository. Generate Serial Number, BoardSerialNumber, SmUUID in SMBIOS tab. Fill in CustomUUID in SystemParameters Tab.

**Special Reminder for users who have AzureWave CE-123H**

-  Before booting into the USB Installer, replace the file : `EFI/CLOVER/config.plist` with `WiFi-4352/config.plist`. Generate Serial Number, BoardSerialNumber, SmUUID in SMBIOS tab. Fill in CustomUUID in SystemParameters Tab.


**Special Reminder for i5 ThinkPad X230 users:**

- Remember to CHECK the `SSDT.aml` file in `EFI/CLOVER/ACPI/patched` folder is for you CPU Model. For example, if you are running Core i5-3320M Make sure the `SSDT.aml` file is for i5-3320M. (In `i5-3320M/EFI/CLOVER/ACPI/patched/`)

## Installation

1.  Modify UEFI Settings according to the table above. For other items. keep their default values.

2.  Boot your macOS Installer USB by pressing F12 while booting.

3.  Go to step 4 of this guide (https://www.tonymacx86.com/threads/unibeast-install-macos-sierra-on-any-supported-intel-based-pc.200564/) and follow the instructions there.

4.  Follow the instructions shown in macOS Sierra. 

--------

## Post-Install (Copied from Guide 1)

1.   Turn on your ThinkPad.

2.   Press F12 to choose boot device.

3.   Choose USB.

4.   At the Boot Screen, choose your **new Sierra installation.**

5.   Complete macOS Sierra setup.

## Post-Install (Kexts Installation)

1.   Copy all files inside `Kexts/` folder in this repository to your Desktop. If you are using AzureWave CE-123H, copy the Kexts in `WiFi-4352/Extra-Kexts/` to your desktop as well.

2.   Open `Tools `folder from this repository and run KextBeast. Install all kexts on your Desktop. The kext files on your Desktop can be deleted once the installation completed successfully. 

3.   Install aDummyHDA.kext via KextBeast.

5.  Run `Kext Utility.app` in `Tools/` folder from this repository. Type your password and fix Kexts Permissions and rebuild cache.

6.  Run EFI Mounter v3.app in `Tools` Folder. Mount the ESP of the USB Installer and the ESP of the local drive, (SSD/HDD) in your ThinkPad.

7.   Copy `EFI/CLOVER` folder from the **ESP of your USB Installer** to 
the ESP of your local drive. (Where Sierra is installed on)

8.  Check the SMBIOS of `EFI/CLOVER/config.plist` in your local drive. If they are empty, regenerate another one. Make sure you filled in BoardSerialNumber, SerialNumber and SmUUID. Also, check the CustomUUID in SystemParameters. Make sure it has a random UUID filled in.

