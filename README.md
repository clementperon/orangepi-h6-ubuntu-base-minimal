# orangepi-h6-ubuntu-base-minimal

Ubuntu 18.04 base minimal image tested on the Orange Pi One Plus

This is a follow up on the mainline kernel 5.0 for the H6 SBC:

* Mainline Kernel 5.0
* HDMI
* DVFS
* Gbps and fast ethernet


OS Img is available here (use 7z to unzip and burn to SD card with Win32DiskImager or Etcher):
https://mega.nz/#!NPoSkIyT!Ul1n_1WVklPXlR2nHQxEE356_2iM8WAWWm9AWNR08hg

  User: ubuntu     
  Pasw: ubuntu


|  SBC Dev Board sample  |   Orange Pi One Plus  |   Orange Pi One Plus  |   Orange Pi One Plus  |   Orange Pi One Plus  |
|------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| kernel version         |      5.1.rc3          |      5.0.1            |      5.0.2            |      5.0.2            |
| gcc version            |      7.3              |      8.2.0            |      8.2.0            |      8.2.0            |
| display                |      hdmi             |      hdmi             |      hdmi             |   hdmi (1920x100)     |
| graphical interface    | Unabled to read EDID *|      CLI              |      CLI              |      LXDE             |
| pmic                   |      axp805/6         |      axp805/6         |      axp805/6         |      axp805/6         |
| idle Temp ºC / freq    |  40 ºC / ~480 Mhz   * |  40 ºC / ~480 Mhz     |  40 ºC / ~480 Mhz     |  42 ºC / ~480 Mhz     |
| full Temp ºC / freq    |  80 ºC / 1.8 GHz    * |  78 ºC / 1.8 GHz      |  80 ºC / 1.8 GHz      |  80 ºC / 1.8 GHz      |
| RAM memory usage (avg) |     109  Mbytes       |      65  Mbytes       |      65  Mbytes       |     155  Mbytes       |
| i2c                    |      yes              |      yes              |      yes              |      yes              |
| spi                    |      no               |      spidev0.0        |      spidev0.0        |      spidev0.0        |
| Camera                 |      none             |      none             |      none             |      none             |
| Wifi                   |      none             |      none             |      none             |      none             |
| BT                     |      none             |      none             |      none             |      none             |
| ethernet               |      Gbps / 100Mbps   |    100 MBit / 1 GBit  |      Gbps             |      Gbps             |
| issues                 | reboot = shutdown     | reboot = shutdown     | reboot = shutdown     | reboot = shutdown     |
|                        |                       |                       |                       |                       |

* bootlog: https://gist.github.com/avafinger/8c4fe50e90fd3457e9b195fc970e86a1
* bootlog 5.1-rc2: https://gist.github.com/avafinger/cfb7fd51543601a48b58795bb8e98138

# HDMI workaround (not always works)

This is a workaround for the EDID wrong modeline set.
**Failing to set the correct timings for the HDMI (Desktop) will hang the board after a while!**

* Edit the file /boot/boot.cmd
* Add the line:
	gpio set ph2

Something like:

	# Recompile with:
	# cd /media/alex/boot/
	# sudo mkimage -C none -A arm -T script -d boot.cmd boot.scr
	
	gpio set ph2
	setenv fsck.repair yes
	setenv ramdisk initrd.img
	setenv kernel Image

Save it and then recompile:
	
	sudo mkimage -C none -A arm -T script -d boot.cmd boot.scr

# Installing
You need *wget* and *curl* installed to grab the files in a Linux distro or use the **img** above.

