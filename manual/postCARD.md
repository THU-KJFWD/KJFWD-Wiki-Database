---
title: 诊断卡食用教程
description: A0了为什么还不能开机？（zj先生的灵魂提问
published: true
date: 2022-11-09T07:49:29.353Z
tags: 
editor: markdown
dateCreated: 2022-11-09T07:49:27.915Z
---

# 诊断卡是啥
> 诊断卡是一种计算机内，可以显示主机板自检进程和开机自检阶段错误代码的扩展卡，通常用来排除不能开机的计算机的故障。 
> <p align="right">--翻译自英文 <a href="https://en.wikipedia.org/wiki/POST_card/">Wikipedia</a> </p>

从工作原理上来说，自IBM PC开始，计算机上引入了用于指示在系统启动期间状态或故障的8bit代码（也就是两个HEX数字，例如A0），该代码会在电脑启动时发送到特定的IO接口（通常是LPC、PCI、PCI-E甚至USB）  

> 说人话：你妈喊你早上起床，你迅速从脑子里过了一遍，今天有没有肚子疼、嗓子疼、腿疼、头疼，哦，原来今天轮到jio疼了，于是你给你妈说，妈，我脚疼！就接着睡了

# 诊断卡怎么用

### 诊断卡真的是最容易使用、且效率最高的工具了捏！

## 第一步，你需要一部安卓手机并下载[APP](http://nas.kjfwd.com:31200/sharing/nrdO5IuJ2)  

安装APP就不用教了吧

## 第二步，给诊断卡蓝牙模块供电，并且蓝连牙

请不要将诊断卡的供电接到待测试设备上，谁知道他的5V供电会不会突然没了捏
插上电之后，应该可以看到蓝色指示灯了，参考APP引导蓝连牙  

## 第三步，好多插口，诊断卡应该插在哪里捏？

> 新的主板通常不会把诊断代码发送给PCI-E总线上的设备，因此没有PCI插槽的主板可能只有LPC和USB接口可以用来插诊断卡了 
> <p align="right">--翻译自英文 <a href="https://en.wikipedia.org/wiki/POST_card/">Wikipedia</a> </p>

![postcard.jpg](/manual/img/post_postcard.jpg)

诊断卡结构如上图所示，板上集成了PCI、PCI-E和USB诊断插口，初次之外，还有一个不怎么常见的EC诊断口，该插口仅部分笔记本有集成。
除此之外，新款诊断卡还有四个扩展板，可以通过排线链接：
1. T形扩展板，用来连接台式机的LPC插座
2. M.2接口（Ekey：无线网卡）（Bkey：SATA固态硬盘，PCI-E X2带宽的其他设备，无线网卡）
3. M.2接口（Mkey：NVMe固态硬盘，PCI-E X4带宽的其他设备）（Ekey：无线网卡，PCI-E X1带宽的其他设备）
4. MiniPCI-E、mSATA转接卡，用来插古早笔记本上，属于是固态硬盘初期接口标准没有统一时的接口了

### 这里直接给出推荐顺序：

|   设备类型  |  诊断接口1  |  诊断接口2  |  诊断接口3  |  诊断接口4  |  
| :----: | :----: | :----: | :----: | :----: |
| 台式机  | USB |  PCI |  LPC |  PCI-E,M.2  |   
| 笔记本 | USB |  EC |  miniPCIE/SATA  |  M.2  |   

> 请注意！不要将PCI插口插进PCI-E槽中，100%烧毁！
> 请注意！不要将PCI-E插口插进PCI槽中，100%烧毁！
{.is-warning}  


按照以上顺序，通常来说，如果到PCI-E/M.2 插槽还是没有读码，这台设备就不支持诊断卡了。（悲，确实是有主板选择不向总线设备发送诊断代码的  

## 第四步，获得诊断代码

到这一步就可以获得两个十六进制数啦，那么他对应什么意思呢？  
  
其实APP上已经集成了四家BIOS厂家的诊断代码含义了，通常来说大家能见到的BIOS也就是这四家代工的，按照对应厂家的操作就行。现在Phoenix的相对少见一些，台式机AMI比较多，笔记本Insyde比较多。也有一些神秘厂家会用开源产品自己魔改出BIOS，比如Surface系列的UEFI就是微软自己造的，Macbook的UEFI也是果子自己改的，不过通常来说诊断代码会有一些共同性，并且上面提到的这俩没一个善茬，去售后吧真的，别折磨自己。

