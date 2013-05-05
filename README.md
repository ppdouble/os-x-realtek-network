## Fork of Mieze's Realtek RTL8111 Network Driver by RehabMan


### How to Install:

First you must remove any other Realtek driver you may have installed.  

Common ones are:
RealtekRTL81xx.kext
RealtekRTL8000SL.kext
AppleRTL8169Ethernet.kext

It is possible that these kexts are installed under directly in /System/Library/Extensions or under /S/L/E/IONetworkingFamily.kext/Contents/PlugIns, so be sure to check both locations.

Next, install the kext using Kext Wizard or your favorite kext installer.

Special note for Snow Leopard users:  There is a conflict between the RTL8169 driver that is part of Snow Leopard and this driver.  The RTL8169 driver, during its probe method, puts the hardware into a bad state where it cannot be detected properly by this driver.  As a result, you must remove it:

sudo rm -rf /System/Library/Extensions/IONetworkingFamily.kext/Contents/PlugIns/AppleRTL8169Ethernet.kext


### Downloads:

Downloads are available on Google Code:

https://code.google.com/p/os-x-realtek-network/downloads/list


### Build Environment

My build environment is currently Xcode 4.62, using SDK 10.6, targeting OS X 10.6.

This kext can be built with any of the following SDKs: 10.8, 10.7, or 10.6 but only by enabling
the hacks previously used in the code (see DISABLE_ALL_HACKS in the source code)

In addition, it can be built supporting any of these OS X targets: 10.8, 10.7, or 10.6.

For greatest compatibility, the provided build is SDK 10.6 targeting 10.6.


### 32-bit Builds

This project does not support 32-bit builds.  It is coded for 64-bit only.


### Source Code:

The source code is maintained at the following sites:

https://code.google.com/p/os-x-realtek-network/

https://github.com/RehabMan/OS-X-Realtek-Network

Mieze's original repository is now on github:

https://github.com/Mieze/RTL8111_driver_for_OS_X


### Feedback:

Please use the following thread on tonymacx86.com for feedback, questions, and help:

http://www.tonymacx86.com/hp-probook/93732-new-kexts-proposed-probook-installer-v6-1-a.html

- or here -

http://www.insanelymac.com/forum/topic/287161-new-driver-for-realtek-rtl8111/


### Known issues:


### Change Log:

2013-04-25 (RehabMan)

- Pulled latest changes from Mieze

2013-04-17 (RehabMan)

- Pulled latest changes from Mieze

- Removed hacks previously necessary for running on SL/Lion

- Now using SDK 10.6

- Now exporting public functions from RTL8111 class (better compatibility with SL)


2013-03-22 (RehabMan)

- Modified for single binary to work on ML, Lion, and SL.

- Optimize build to reduce code size and exported symbols.


2013-03-17 (Mieze)

- Initial build provided by Mieze on insanelymac.com


### History

This repository contains a modified version of Mieze's Ethernet driver for Realtek RTL8111 series chips.  All credits to Mieze for the original code and probably further enhancements/bug fixes.

Original sources came from this post on Insanely Mac:

http://www.insanelymac.com/forum/topic/287161-new-driver-for-realtek-rtl8111/

My goal in creating this repository was just to create a single binary that could be used on 10.8, 10.7, and 10.6 for use in the Probook Installer.


## README.md from Mieze's repository

RTL8111_driver_for_OS_X
=======================

OS X open source driver for the Realtek RTL8111/8168 family

*** Please note that this driver isn't maintained by Realtek! ***

Due to the lack of an OS X driver that makes use of the advanced features of the Realtek RTL81111/8168 series I started a new project with the aim to create a state of the art driver that gets the most out of those NICs which can be found on virtually any cheap board on the market today. Based on Realtek's Linux driver (version 8.035.0) I have written a driver that is optimized for performance while making efficient use of system resources and keeping the CPU usage down under heavy load.

Key Features of the Driver
- Supports Realtek RTL8111/8168 C/D/E/F/G found on recent boards.
- Support for multisegment packets relieving the network stack of unnecessary copy operations when assembling packets for transmission.
- No-copy receive and transmit. Only small packets are copied on reception because creating a copy is more efficient than allocating a new buffer.
TCP, UDP and IPv4 checksum offload (receive and transmit).
- TCP segmentation offload under IPv4.
- Fully optimized for Mountain Lion (64bit architecture) but should work with Lion too. Snow Leopard requies some changes.
- Supports Wake on LAN.
- Support for Energy Efficient Ethernet (EEE) which can be disabled by setting enableEEE to NO in the drivers Info.plist without rebuild. The default is YES.
- The driver is published under GPLv2.

Limitations
- Support for the Realtek RTL8111B/8168B is still experimental. Therefore it is only included in debug builds and has never been tested successfully because I don't have access to a board with one of these outdated chips.
- As checksum offload doesn't work with jumbo frames they are currently unsupported and will probably never be.
- No support for 32bit kernels.

Installation

Before you install the driver you have to remove any installed driver for RTL8111/8168.

- Goto /S/L/E and delete the old driver (Lnx2mac, AppleRealtekRTL8169, etc.).
    
- Recreate the kernel cache.
    
- Open System Preferences and delete the corresponding network interface, e. g. en0. If you forget this step you might experience strange problems with certain Apple domains, iTunes and iCloud later.
    
- Shutdown, wait for 30 seconds and reboot.
    
- Install the new driver and recreate the kernel cache.
    
- Reboot
    
- Open System Preferences again, select Network and check if the new network interface has been created automatically or create it manually now.
    
- Configure the interface.

Current status

The driver has been successfully tested under 10.8.2 and 10.8.3 with the D (chipset 9), E (chipset 16) and F (chipset 17) versions of the RTL8111 and is known to work stable on these devices but you'll have to consider that there are 25 different revisions of the RTL8111. The RTL8111B/8168B chips have been reported to work since version 1.0.2 too.

Changelog
- Version 1.0.1 (2013-03-31):
    - Improved behavior when rx checksum offload isn't working properly.
    - Adds the chipset's model name to IORegistry so that it will show up in System Profiler.
- Version 1.0.2 (2013-04-22):
    - Added support for rx checksum offload of TCP and UDP over IPv6.
- Version 1.0.3 (2013-04-25):
    - The issue after a reboot from Windows has been eliminated.
- Version 1.0.4 (2013-05-04)
    - Moved setLinkStatus(kIONetworkLinkValid) from start() to enable(). Cleaned up getDescCommand().

Known Issues
- Eventually you might find an "Ethernet [RealtekRTL8111]: replaceOrCopyPacket() failed." message in the log file. This is nothing to worry about and means that a single packet has been dropped because the driver failed to allocate a new packet buffer. Packet buffers are allocated from a buffer pool which is dynamically sized by the network stack. When the pool is exhausted the OS increases it's size making the error a self-healing issue.

Building from Source

I'm using XCode 4.6.2 for development. You can get a free copy of XCode after becoming a member of the Apple developer program. The free membership is sufficient in order to get access to development tools and documentation.