Get the latest files by running (or seee below to fetch specific Release version files):


	wget $(curl -s https://api.github.com/repos/avafinger/orangepi-h6-ubuntu-base-minimal/releases/latest | grep -oP '"browser_download_url": "\K(.*)(?=")')


then

	Unzip the file and flash it to SD card (8GB) using




# Release v1.0

* minor fixes and optimizations (journaling , heart-beat)
* DHCP enabled (default)

		user: ubuntu
		password: ubuntu


Get v1.0 files:

		wget $(curl -s https://api.github.com/repos/avafinger/orangepi-h6-ubuntu-base-minimal/releases | grep -oP '"browser_download_url": "\K(.*)(?=")' | grep v1.0)


Boot takes about 5 seconds or less depending on SD card in use. 
**Tip**: If you can't see anything on screen, type enter twice!


On first login:

	sudo apt-get update
	sudo apt-get dist-upgrade


# Release v1.1


* HDMI
* DVFS (1.8 GHz)
* i2c1 (enabled)
* spidev0.0 (enabled)

Get v1.1 files:

		wget $(curl -s https://api.github.com/repos/avafinger/orangepi-h6-ubuntu-base-minimal/releases | grep -oP '"browser_download_url": "\K(.*)(?=")' | grep v1.1)



Install:


		sudo dpkg -i linux-image-5.0.1-h6_1.0-1.deb
		sync
		sudo shutdown -h now


		
		Wait a few seconds e push the Power button (ON/OFF) for 2 ~ 5 seconds to power on the board.


# Release v1.2

Mainline Kernel 5.0.2

* HDMI
* DVFS ( 1.8 Ghz ~ **throttle at 80º C** )
* Gbps
* i2c
* spidev

**The DT has been changed for best performance so CPU will throttle only at 80º C, get a Heatsink**
The test showed the board is still stable for a high load > 1 hr, no files got corrupted.

Get v1.2 files:

		wget $(curl -s https://api.github.com/repos/avafinger/orangepi-h6-ubuntu-base-minimal/releases | grep -oP '"browser_download_url": "\K(.*)(?=")' | grep v1.2)


Install new Image:

		sudo dpkg -i linux-image-5.0.2-h6_1.0-2.deb
		sync
		sudo shutdown -h now


# Release v1.3

Mainline Kernel 5.0.2 - LXDE

* HDMI ( 1920x1080 )
* DVFS ( 1.8 Ghz ~ **throttle at 80º C** )
* Gbps
* i2c
* spidev

**The DT has been changed for best performance so CPU will throttle only at 80º C, get a Heatsink**


Get v1.3 files:


		wget $(curl -s https://api.github.com/repos/avafinger/orangepi-h6-ubuntu-base-minimal/releases | grep -oP '"browser_download_url": "\K(.*)(?=")' | grep v1.3)



Flash Image to SD CARD:


		Unzip orangepi-one-plus-lxde-8GB-sd.img.7z and flash with win32diskimager or etcher



**Tip**: You may need to update the file **resolver.conf** to reflect your network (DNS)


# Release v1.4

Mainline Kernel 5.1.0.rc2 (Experimental)

* HDMI ( enabled - some issues )
* DVFS ( 1.8 Ghz ~ **throttle at 80º C** )
* Gbps
* i2c
* spidev

**The DT has been changed for best performance so CPU will throttle only at 80º C, get a Heatsink**


Get v1.4 files:


		wget $(curl -s https://api.github.com/repos/avafinger/orangepi-h6-ubuntu-base-minimal/releases | grep -oP '"browser_download_url": "\K(.*)(?=")' | grep v1.4)


# Release v1.5

Mainline Kernel 5.1.0.rc3 (Experimental)

* HDMI ( enabled - some issues )
* DVFS ( 1.8 Ghz ~ **throttle at 80º C** )
* Gbps / 100Mbps
* i2c


**The DT has been changed for best performance so CPU will throttle only at 80º C, get a Heatsink**


Get v1.5 files:


		wget $(curl -s https://api.github.com/repos/avafinger/orangepi-h6-ubuntu-base-minimal/releases | grep -oP '"browser_download_url": "\K(.*)(?=")' | grep v1.5)



[![htop](https://github.com/avafinger/orangepi-h6-ubuntu-base-minimal/raw/master/img/htop.png)]

[![htop](https://github.com/avafinger/orangepi-h6-ubuntu-base-minimal/raw/master/img/hdmi_1920x1080.png)]


Mainline kernel 5.0 bootlog

	[    0.000000] Booting Linux on physical CPU 0x0000000000 [0x410fd034]
	[    0.000000] Linux version 5.0.0-h6 (ubuntu@opi-h6) (gcc version 8.2.0 (Ubuntu 8.2.0-1ubuntu2~18.04)) #1 SMP PREEMPT Mon Mar 4 22:56:40 UTC 2019
	[    0.000000] Machine model: Opi H6
	[    0.000000] cma: Reserved 128 MiB at 0x0000000078000000
	[    0.000000] NUMA: No NUMA configuration found
	[    0.000000] NUMA: Faking a node at [mem 0x0000000040000000-0x000000007fffffff]
	[    0.000000] NUMA: NODE_DATA [mem 0x77de2840-0x77de3fff]
	[    0.000000] Zone ranges:
	[    0.000000]   DMA32    [mem 0x0000000040000000-0x000000007fffffff]
	[    0.000000]   Normal   empty
	[    0.000000] Movable zone start for each node
	[    0.000000] Early memory node ranges
	[    0.000000]   node   0: [mem 0x0000000040000000-0x000000007fffffff]
	[    0.000000] Initmem setup node 0 [mem 0x0000000040000000-0x000000007fffffff]
	[    0.000000] On node 0 totalpages: 262144
	[    0.000000]   DMA32 zone: 4096 pages used for memmap
	[    0.000000]   DMA32 zone: 0 pages reserved
	[    0.000000]   DMA32 zone: 262144 pages, LIFO batch:63
	[    0.000000] psci: probing for conduit method from DT.
	[    0.000000] psci: PSCIv1.1 detected in firmware.
	[    0.000000] psci: Using standard PSCI v0.2 function IDs
	[    0.000000] psci: MIGRATE_INFO_TYPE not supported.
	[    0.000000] psci: SMC Calling Convention v1.1
	[    0.000000] random: get_random_bytes called from start_kernel+0x94/0x3e4 with crng_init=0
	[    0.000000] percpu: Embedded 22 pages/cpu @(____ptrval____) s52760 r8192 d29160 u90112
	[    0.000000] pcpu-alloc: s52760 r8192 d29160 u90112 alloc=22*4096
	[    0.000000] pcpu-alloc: [0] 0 [0] 1 [0] 2 [0] 3 
	[    0.000000] Detected VIPT I-cache on CPU0
	[    0.000000] CPU features: detected: ARM erratum 845719
	[    0.000000] Speculative Store Bypass Disable mitigation not required
	[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 258048
	[    0.000000] Policy zone: DMA32
	[    0.000000] Kernel command line: console=ttyS0,115200 earlyprintk root=/dev/mmcblk0p2 rw rootfstype=ext4 rootwait fsck.repair=yes panic=10
	[    0.000000] printk: log_buf_len individual max cpu contribution: 4096 bytes
	[    0.000000] printk: log_buf_len total cpu_extra contributions: 12288 bytes
	[    0.000000] printk: log_buf_len min size: 16384 bytes
	[    0.000000] printk: log_buf_len: 32768 bytes
	[    0.000000] printk: early log buf free: 14040(85%)
	[    0.000000] Memory: 885240K/1048576K available (8254K kernel code, 642K rwdata, 2348K rodata, 512K init, 222K bss, 32264K reserved, 131072K cma-reserved)
	[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=4, Nodes=1
	[    0.000000] rcu: Preemptible hierarchical RCU implementation.
	[    0.000000] 	Tasks RCU enabled.
	[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 25 jiffies.
	[    0.000000] NR_IRQS: 64, nr_irqs: 64, preallocated irqs: 0
	[    0.000000] GIC: Using split EOI/Deactivate mode
	[    0.000000] arch_timer: cp15 timer(s) running at 24.00MHz (phys).
	[    0.000000] clocksource: arch_sys_counter: mask: 0xffffffffffffff max_cycles: 0x588fe9dc0, max_idle_ns: 440795202592 ns
	[    0.000004] sched_clock: 56 bits at 24MHz, resolution 41ns, wraps every 4398046511097ns
	[    0.000242] Console: colour dummy device 80x25
	[    0.000311] Calibrating delay loop (skipped), value calculated using timer frequency.. 48.00 BogoMIPS (lpj=96000)
	[    0.000319] pid_max: default: 32768 minimum: 301
	[    0.000402] LSM: Security Framework initializing
	[    0.000831] Dentry cache hash table entries: 131072 (order: 8, 1048576 bytes)
	[    0.001039] Inode-cache hash table entries: 65536 (order: 7, 524288 bytes)
	[    0.001070] Mount-cache hash table entries: 2048 (order: 2, 16384 bytes)
	[    0.001082] Mountpoint-cache hash table entries: 2048 (order: 2, 16384 bytes)
	[    0.024036] ASID allocator initialised with 32768 entries
	[    0.032031] rcu: Hierarchical SRCU implementation.
	[    0.048061] smp: Bringing up secondary CPUs ...
	[    0.080495] Detected VIPT I-cache on CPU1
	[    0.080546] CPU1: Booted secondary processor 0x0000000001 [0x410fd034]
	[    0.112430] Detected VIPT I-cache on CPU2
	[    0.112462] CPU2: Booted secondary processor 0x0000000002 [0x410fd034]
	[    0.144485] Detected VIPT I-cache on CPU3
	[    0.144512] CPU3: Booted secondary processor 0x0000000003 [0x410fd034]
	[    0.144578] smp: Brought up 1 node, 4 CPUs
	[    0.144582] SMP: Total of 4 processors activated.
	[    0.144588] CPU features: detected: 32-bit EL0 Support
	[    0.144592] CPU features: detected: CRC32 instructions
	[    0.144843] CPU: All CPU(s) started at EL2
	[    0.144858] alternatives: patching kernel code
	[    0.145847] devtmpfs: initialized
	[    0.149768] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
	[    0.149822] futex hash table entries: 1024 (order: 4, 65536 bytes)
	[    0.154101] pinctrl core: initialized pinctrl subsystem
	[    0.155202] NET: Registered protocol family 16
	[    0.155611] audit: initializing netlink subsys (disabled)
	[    0.155745] audit: type=2000 audit(0.152:1): state=initialized audit_enabled=0 res=1
	[    0.156643] cpuidle: using governor menu
	[    0.156935] vdso: 2 pages (1 code @ (____ptrval____), 1 data @ (____ptrval____))
	[    0.156943] hw-breakpoint: found 6 breakpoint and 4 watchpoint registers.
	[    0.161325] DMA: preallocated 256 KiB pool for atomic allocations
	[    0.161882] Serial: AMBA PL011 UART driver
	[    0.175871] HugeTLB registered 1.00 GiB page size, pre-allocated 0 pages
	[    0.175880] HugeTLB registered 32.0 MiB page size, pre-allocated 0 pages
	[    0.175884] HugeTLB registered 2.00 MiB page size, pre-allocated 0 pages
	[    0.175889] HugeTLB registered 64.0 KiB page size, pre-allocated 0 pages
	[    0.176335] cryptd: max_cpu_qlen set to 1000
	[    0.177592] SCSI subsystem initialized
	[    0.177779] libata version 3.00 loaded.
	[    0.178036] usbcore: registered new interface driver usbfs
	[    0.178087] usbcore: registered new interface driver hub
	[    0.178162] usbcore: registered new device driver usb
	[    0.178488] pps_core: LinuxPPS API ver. 1 registered
	[    0.178492] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
	[    0.178511] PTP clock support registered
	[    0.178642] EDAC MC: Ver: 3.0.0
	[    0.179332] Advanced Linux Sound Architecture Driver Initialized.
	[    0.180095] clocksource: Switched to clocksource arch_sys_counter
	[    0.180260] VFS: Disk quotas dquot_6.6.0
	[    0.180318] VFS: Dquot-cache hash table entries: 512 (order 0, 4096 bytes)
	[    0.187798] NET: Registered protocol family 2
	[    0.188392] tcp_listen_portaddr_hash hash table entries: 512 (order: 1, 8192 bytes)
	[    0.188495] TCP established hash table entries: 8192 (order: 4, 65536 bytes)
	[    0.188584] TCP bind hash table entries: 8192 (order: 5, 131072 bytes)
	[    0.188731] TCP: Hash tables configured (established 8192 bind 8192)
	[    0.188858] UDP hash table entries: 512 (order: 2, 16384 bytes)
	[    0.188890] UDP-Lite hash table entries: 512 (order: 2, 16384 bytes)
	[    0.189043] NET: Registered protocol family 1
	[    0.189501] RPC: Registered named UNIX socket transport module.
	[    0.189505] RPC: Registered udp transport module.
	[    0.189508] RPC: Registered tcp transport module.
	[    0.189511] RPC: Registered tcp NFSv4.1 backchannel transport module.
	[    0.189684] Unpacking initramfs...
	[    0.239329] Freeing initrd memory: 1076K
	[    0.245226] Initialise system trusted keyrings
	[    0.245356] workingset: timestamp_bits=44 max_order=18 bucket_order=0
	[    0.252515] squashfs: version 4.0 (2009/01/31) Phillip Lougher
	[    0.253259] NFS: Registering the id_resolver key type
	[    0.253281] Key type id_resolver registered
	[    0.253284] Key type id_legacy registered
	[    0.253294] nfs4filelayout_init: NFSv4 File Layout Driver Registering...
	[    0.253463] 9p: Installing v9fs 9p2000 file system support
	[    0.259029] NET: Registered protocol family 38
	[    0.259044] Key type asymmetric registered
	[    0.259049] Asymmetric key parser 'x509' registered
	[    0.259116] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 247)
	[    0.259247] io scheduler mq-deadline registered
	[    0.259252] io scheduler kyber registered
	[    0.259627] sun50i-de2-bus 1000000.display-engine: Error couldn't map SRAM to device
	[    0.260308] sun4i-usb-phy 5100400.phy: Couldn't get regulator usb0_vbus... Deferring probe
	[    0.264434] sun50i-h6-r-pinctrl 7022000.pinctrl: initialized sunXi PIO driver
	[    0.270834] Serial: 8250/16550 driver, 4 ports, IRQ sharing enabled
	[    0.272730] cacheinfo: Unable to detect cache hierarchy for CPU 0
	[    0.277518] loop: module loaded
	[    0.279407] libphy: Fixed MDIO Bus: probed
	[    0.279648] tun: Universal TUN/TAP device driver, 1.6
	[    0.280860] PPP generic driver version 2.4.2
	[    0.281173] VFIO - User Level meta-driver version: 0.3
	[    0.281892] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
	[    0.281897] ehci-platform: EHCI generic platform driver
	[    0.282090] ehci-platform 5101000.usb: EHCI Host Controller
	[    0.282112] ehci-platform 5101000.usb: new USB bus registered, assigned bus number 1
	[    0.282813] ehci-platform 5101000.usb: irq 15, io mem 0x05101000
	[    0.296106] ehci-platform 5101000.usb: USB 2.0 started, EHCI 1.00
	[    0.296742] hub 1-0:1.0: USB hub found
	[    0.296771] hub 1-0:1.0: 1 port detected
	[    0.297361] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
	[    0.297385] ohci-platform: OHCI generic platform driver
	[    0.297548] ohci-platform 5101400.usb: Generic Platform OHCI controller
	[    0.297566] ohci-platform 5101400.usb: new USB bus registered, assigned bus number 2
	[    0.297856] ohci-platform 5101400.usb: irq 16, io mem 0x05101400
	[    0.360827] hub 2-0:1.0: USB hub found
	[    0.360865] hub 2-0:1.0: 1 port detected
	[    0.361722] usbcore: registered new interface driver usb-storage
	[    0.363062] mousedev: PS/2 mouse device common for all mice
	[    0.363740] sun6i-rtc 7000000.rtc: registered as rtc0
	[    0.363747] sun6i-rtc 7000000.rtc: RTC enabled
	[    0.363826] i2c /dev entries driver
	[    0.364016] sun50i-h6-r-pinctrl 7022000.pinctrl: 7022000.pinctrl supply vcc-pl not found, using dummy regulator
	[    0.364077] sun50i-h6-r-pinctrl 7022000.pinctrl: Linked as a consumer to regulator.0
	[    0.364667] axp20x-i2c 0-0036: AXP20x variant AXP806 found
	[    0.368963] dcdca: supplied by regulator-dummy
	[    0.370026] dcdcc: supplied by regulator-dummy
	[    0.370628] dcdcd: supplied by regulator-dummy
	[    0.371075] vdd-sys: Bringing 900000uV into 960000-960000uV
	[    0.371569] dcdce: supplied by regulator-dummy
	[    0.372167] aldo1: supplied by regulator-dummy
	[    0.372757] aldo2: supplied by regulator-dummy
	[    0.373205] vcc-ac200: Bringing 700000uV into 3300000-3300000uV
	[    0.373680] aldo3: supplied by regulator-dummy
	[    0.374124] vcc-3v3-1: Bringing 700000uV into 3300000-3300000uV
	[    0.374885] bldo1: supplied by regulator-dummy
	[    0.375887] bldo2: supplied by regulator-dummy
	[    0.376331] vcc-efuse-pcie-hdmi-io: Bringing 800000uV into 1800000-1800000uV
	[    0.376494] bldo3: supplied by regulator-dummy
	[    0.376963] vcc-dcxoio: Bringing 700000uV into 1800000-1800000uV
	[    0.377727] bldo4: supplied by regulator-dummy
	[    0.378319] cldo1: supplied by regulator-dummy
	[    0.378916] cldo2: supplied by regulator-dummy
	[    0.379383] vcc-wifi-1: Bringing 700000uV into 3300000-3300000uV
	[    0.379838] cldo3: supplied by regulator-dummy
	[    0.380292] vcc-wifi-2: Bringing 700000uV into 3300000-3300000uV
	[    0.380753] sw: supplied by regulator-dummy
	[    0.380942] axp20x-i2c 0-0036: AXP20X driver loaded
	[    0.382492] cpu cpu0: Linked as a consumer to regulator.1
	[    0.382538] cpu cpu0: Dropping the link to regulator.1
	[    0.382670] cpu cpu0: Linked as a consumer to regulator.1
	[    0.384323] sdhci: Secure Digital Host Controller Interface driver
	[    0.384329] sdhci: Copyright(c) Pierre Ossman
	[    0.384399] Synopsys Designware Multimedia Card Interface Driver
	[    0.384702] sdhci-pltfm: SDHCI platform and OF driver helper
	[    0.385362] ledtrig-cpu: registered to indicate activity on CPUs
	[    0.385520] hidraw: raw HID events driver (C) Jiri Kosina
	[    0.385715] usbcore: registered new interface driver usbhid
	[    0.385717] usbhid: USB HID core driver
	[    0.387020] NET: Registered protocol family 17
	[    0.387140] 9pnet: Installing 9P2000 support
	[    0.387183] Key type dns_resolver registered
	[    0.387575] registered taskstats version 1
	[    0.387577] Loading compiled-in X.509 certificates
	[    0.394383] sun4i-usb-phy 5100400.phy: Linked as a consumer to regulator.16
	[    0.397049] sun50i-h6-pinctrl 300b000.pinctrl: initialized sunXi PIO driver
	[    0.397287] sun50i-h6-pinctrl 300b000.pinctrl: 300b000.pinctrl supply vcc-ph not found, using dummy regulator
	[    0.397353] sun50i-h6-pinctrl 300b000.pinctrl: Linked as a consumer to regulator.0
	[    0.397647] printk: console [ttyS0] disabled
	[    0.418479] 5000000.serial: ttyS0 at MMIO 0x5000000 (irq = 13, base_baud = 1500000) is a 16550A
	[    0.418521] printk: console [ttyS0] enabled
	[    0.419067] sun50i-h6-pinctrl 300b000.pinctrl: 300b000.pinctrl supply vcc-pd not found, using dummy regulator
	[    0.419227] dwmac-sun8i 5020000.ethernet: PTP uses main clock
	[    0.524145] ehci-platform 5311000.usb: EHCI Host Controller
	[    0.524188] ehci-platform 5311000.usb: new USB bus registered, assigned bus number 3
	[    0.525185] ehci-platform 5311000.usb: irq 17, io mem 0x05311000
	[    0.540140] ehci-platform 5311000.usb: USB 2.0 started, EHCI 1.00
	[    0.541046] hub 3-0:1.0: USB hub found
	[    0.541086] hub 3-0:1.0: 1 port detected
	[    0.541955] ohci-platform 5311400.usb: Generic Platform OHCI controller
	[    0.541979] ohci-platform 5311400.usb: new USB bus registered, assigned bus number 4
	[    0.542313] ohci-platform 5311400.usb: irq 18, io mem 0x05311400
	[    0.605057] hub 4-0:1.0: USB hub found
	[    0.605085] hub 4-0:1.0: 1 port detected
	[    0.605864] usb_phy_generic usb_phy_generic.0.auto: usb_phy_generic.0.auto supply vcc not found, using dummy regulator
	[    0.605923] usb_phy_generic usb_phy_generic.0.auto: Linked as a consumer to regulator.0
	[    0.606242] musb-hdrc musb-hdrc.1.auto: MUSB HDRC host driver
	[    0.606251] musb-hdrc musb-hdrc.1.auto: new USB bus registered, assigned bus number 5
	[    0.606759] hub 5-0:1.0: USB hub found
	[    0.606785] hub 5-0:1.0: 1 port detected
	[    0.607817] thermal thermal_zone0: failed to read out thermal zone (-16)
	[    0.607872] thermal thermal_zone1: failed to read out thermal zone (-16)
	[    0.608436] sun50i-h6-pinctrl 300b000.pinctrl: 300b000.pinctrl supply vcc-pf not found, using dummy regulator
	[    0.608636] sunxi-mmc 4020000.mmc: Linked as a consumer to regulator.12
	[    0.609264] sunxi-mmc 4020000.mmc: Got CD GPIO
	[    0.634771] sunxi-mmc 4020000.mmc: initialized, max. request size: 16384 KB, uses new timings mode
	[    0.635100] sun50i-h6-pinctrl 300b000.pinctrl: 300b000.pinctrl supply vcc-pd not found, using dummy regulator
	[    0.635262] vcc-gmac-3v3: supplied by vcc-ac200
	[    0.636683] dwmac-sun8i 5020000.ethernet: PTP uses main clock
	[    0.636804] dwmac-sun8i 5020000.ethernet: Linked as a consumer to regulator.17
	[    0.671783] mmc0: host does not support reading read-only switch, assuming write-enable
	[    0.673766] mmc0: new high speed SDHC card at address 59b4
	[    0.675512] mmcblk0: mmc0:59b4 SDU1  7.52 GiB 
	[    0.677227]  mmcblk0: p1 p2
	[    0.744333] dwmac-sun8i 5020000.ethernet: Current syscon value is not the default 58000 (expect 0)
	[    0.744359] dwmac-sun8i 5020000.ethernet: No HW DMA feature register supported
	[    0.744365] dwmac-sun8i 5020000.ethernet: RX Checksum Offload Engine supported
	[    0.744371] dwmac-sun8i 5020000.ethernet: COE Type 2
	[    0.744376] dwmac-sun8i 5020000.ethernet: TX Checksum insertion supported
	[    0.744382] dwmac-sun8i 5020000.ethernet: Normal descriptors
	[    0.744387] dwmac-sun8i 5020000.ethernet: Chain mode enabled
	[    0.744529] libphy: stmmac: probed
	[    0.745994] sun6i-rtc 7000000.rtc: setting system clock to 1970-01-01T00:00:05 UTC (5)
	[    0.746312] vdd-gpu: disabling
	[    0.746652] ALSA device list:
	[    0.746655]   No soundcards found.
	[    0.747186] Freeing unused kernel memory: 512K
	[    0.764212] Run /init as init process
	[    0.786439] random: fast init done
	[    0.876143] usb 3-1: new high-speed USB device number 2 using ehci-platform
	[    0.994502] EXT4-fs (mmcblk0p2): mounted filesystem without journal. Opts: (null)
	[    1.034877] hub 3-1:1.0: USB hub found
	[    1.035220] hub 3-1:1.0: 4 ports detected
	[    1.324137] usb 3-1.1: new low-speed USB device number 3 using ehci-platform
	[    1.443660] input: USB Optical Mouse as /devices/platform/soc/5311000.usb/usb3/3-1/3-1.1/3-1.1:1.0/0003:1BCF:0007.0001/input/input0
	[    1.444189] input: USB Optical Mouse as /devices/platform/soc/5311000.usb/usb3/3-1/3-1.1/3-1.1:1.0/0003:1BCF:0007.0001/input/input1
	[    1.444485] hid-generic 0003:1BCF:0007.0001: input,hidraw0: USB HID v1.10 Mouse [USB Optical Mouse] on usb-5311000.usb-1.1/input0
	[    1.524147] usb 3-1.4: new low-speed USB device number 4 using ehci-platform
	[    1.560061] systemd[1]: System time before build time, advancing clock.
	[    1.642360] input: SIGMACHIP USB Keyboard as /devices/platform/soc/5311000.usb/usb3/3-1/3-1.4/3-1.4:1.0/0003:1C4F:0002.0002/input/input2
	[    1.669298] NET: Registered protocol family 10
	[    1.669783] Segment Routing with IPv6
	[    1.704809] hid-generic 0003:1C4F:0002.0002: input,hidraw1: USB HID v1.10 Keyboard [SIGMACHIP USB Keyboard] on usb-5311000.usb-1.4/input0
	[    1.708627] input: SIGMACHIP USB Keyboard Consumer Control as /devices/platform/soc/5311000.usb/usb3/3-1/3-1.4/3-1.4:1.1/0003:1C4F:0002.0003/input/input3
	[    1.728823] systemd[1]: systemd 237 running in system mode. (+PAM +AUDIT +SELINUX +IMA +APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ +LZ4 +SECCOMP +BLKID +ELFUTILS +KMOD -IDN2 +IDN -PCRE2 default-hierarchy=hybrid)
	[    1.729246] systemd[1]: Detected architecture arm64.
	[    1.749125] systemd[1]: Set hostname to <opi-h6>.
	[    1.768384] input: SIGMACHIP USB Keyboard System Control as /devices/platform/soc/5311000.usb/usb3/3-1/3-1.4/3-1.4:1.1/0003:1C4F:0002.0003/input/input4
	[    1.768622] hid-generic 0003:1C4F:0002.0003: input,hidraw2: USB HID v1.10 Device [SIGMACHIP USB Keyboard] on usb-5311000.usb-1.4/input1
	[    2.051663] systemd[1]: File /lib/systemd/system/systemd-journald.service:36 configures an IP firewall (IPAddressDeny=any), but the local system does not support BPF/cgroup based firewalling.
	[    2.051676] systemd[1]: Proceeding WITHOUT firewalling in effect! (This warning is only shown for the first loaded unit using IP firewalling.)
	[    2.217988] random: systemd: uninitialized urandom read (16 bytes read)
	[    2.218044] systemd[1]: Reached target Swap.
	[    2.232455] random: systemd: uninitialized urandom read (16 bytes read)
	[    2.232965] systemd[1]: Set up automount Arbitrary Executable File Formats File System Automount Point.
	[    2.248240] random: systemd: uninitialized urandom read (16 bytes read)
	[    2.248505] systemd[1]: Started Forward Password Requests to Wall Directory Watch.
	[    2.264365] systemd[1]: Reached target Remote File Systems.
	[    2.281518] systemd[1]: Created slice System Slice.
	[    2.444490] EXT4-fs (mmcblk0p2): re-mounted. Opts: (null)
	[    2.983328] systemd-journald[1247]: Received request to flush runtime journal from PID 1
	[    3.310129] sun4i-drm display-engine: bound 1100000.mixer (ops sun8i_mixer_platform_driver_exit [sun8i_mixer])
	[    3.310346] sun4i-drm display-engine: bound 6510000.tcon-top (ops sun8i_tcon_top_platform_driver_exit [sun8i_tcon_top])
	[    3.310559] sun4i-drm display-engine: bound 6515000.lcd-controller (ops sun4i_tcon_platform_driver_exit [sun4i_tcon])
	[    3.310632] sun8i-dw-hdmi 6000000.hdmi: 6000000.hdmi supply hvcc not found, using dummy regulator
	[    3.310678] sun8i-dw-hdmi 6000000.hdmi: Linked as a consumer to regulator.0
	[    3.310830] sun8i-dw-hdmi 6000000.hdmi: Detected HDMI TX controller v2.12a with HDCP (DWC HDMI 2.0 TX PHY)
	[    3.311996] sun8i-dw-hdmi 6000000.hdmi: registered DesignWare HDMI I2C bus driver
	[    3.312462] sun4i-drm display-engine: bound 6000000.hdmi (ops sun8i_dw_hdmi_ops [sun8i_drm_hdmi])
	[    3.312471] [drm] Supports vblank timestamp caching Rev 2 (21.10.2013).
	[    3.312473] [drm] No driver support for vblank timestamp query.
	[    3.312941] [drm] Initialized sun4i-drm 1.0.0 20150629 for display-engine on minor 0
	[    3.829016] Console: switching to colour frame buffer device 240x67
	[    3.857834] sun4i-drm display-engine: fb0: DRM emulated frame buffer device
	[    3.907254] EXT4-fs (mmcblk0p1): mounted filesystem with ordered data mode. Opts: (null)
	[    4.356496] dwmac-sun8i 5020000.ethernet eth0: device MAC address ca:2e:21:a0:aa:d4
	[    4.356805] RTL8211E Gigabit Ethernet stmmac-0:00: attached PHY driver [RTL8211E Gigabit Ethernet] (mii_bus:phy_addr=stmmac-0:00, irq=POLL)
	[    4.358485] dwmac-sun8i 5020000.ethernet eth0: No Safety Features support found
	[    4.358498] dwmac-sun8i 5020000.ethernet eth0: No MAC Management Counters available
	[    4.358502] dwmac-sun8i 5020000.ethernet eth0: PTP not supported by HW
	[    4.591370] zram: Added device: zram0
	[    4.592482] zram: Added device: zram1
	[    4.593320] zram: Added device: zram2
	[    4.594309] zram: Added device: zram3
	[    4.662513] zram0: detected capacity change from 0 to 130293760
	[    5.710523] Adding 127236k swap on /dev/zram0.  Priority:5 extents:1 across:127236k SS
	[    5.711811] zram1: detected capacity change from 0 to 130293760
	[    6.710603] random: crng init done
	[    6.710615] random: 7 urandom warning(s) missed due to ratelimiting
	[    6.723641] Adding 127236k swap on /dev/zram1.  Priority:5 extents:1 across:127236k SS
	[    6.724900] zram2: detected capacity change from 0 to 130293760
	[    6.735277] Adding 127236k swap on /dev/zram2.  Priority:5 extents:1 across:127236k SS
	[    6.736442] zram3: detected capacity change from 0 to 130293760
	[    6.747334] Adding 127236k swap on /dev/zram3.  Priority:5 extents:1 across:127236k SS
	[    7.432790] dwmac-sun8i 5020000.ethernet eth0: Link is Up - 1Gbps/Full - flow control rx/tx
	[    7.432822] IPv6: ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready

# Acknowledgments

* Icenowy Zheng and Jernej Skrabec (linux-sunxi) for their work on H6
* Armbian