以下是AMI BIOS 的启动过程，翻译在做了.jpg  

<details>
<summary>AMI BIOS启动过程 1991年2月 英文<a href="http://www.bioscentral.com/postcodes/amibios.htm">Ref:BIOSCentral</a></summary>
<br>

|  Name  |  Post Procedures  |   
| :----: | :----: |  
|  NMI disable  |  NMI interrupt line to the CPU is disabled by setting bit 7 I?O port 70h (CMOS)  |   
| Power On Delay  | Once the keyboard controller gets power, it sets the hard and soft reset bits.  Check the keyboard controller or clock generator if a failure occurs |   
| Initialize Chipsets | Check the BIOS, CLOCK and chipsets |  
| Reset Determination | The BIOS reads the bits in the keyboard controller to see if a hard or soft reset is required (a soft reset will not test memory above 64K).  Failure could be the BIOS or keyboard controller |  
| ROM BIOS Checksum | 	The BIOS performs a checksum on itself and adds a preset factory value that should make it equal to 00.  If a failure occurs, check the BIOS chips |  
| Keyboard Test | 	A command is sent to the 8042 keyboard controller which performs a test and sets a buffer space for commands.  After the buffer is defined the BIOS sends a command byte, writes data to the buffer, checks the high order bits of the internal keyboard controller and issues a No Operation (NOP) command |  
| CMOS | Shutdown byte in CMOS RAM offset 0F is tested, the BIOS checksum calculated and diagnostic byte 0E updated before the CMOS RAM area is initialized and updated for date and time.  Check the RTC and CMOS chip or battery if a failure occurs |  
| DMA (8237) and PIC (8259) Disable | The DMA and Programmable Interrupt Controller are disabled before the POST proceeds and further.  Check the 8237 or 8259 chips if a failure occurs |  
| Video Disable | 	The video controller is disabled and port B initialized.  Check the video adapter if a failure occurs |  
| Chipset Initialized and Memory Detected | Memory addressed in 64K blocks.   Failure would be in the chipset.  If all memory is not seen, failure could be in a chip in the block after the last one seen |  
| PIT Test | The timing functions of the 8254 Programmable Interrupt Timer are tested.  The PIT and RTC chips normally cause errors here |  
| Memory Refresh | PIT's ability to refresh memory is tested.  If an XT, DMA controller #1 handles this.  Failure is normally the PIT (8254) in AT's or the 8237, DMA #1, in XT's |  
| Address Line | Test the address lines in the first 64K of RAM.  If a failure occurs, an address line may be the problem |  
| Base 64K | Data patterns are written to the first 64K of RAM, unless there is a bad RAM chip in which case you will get a failure |  
| Chipset Initialization | The PIT, PIC and DMA controllers are initialized |  
| Set Interrupt Table | Interrupt vector table used by PIC is installed in low memory, the first 2K |  
| 8042 Keyboard Controller Check | The BIOS reads the buffer area in the keyboard controller I/O port 60.  Failure here is normally the keyboard controller |  
| Video Tests | The type of video adapter is checked for, then a series of tests are performed on the adapter and monitor |  
| BIOS Data Area | The vector table is checked for proper operation and video memory verified before protected mode tests are entered into.   This is done so that any errors found are displayed on the monitor |  
| Protected Mode Tests | Perform reads and writes to all memory locations below 1MB.  Failure at this point indicate a bad RAM chip, the 8042 Keyboard Controller or a data line |  
| DMA Chips | The DMA registers are tested using a data pattern |  
| Final Initialization | 	these differ with each version.   Typically, the floppy and hard drives are tested and initialized and a check is made for serial and parallel devices.  The information gathered is then compared against the contents of the CMOS and you will see the results of any failures on the monitor |  
| BOOT | 	The BIOS hands over control to the Int 19 bootloader.  This is where you would see error messages such as non-system disk |  
  
  
</details> 

  

以下是AMI BIOS 代码对应的含义参考，翻译在做了.jpg

<details>
<summary>AMI BIOS Post Codes (After April 1990): <a href="http://www.bioscentral.com/postcodes/amibios.htm">Ref:BIOSCentral</a></summary>
<br>


