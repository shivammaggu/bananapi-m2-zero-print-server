# A Custom Print Server using Banana Pi M2 Zero

A print server that gets attached to a USB printer and shares its printing capability over the network using WiFi :signal_strength:

## Contents

1. [Requirements](#requirements-writing_hand)

    1. [Standard Requirements](#standard-requirements-printer)

    2. [Hardware Requirements for initial setup](#hardware-requirements-for-initial-setup-desktop_computer)

    3. [Hardware Requirements for Print Server](#hardware-requirements-for-print-server-toolbox)

       - [Banana Pi M2 Zero Board Features](#board-features)

       - [Where to buy Banana Pi M2 Zero](#where-to-buy)

       - [Where to buy 12.5W Official Raspberry Pi Micro USB Power Supply](#where-to-buy-1)

       - [Where to buy Heat Sink](#where-to-buy-2)

       - [Where to buy Class 10 Micro SD Card 16 GB/32 GB](#where-to-buy-3)

       - [Where to buy Raspberry Pi Zero W Case](#where-to-buy-4)

       - [Where to buy Micro USB-B Male to USB-A Female Adapter](#where-to-buy-5)

       - [Where to buy Mini HDMI to Standard HDMI Adapter](#where-to-buy-6)

    4. [Software Requirements for Print Server](#software-requirements-for-print-server-computer)

2. [Setup](#setup-hammer_and_wrench)

    1. [Software Setup](#software-setup-technologist)

    2. [Hardware Setup](#hardware-setup-mechanic)

    3. [Print Server Setup](#print-server-setup-space_invader)

        - [Router Configuration](#router-configuration)

        - [Banana Pi Hardware Configuration](#banana-pi-hardware-configuration)

        - [Debian OS Configuration](#debian-os-configuration)

        - [Static IP Configuration](#static-ip-configuration)

        - [WiFi Configuration](#wifi-configuration)

        - [SSH Configuration](#ssh-configuration)

        - [HP Driver Configuration](#hp-driver-configuration)

        - [SAMBA Configuration](#samba-configuration)

        - [CUPS Configuration](#cups-configuration)

        - [Printer Configuration](#printer-configuration)

        - [HP Driver Plugin Configuration](#hp-driver-configuration)

3. [Usage](#usage-point_down)

    1. [Android](#android-arrow_forward)

    2. [iOS/iPadOS](#iosipados-iphone)

    3. [macOS](#macos-)

    4. [Linux](#linux-penguin)

    5. [Windows](#windows-window)

4. [Notes](#notes-memo)

    1. [Documents](#documents-books)

    2. [Links](#links-link)

    3. [Further Enhancements](#further-enhancements-hammer_and_pick)

    4. [Known Issues](#known-issues-lady_beetle)

## Requirements :writing_hand:

- ### Standard Requirements :printer:

  - Printer with USB cable - Of course :)
  - Working Wifi AP at home - Should I even need to mention this? :P

- ### Hardware Requirements for initial setup :desktop_computer:

  - Monitor with HDMI support
  - Standard HDMI cable
  - Keyboard
  - Micro SD card Adapter/SD card Reader
  - A PC or Laptop with Mac/Linux/Windows OS

- ### Hardware Requirements for Print Server :toolbox:

  - <details open>
    <summary> Banana Pi M2 Zero </summary>

    ###### Board Features

    - Allwinner H3/H2+ Quad-core Cortex-A7 H265/HEVC 1080P CPU
    - Mali400MP2 GPU @600MHz, Supports OpenGL ES 2.0
    - 512 MB DDR3 (shared with GPU)
    - Upto 64 GB SD card support
    - Onboard WiFi and Bluetooth with SDIO AP6212 chip
    - Audio/Video output from the Mini HDMI 1.4 port, supports 1080P@30fps
    - Micro USB DC power supply 5V/2A
    - Micro USB 2.0 OTG for data transfer
    - Power Button
    - Reset Button
    - Power LED/Status LED
    - U.fi antenna connector

    ###### Where to buy

    - [banana-pi](https://www.banana-pi.org/en/banana-pi-sbcs/1.html)
    - [tannatechbiz](https://tannatechbiz.com/banana-pi-m2-zero.html)

  </details>
  
  - <details>
    <summary> 12.5W Official Raspberry Pi Micro USB Power Supply </summary>

    ###### Where to buy

    - [raspberrypi](https://www.raspberrypi.com/products/micro-usb-power-supply/)
    - [silverlineelectronics](https://www.silverlineelectronics.in/products/12-5w-official-raspberry-pi-micro-usb-power-supply-for-raspberry-pi-1-2-3-zero2-zerow?_pos=1&_sid=5f97c430a&_ss=r)

  </details>
  
  - <details>
    <summary> Heat Sink </summary>

    ###### Where to buy

    - [silverlineelectronics](https://www.silverlineelectronics.in/collections/raspberry-pi-cooling-tech/products/copy-of-high-quality-big-green-small-sliveraluminum-heat-sink-set-for-raspberry-pi?_pos=2&_sid=5fed745ff&_ss=r)

  </details>
  
  - <details>
    <summary> Class 10 Micro SD Card 16 GB/32 GB </summary>

    ###### Where to buy

    - [silverlineelectronics](https://www.silverlineelectronics.in/collections/raspberry-pi-sd-cards-storage/products/copy-of-sandisk-micro-sd-sdhc-16gb-class-10-memory-card-up-to-98mb-s-speed?_pos=7&_sid=5a2f6d137&_ss=r)

  </details>
  
  - <details>
    <summary> Raspberry Pi Zero W Case </summary>

    ###### Where to buy

    - [raspberrypi](https://www.raspberrypi.com/products/raspberry-pi-zero-case/)
    - [robocraze](https://robocraze.com/products/raspberry-pi-zero-w-wh-enclosure-case-and-camera-cable)

  </details>
  
  - <details>
    <summary> Micro USB-B Male to USB-A Female Adapter </summary>

    ###### Where to buy

    - [raspberrypi](https://www.raspberrypi.com/products/micro-usb-male-to-usb-a-female-cable/)
    - [robu](https://robu.in/product/raspberry-pi-official-micro-usb-b-male-to-usb-a-female-adapter/)

  </details>
  
  - <details>
    <summary> Mini HDMI to Standard HDMI Adapter </summary>

    ###### Where to buy

    - [raspberrypi](https://www.raspberrypi.com/products/mini-hdmi-c-male-to-standard-hdmi-a-female-adapter/)
    - [robu](https://robu.in/product/raspberry-pi-official-mini-hdmi-male-to-hdmi-female-adapter/)

  </details>

- ### Software Requirements for Print Server :computer:

  - A Banana Pi M2 Zero compatible Linux image.
  Here I am using Debian 11 (BullsEye) from the Current (main) branch with 5.10.60 Mainline kernel and Armbian 21.08.1 build script.
  Debian BullsEye is the stable Debian release at the time of writing this article. [Armbian Archive Source](https://archive.armbian.com/)
  - Tool for Flashing Linux image to SD card [balenaEtcher](https://www.balena.io/etcher/) / [Raspberry Pi Imager](https://github.com/raspberrypi/rpi-imager)
  - Printer drivers for Linux on ARM CPU - IMPORTANT :warning:
    - I used the HP Laserjet Professional m1136 MFP printer for this setup.
    - HP Printer drivers for Debian can be installed from
      1. apt package manager `sudo apt install hplip`
      2. apt-get package manager `sudo apt-get install hplip`
      3. SourceForge [hplip](https://sourceforge.net/projects/hplip/files/hplip/)
    - You should check your printer website for driver availability and installation.
    Alternatively, you can check [Open Printing PPD](https://www.openprinting.org/printers) or [foo2zjs](https://github.com/koenkooi/foo2zjs) for driver files and installation.

## Setup :hammer_and_wrench:

- ### Software Setup :technologist:

  - Download the Debian image on your PC/Laptop.
  I'll add the image that I used for this setup in the [repository](Armbian/).
  You can check out the [Armbian GitHub repository](https://github.com/armbian/build) and build your image.
  They also provide official images for most SBCs on their [website](https://www.armbian.com/download/).
  If one is not available you can also check out the [archives](https://archive.armbian.com/).
  - Download [balenaEtcher](https://www.balena.io/etcher/) or [Raspberry Pi Imager](https://github.com/raspberrypi/rpi-imager) for your OS and install it on your PC/Laptop.

- ### Hardware Setup :mechanic:

  - Insert the SD card in the micro SD card adapter/SD card reader and attach it to your PC/Laptop.
  - Format the SD card. - Optional:question:
    - Mac
      1. Open Terminal.
      2. Run `diskutil list`.
      3. Look for the disk identifier that contains the SD card. It should be in the format `/dev/diskX` where X will be a number.
      4. Run `diskutil eraseDisk FAT32 UNTITLED MBRFormat /dev/diskX` after replacing X with the disk number. (CAUTION:exclamation: - Verify the selected disk before running.)
    - Linux
      1. Open Terminal.
      2. Check the device name with `lsblk`. It should be in the format of `sdX` or `mmcblkX` where X is the disk identifier.
      3. Run `fdisk /dev/sdX` or `fdisk /dev/mmcblkX`.
      4. Enter `d` until all existing partitions are deleted.
      5. Enter `o` to create a new DOS partition table.
      6. Enter `n` to create a new partition and accept all default values by pressing enter.
      7. Enter `t` to select the partition and then Enter 7 to select the exFAT file system.
      8. Enter `w` to save changes and quit. (CAUTION:exclamation: - Verify the selected disk before running.)
      9. Run `sudo mkfs.exfat -n "SDCard" /dev/sdX1` or `sudo mkfs.exfat -n "SDCard" /dev/mmcblkXp1` after replacing X with the disk identifier. This process will write the exFAT filesystem.
    - Windows
      1. Open Command Prompt using Run as Administrator.
      2. Run `diskpart` to open the Microsoft DiskPart interactive utility.
      3. Run the `list disk` command and look for the disk number where the SD card is present.
      4. Run `select disk X` where X is the disk number.
      5. Run `clean` to empty the SD card. (CAUTION:exclamation: - Verify the selected disk using `list disk` before cleaning. Selected Disk will be marked with an "*".)
      6. Run `convert mbr` will convert it to Master Boot Record.
      7. Run `create partition primary` to create a new partition with all the available space.
      8. Run `select partition 1` to select the newly created partition.
      9. Run `active` to activate the MBR partition.
      10. Run `format fs=exfat label=SDCard quick` to quick format the partition to the exFAT filesystem and rename it to SDCard. You can enter any label of your choice.
      11. Run `assign` to assign a Drive letter.
      12. Run `exit` to close the DiskPart utility.
  - Open balenaEtcher/Raspberry Pi Imager on your PC/Laptop.
  - Select the Debian image which will be written on the SD card.
  - Select the SD card to write. (CAUTION:exclamation: - Verify the selected disk is the SD card and not any other disk before proceeding.)
  - Click on the Flash/Write button to start the process. (CAUTION:exclamation: - All existing data on the SD card will be formatted.)
  - Wait for the write and verification process to complete and then eject the SD card. We now have our Operating System ready for the Banana Pi M2 Zero.

- ### Print Server Setup :space_invader:

  - ###### Router Configuration

    - Open your router setup page in a web browser. It is usually located at `192.168.1.1`, `192.168.0.1`, `192.168.1.100`, or `10.0.0.1`.
    - Login to your router web page and find the settings for LAN or DHCP server.
    - Check if your DHCP server is enabled and check the DHCP server pool settings.
    - Change the start IP address to leave one address unassignable by DHCP. For example, if the DHCP start IP address was `192.168.1.2` then change it to `192.168.1.3`.
    This way the DHCP server cannot assign the `192.168.1.2` address to any of the devices connected to your home network. We can now use this address as a static IP for our print server.

    <br />

    | <img alt="original config" src="/Screenshots/1.%20router%20original%20config.png" /> | <img alt="updated config" src="/Screenshots/2.%20router%20updated%20config.png" /> |
    | :--: | :--: |
    | *Original Router Configuration* | *Updated Router Configuration* |

  - ###### Banana Pi Hardware Configuration
  
    - Stick the Heak sink on top of the Banana Pi M2 Zero's H3/H2+ processor to prevent the board from overheating.
    - If your Printer and Router/Wireless AP are far apart, you might need to connect an external antenna to the Pi's U.fi/IPEX connector.
    Check the [Further Enhancements](#further-enhancements-hammer_and_pick) section for the antenna details.
    - Insert the SD Card with the flashed image into the `MICRO_SD` port of the Banana Pi M2 Zero.
    - Take the micro USB-B Male to USB-A Female Adapter and connect a USB keyboard to the USB-A side. Connect the micro USB side of the adapter to the `OTG` port of the Banana Pi M2 Zero.
    - Connect the micro USB power supply to the `DC_IN` port of the Banana Pi M2 Zero.
    - Connect the mini HDMI adapter to one end of the HDMI cable and insert it into the `HDMI` port on the Banana Pi M2 Zero.
    Connect the other end of the HDMI cable to the monitor.
    - After completing this setup turn on the monitor and Banana Pi M2 Zero's power supply.
    - The Pi's LED will blink once and then stay turned on. Subsequently, the boot process will begin and the output should be visible on the monitor.
  
  - ###### Debian OS Configuration

    - Root password setup and Create a new user.

  - ###### Static IP Configuration

    - Setup Static IP on Pi.

  - ###### WiFi Configuration

    - Connect to WiFi
    - reboot
    - pi should be automatically connected to the router with the static IP. Check the router web page to confirm.

  - ###### SSH Configuration

    - Check if ssh is installed and running else install ssh.
    - Disconnect the keyboard from the USB-A side of the adapter and connect the USB printer
    - Disconnect the monitor from the mini HDMI port.
    - ssh into the pi from the PC/Laptop using user creds.

  - ###### HP Driver Configuration

    - update and upgrade
    - reboot
    - ssh again
    - install hplip
  
  - ###### SAMBA Configuration

    - install SAMBA
    - Setup smbd.conf file `guest ok = yes` `read only = no`
    - restart samba

  - ###### CUPS Configuration

    - Add our print user to the `lpadmin` user group.
    - `sudo cupsctl --remote-admin --remote-any --share-printers`
    - Note: If the first print job since power on completes successfully but the subsequent ones fail then run this command `lpadmin -p PRINTERNAME -o usb-no-reattach-default=true`
    - setup cupd.conf file
    - restart cups

  - ###### Printer Configuration

    - Run the CUPS web interface on the browser
    - Add the printer
    - Setup default options

  - ###### HP Driver Plugin Configuration

    - run hp-setup -i
    - install hp plugin
    - reboot pi
  
## Usage :point_down:

- ### Android :arrow_forward:

- ### iOS/iPadOS :iphone:

- ### macOS &#63743;

- ### Linux :penguin:

- ### Windows :window:

## Notes :memo:

- ### Documents :books:

  - [Schematic Diagram](Docs/Banana%20Pi%20M2%20Zero%20Schematic%20V1.0-R.pdf)
  - [User Guide](Docs/Banana%20Pi%20M2%20Zero%20User%20Guide.pdf)
  - [Poster](Docs/BPI%20M2%20Zero.jpg)

- ### Links :link:

  - [Banana Pi M2 Zero Wiki](https://wiki.banana-pi.org/Banana_Pi_BPI-M2_ZERO)
  - [Banana Pi M2 Zero Quick Start Guide](https://wiki.banana-pi.org/Quick_Start_Banana_pi_SBC)
  - [Banana Pi M2 Zero Getting Started](https://wiki.banana-pi.org/Getting_Started_with_M2_Zero)
  - [Banana Pi Forum](https://forum.banana-pi.org/)
  - [Banana Pi M2 Zero Dev Images by BPI](https://download.banana-pi.dev/d/ca025d76afd448aabc63/?p=%2FImages%2FBPI-M2Z&mode=list)
  - [Setup CUPS for remote access](https://askubuntu.com/a/532123)
  - [CUPS Printer Sharing](https://opensource.apple.com/source/cups/cups-408/cups/doc/help/sharing.html)
  - [Armbian Docs Getting Started](https://docs.armbian.com/User-Guide_Getting-Started/)
  - [Armbian Wireless Setup](https://docs.armbian.com/User-Guide_Getting-Started/#how-to-connect-to-wireless)
  - [Armbian Static IP Setup](https://docs.armbian.com/User-Guide_Getting-Started/#how-to-set-fixed-ip)

- ### Further Enhancements :hammer_and_pick:

  - [ ] Install RJ45 port for Ethernet connectivity.
  [Socket Reference](https://forum.banana-pi.org/t/bpi-m2-zero-ethernet-socket-reference/13787),
  [Enable Port in OS](https://stackoverflow.com/questions/73557042/how-to-enable-eth0-on-banana-pi-zero-m2),
  [Buy RJ45 port](https://robu.in/product/rj45-8p-8c-female-plug-pack-of-5/)
  - [ ] Add U.fi/IPEX to SMA antenna for better 2.4GHz WiFi connectivity. [Buy a 2.4GHz antenna](https://robocraze.com/products/6dbi-2-4ghz-5ghz-dual-band-wifi-rp-sma-antenna-20cm-u-fl-ipex-cable)
  - [ ] Add LEDs for showing boot sequence and Network connectivity. [GPIO Reference](https://github.com/TuryRx/Banana-pi-m2-zero-GPIO)
  - [ ] Setup using BuildRoot. [buildroot.org](https://buildroot.org/), [WiFi Packages](https://blog.crysys.hu/2018/06/enabling-wifi-and-converting-the-raspberry-pi-into-a-wifi-ap/), [BuildRoot GitHub](https://github.com/buildroot/buildroot)
  - [ ] Headless Initial Setup with Debian image. (Configure WiFi connectivity, Configure Static IP, Enable ssh with username and password) - Research Required :sweat:
  - [ ] [BalenaOS](https://www.balena.io/blog/wifi-enable-usb-printers-with-a-raspberry-pi-and-share-it-over-your-network/) and Cloud deployment looks quite promising.
  - [ ] SSH and CUPS print outside of the local network. Requires Dynamic DNS for handling public IP. Setup port forwarding on the router. Change the default SSH and CUPS ports.
  - [ ] Set up Avahi daemon/Bonjour for automatic printer discovery on other devices.
  - [ ] Access the scanner functionality of a Multifunction printer over the network using SANE. [SANE Reference](https://pimylifeup.com/raspberry-pi-scanner-server/), [SANE Frontend](http://www.sane-project.org/sane-frontends.html)

- ### Known Issues :lady_beetle:

  - Android <br />
    Use [Let's Print Droid](https://play.google.com/store/apps/details?id=com.blackspruce.lpd&hl=en_IN&gl=US) instead of the default android printing service to mitigate the below issues.
    Add your printer as a new CUPS server with the static IP and share all the prints to this app. Make sure the Page Description Language is selected as RAW.
    - ~~Page margin error. Content gets cut on the right side of the page for some PDFs that are not in A4 size. - Portrait~~
    - ~~Page margin error. Content gets cut on the bottom of the page PDFs/images. - Landscape~~
    - ~~If a captured image is converted to PDF using Adobe scan, Camscanner, or similar apps and filters are applied then printing such PDFs results in garbage prints.~~
  
  - iOS/iPadOS <br />
    Testing in progress
  
  - macOS <br />
    Testing in progress
  
  - Linux <br />
    Testing in progress

  - Windows <br />
    Testing in progress