|  code  |  meaning |   
| :----: | :----: |  
|  01	|  NMI is disabled and the i286 register test is about to start  |
|  02	|  i286 register test has passed  |
|  03	|  ROM BIOS checksum test (32KB from E8000h) passed OK  |
|  04	|  Passed keyboard controller test with and without mouse  |
|  05	|  Chipset initialized...DMA and interrupt controller disabled  |
|  06	|  Video system disabled and the system timer checks OK  |
|  07	|  8254 programmable interval timer initialized  |
|  08	|  Delta counter channel 2 initialization complete  |
|  09	|  Delta counter channel 1 initialization complete  |
|  0A	|  Delta counter channel 0 initialization complete  |
|  0B	|  Refresh started  |
|  0C	|  System timer started  |
|  0D	|  Refresh check OK  |
|  10	|  Ready to start 64KB base memory test  |
|  11	|  Address line test OK  |
|  12	|  64KB base memory test OK  |
|  15	|  ISA BIOS interrupt vectors initialized  |
|  17	|  Monochrome video mode OK  |
|  18	|  CGA color mode set OK  |
|  19	|  Attempting to pass control to video ROM at C0000h  |
|  1A	|  Returned from video ROM  |
|  1B	|  Shadow RAM enabled  |
|  1C	|  Display memory read/write test OK  |
|  1D	|  Alternate display memory read/write test OK  |
|  1E	|  Global equipment byte set for proper  |
|  1F	|  Ready to initialize video system  |
|  20	|  Finished setting video mode  |
|  21	|  ROM type 27256 verified  |
|  22	|  The power-on message is displayed  |
|  30	|  Ready to start the virtual mode memory test  |
|  31	|  Virtual memory mode test started  |
|  32	|  CPU has switched to virtual mode  |
|  33	|  Testing the memory address lines  |
|  34	|  Testing the memory address lines  |
|  35	|  Lower 1MB of RAM found  |
|  36	|  Memory size computation checks OK  |
|  37	|  Memory test in progress  |
|  38	|  Memory below 1MB is initialized  |
|  39	|  Memory above 1MB is initialized  |
|  3A	|  Memory size is displayed  |
|  3B	|  Ready to test the lower 1MB of RAM  |
|  3C	|  Memory test of lower 1MB OK  |
|  3D	|  Memory test above 1MB OK  |
|  3E	|  Ready to shutdown for real-mode testing  |
|  3F	|  Shutdown Ok - now in real mode  |
|  40	|  Cache memory now on...Ready to disable gate A 20  |
|  41	|  A20 line disabled successfully  |
|  42	|  i486 internal cache turned on  |
|  43	|  Ready to start DMA controller test  |
|  50	|  DMA page register test OK  |
|  51	|  Starting DMA controller 1 register test  |
|  52	|  DMA controller 1 test passed, starting DMA controller 2 register test  |
|  53	|  DMA controller 2 test passed  |
|  54	|  Ready to test latch on DMA controller 1 and 2  |
|  55	|  DMA controller 1 and 2 latch test OK  |
|  56	|  DMA controller 1 and 2 configured OK  |
|  57	|  8259 programmable interrupt controller initialized Ok  |
|  70	|  Start of keyboard test  |
|  71	|  Keyboard controller OK  |
|  72	|  Keyboard test OK...Starting mouse interface test  |
|  73	|  Keyboard and mouse global initialization OK  |
|  74	|  Display setup prompt.. Floppy setup ready to start  |
|  75	|  Floppy controller setup OK  |
|  76	|  hard disk setup ready to start  |
|  77	|  Hard disk controller setup OK  |
|  79	|  Ready to initialize timer data  |
|  7A	|  Timer data area initialized  |
|  7B	|  CMOS battery verified OK  |
|  7E	|  CMOS memory size updated  |
|  7F	|  Enable setup routine if Delete is pressed  |
|  80	|  Send control to adapter ROM at C800h to DE00h  |
|  81	|  Return from adapter ROM  |
|  82	|  Printer data initialization is OK  |
|  83	|  RS-232 data initialization is OK  |
|  84	|  80x87 check and test OK  |
|  85	|  Display any soft error message  |
|  86	|  Give control to ROM at E0000h  |
|  A0	|  Program the cache SRAM  |
|  A1	|  Check for external cache  |
|  A2	|  initialize EISA adapter card slots  |
|  A3	|  Test extended NMI in EISA system  |
|  00	|  Call the INT19 boot loader  |
</details>
