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
>
> <p align="right">--翻译自英文 <a href="https://en.wikipedia.org/wiki/POST_card/">Wikipedia</a> </p>

从工作原理上来说，自 IBM PC 开始，计算机上引入了用于指示在系统启动期间状态或故障的 8bit 代码（也就是两个 HEX 数字，例如 A0），该代码会在电脑启动时发送到特定的 IO 接口（通常是 LPC、PCI、PCI-E 甚至 USB）

> 说人话：你妈喊你早上起床，你迅速从脑子里过了一遍，今天有没有肚子疼、嗓子疼、腿疼、头疼，哦，原来今天轮到 jio 疼了，于是你给你妈说，妈，我脚疼！就接着睡了。要是你仔细想了一圈，没啥不舒服的，就找找衣服袜子，全都穿好了再准备出门

本文希望通过介绍诊断卡的使用，帮助大家了解现代 UEFI 的启动过程，启动过程放在了诊断代码部分。讲道理用诊断卡肥肠简单，但是搞清楚背后是怎么个原理足够讲，，，一百篇这个体量的文章，所以你可以在这篇文章里看到很多的链接，请自备科学上网方式。

### 一些你可能会感兴趣的内容

1. BIOS 和 UEFI 的一些操作上的差别，文章并不完全严谨，但是很好理解[科普贴：BIOS 和 UEFI 的启动项](https://zhuanlan.zhihu.com/p/31365115)
2. BIOS 和 UEFI 一些产生上的差别，普通用户似乎不必理会，但是为了避免一些术语上的理解错误，建议阅读[UEFI 引导与 传统 BIOS 引导在原理上有什么区别？芯片公司在其中扮演什么角色？](https://zhuanlan.zhihu.com/p/81960137)
3. 老狼老师的知乎专栏，例如解释 microcode 的文章[Microcode 是什么？它为什么能修正 CPU 硬件错误？](https://zhuanlan.zhihu.com/p/86432216)
4. 听说你想换掉你的电脑启动 logo？[如何更换 Windows 10 的启动 logo](https://zhuanlan.zhihu.com/p/28201533)

# 诊断卡怎么用

### 诊断卡真的是最容易使用、且效率最高的工具了捏！

## 第一步，你需要一部安卓手机并下载[APP](http://nas.kjfwd.com:31200/sharing/nrdO5IuJ2)

安装 APP 就不用教了吧

## 第二步，给诊断卡蓝牙模块供电，并且蓝连牙

请不要将诊断卡的供电接到待测试设备上，谁知道他的 5V 供电会不会突然没了捏
插上电之后，应该可以看到蓝色指示灯了，参考 APP 引导蓝连牙

## 第三步，好多插口，诊断卡应该插在哪里捏？

> 新的主板通常不会把诊断代码发送给 PCI-E 总线上的设备，因此没有 PCI 插槽的主板可能只有 LPC 和 USB 接口可以用来插诊断卡了
>
> <p align="right">--翻译自英文 <a href="https://en.wikipedia.org/wiki/POST_card/">Wikipedia</a> </p>

![postcard.jpg](/manual/postcard/post_postcard.jpg)

诊断卡结构如上图所示，板上集成了 PCI、PCI-E 和 USB 诊断插口，初次之外，还有一个不怎么常见的 EC 诊断口，该插口仅部分笔记本有集成。
除此之外，新款诊断卡还有四个扩展板，可以通过排线链接：

1. T 形扩展板，用来连接台式机的 LPC 插座
2. M.2 接口（Ekey：无线网卡）（Bkey：SATA 固态硬盘，PCI-E X2 带宽的其他设备，无线网卡）
3. M.2 接口（Mkey：NVMe 固态硬盘，PCI-E X4 带宽的其他设备）（Ekey：无线网卡，PCI-E X1 带宽的其他设备）
4. MiniPCI-E、mSATA 转接卡，用来插古早笔记本上，属于是固态硬盘初期接口标准没有统一时的接口了

### 这里直接给出推荐顺序：

| 设备类型 | 诊断接口 1 | 诊断接口 2 |  诊断接口 3   | 诊断接口 4 |
| :------: | :--------: | :--------: | :-----------: | :--------: |
|  台式机  |    USB     |    PCI     |      LPC      | PCI-E,M.2  |
|  笔记本  |    USB     |     EC     | miniPCIE/SATA |    M.2     |

> 请注意！不要将 PCI 插口插进 PCI-E 槽中，100%烧毁！
> 请注意！不要将 PCI-E 插口插进 PCI 槽中，100%烧毁！
> {.is-warning}

按照以上顺序，通常来说，如果到 PCI-E/M.2 插槽还是没有读码，这台设备就不支持诊断卡了。（悲，确实是有主板选择不向总线设备发送诊断代码的

## 第四步，获得诊断代码

到这一步就可以获得两个十六进制数啦，那么他对应什么意思呢？

其实 APP 上已经集成了四家 BIOS 厂家的诊断代码含义了，通常来说大家能见到的 BIOS 也就是这四家代工的，按照对应厂家的操作就行。现在 Phoenix 的相对少见一些，台式机 AMI 比较多，笔记本 Insyde 比较多。也有一些神秘厂家会用开源产品自己魔改出 BIOS，比如 Surface 系列的 UEFI 就是微软自己造的，Macbook 的 UEFI 也是果子自己改的，不过通常来说诊断代码会有一些共同性，并且上面提到的这俩没一个善茬，去售后吧真的，别折磨自己。

### 一些计算机的启动步骤和诊断代码

![biosp.jpg](/manual/postcard/biosp.jpg)

下文以 InsydeH20 BIOS 启动过程为例，简单描述从按下电源键开始到进入操作系统都经过那些步骤。实际上，大多数现代 BIOS 都有类似的规范，阅读下文的启动阶段详细描述有助于了解 UEFI 的启动过程

#### SEC 阶段

此阶段 CPU 还不能使用内存，通过将 Cache 配置为内存实现操作，之后找到[BFV(Boot firmware volume)](https://edk2-docs.gitbook.io/edk-ii-minimum-platform-specification/appendix_a_full_maps/a1_firmware_volume_layout)(链接指向 EDKII,是一个[开源](https://github.com/tianocore/edk2)的 UEFI 开发环境),BFV 是首个被执行的 firmware volume，包含 PEI CORE 和 PEIM,完活后加载 PEI CORE 并验证.  
为什么叫 Security 阶段呢？这一阶段作为后续操作系统的[信任根(RootOfTrust)](https://blog.csdn.net/weixin_49369227/article/details/120646358),作为系统启动的第一部分，只有 SEC 能被系统信任，以后的各个阶段才有被信任的基础。因此，大部分情况下 SEC 在转交控制权给 PEI 前要验证 PEI 是否可信。

| 阶段 |        功能名称        | 代码 |                                                  说明                                                   |
| :--: | :--------------------: | :--: | :-----------------------------------------------------------------------------------------------------: |
| SEC  |    SYSTEM_POWER_ON     | 0x01 |                                        CPU 开机并切换到保护模式                                         |
| SEC  | BEFORE_MICROCODE_PATCH | 0x02 |                      CPU 加载[微码](https://en.wikipedia.org/wiki/Intel_Microcode)                      |
| SEC  | AFTER_MICROCODE_PATCH  | 0x03 |                                         微码加载进端间寄存器中                                          |
| SEC  |       ACCESS_CSR       | 0x04 |                 初始[CSR 寄存器](https://en.wikipedia.org/wiki/Control/Status_Register)                 |
| SEC  |    GENERIC_MSRINIT     | 0x05 | 初始化[MSR 寄存器](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) |
| SEC  |      CPU_SPEEDCFG      | 0x06 |                        在 MSR 寄存器内配置 CPU 指令吞吐率(millisec/instruction)                         |
| SEC  |      SETUP_CAR_OK      | 0x07 |                                     cache as ram 缓存作为 RAM 测试                                      |
| SEC  |    FORCE_MAX_RATIO     | 0x08 |                                        将 CPU 频率设置为最大水平                                        |
| SEC  |    GO_TO_SECSTARTUP    | 0x09 |                                           设置 BIOS ROM 缓存                                            |
| SEC  |     GO_TO_PEICORE      | 0x0A |                                              进入 PEI 阶段                                              |

#### PEI 阶段

PEI 阶段的 CPU_AP_INIT 阶段之前，处理器中只有一个核心作为 bootstrap processor,AP 阶段后,才有其他核心作为 Application Processor 启动。PEI 阶段多数 code 仍然以 Cache 作为 RAM 执行，

| 阶段 |         功能名称         | 代码 |                                                                             说明                                                                              |
| :--: | :----------------------: | :--: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------: |
| PEI  |         SIO_INIT         | 0x70 |                                              [SIO](https://en.wikipedia.org/wiki/Super_I/O)初始化,包括热量,风扇                                               |
| PEI  |       CPU_REG_INIT       | 0x71 |                                                                        CPU 早期初始化                                                                         |
| PEI  |       CPU_AP_INIT        | 0x72 |         [application processors 初始化](https://wiki.osdev.org/Symmetric_Multiprocessing),[这里有一些其他参考](https://zhuanlan.zhihu.com/p/67989330)         |
| PEI  |       CPU_HT_RESET       | 0x73 |                           [HT 总线](https://en.wikipedia.org/wiki/HyperTransport)重置(注意于超线程区分,即使他就在 AP 初始化的下面)                            |
| PEI  |      PCIE_MMIO_INIT      | 0x74 |                                                                 PCIE Memory Mapped IO 初始化                                                                  |
| PEI  |       NB_REG_INIT        | 0x75 |                [北桥](<https://en.wikipedia.org/wiki/Northbridge_(computing)>)初始化(提一嘴，现在内存控制器都集成到 CPU 内部了，北桥已经无了)                 |
| PEI  |       SB_REG_INIT        | 0x76 |               [南桥](<https://en.wikipedia.org/wiki/Southbridge_(computing)>)初始化(南桥多数功能也集成到 CPU 了,芯片组应该叫做平台控制器集线器)               |
| PEI  |      PCIE_TRAINING       | 0x77 |                              PCIE 链路训练,[为什么 PCIE 和 DDR 需要链路训练](https://www.cnblogs.com/hammerqiu/p/10643692.html)                               |
| PEI  |         TPM_INIT         | 0x78 |                                              [TPM](https://en.wikipedia.org/wiki/Trusted_Platform_Module) 初始化                                              |
| PEI  |        SMBUS_INIT        | 0x79 |                                       系统控制总线([SMBus](https://en.wikipedia.org/wiki/System_Management_Bus))初始化                                        |
| PEI  |    PROGRAM_CLOCK_GEN     | 0x7A |                  一些环形振荡器启动[一些参考链接](https://www.renesas.cn/cn/zh/document/apn/rl78f13-f14-clock-generator-rev100?language=en)                   |
| PEI  |    IGD_EARLY_INITIAL     | 0x7B |                                                                   处理器集成显卡早期初始化                                                                    |
| PEI  |        HECI_INIT         | 0x7C |                                      [HECI](https://en.wikipedia.org/wiki/Host_Embedded_Controller_Interface)总线初始化                                       |
| PEI  |      WATCHDOG_INIT       | 0x7D |                             [监控计时器](https://uefi.org/sites/default/files/resources/Watchdog%20Descriptor%20Table.pdf)初始化                              |
| PEI  |       MEMORY_INIT        | 0x7E |                                                                          内存初始化                                                                           |
| PEI  |  MEMORY_INIT_FOR_CRISIS  | 0x7F |                                                                 电脑寄了之后重启的内存初始化                                                                  |
| PEI  |      MEMORY_INSTALL      | 0x80 |                                                                    简单的内存测试(微星:易)                                                                    |
| PEI  |          TXTPEI          | 0x81 | [intel TXT](https://www.intel.com/content/www/us/en/developer/articles/guide/intel-trusted-execution-technology-intel-txt-enabling-guide.html) 功能早期初始化 |
| PEI  |       SWTTCH_STACK       | 0x82 |                                                                  开始配置 DRAM 作为内存使用                                                                   |
| PEI  |       MEMORY_STACK       | 0x83 |                                                                    将 DRAM 配置为缓存使用                                                                     |
| PEI  |   ENTER_RECOVERY_MODE    | 0x84 |                   恢复设备初始化,此处为[BIOS 的恢复](https://www.supermicro.com/manuals/other/UEFI_BIOS_Recovery.pdf),注意与系统的恢复区分                    |
| PEI  |   RECOVERY_MEDIA_MODE    | 0x85 |                                                                         找到恢复镜像                                                                          |
| PEI  | RECOVERY_MEDIA_NOT_FOUND | 0x86 |                                                                        找不到恢复镜像                                                                         |
| PEI  | RECOVERY_LOAD_FILE_DONE  | 0x87 |                                                                       加载恢复镜像完成                                                                        |
| PEI  |   RECOVERY_START_FLASH   | 0x88 |                                                                     启动 flash BIOS 功能                                                                      |
| PEI  |     PEI_ENTER_DXEIPL     | 0x89 |                                                                    将 BIOS 镜像加载到 RAM                                                                     |
| PEI  |     FINDING_DXE_CORE     | 0x8A |                                                                         寻找 DXE 内核                                                                         |
| PEI  |      GO_TO_DXE_CORE      | 0x8B |                                                                         进入 DXE 内核                                                                         |

#### DXE 阶段

DXE(Drive Execution Environment)阶段中，大多数硬件完成初始化，之后产生 EFI system table,提供各种 service 给后续阶段使用。最后由 DXE dispatcher 把控制权移交给 BDS 阶段来启动操作系统。

<details>
<summary>EFI System Table是啥？</a></summary>
<br>EFI System Table 由Active Console.EFI Boot Service Table,DXE service,Handle database和EFI Runtime Service Table,Version Information,System Configuration Table组成
和之前的部分在进入操作系统后就没用了，而和之后的部分在EFI引导的操作系统还是有用的，如果是Legacy引导则没有用
</details>

| 阶段 |       功能名称        | 代码 |                                                          说明                                                          |
| :--: | :-------------------: | :--: | :--------------------------------------------------------------------------------------------------------------------: |
| DXE  |        TCGDXE         | 0x40 |                                                 DXE 阶段的 TPM 初始化                                                  |
| DXE  |      SB_SPI_INIT      | 0x41 |                                                    南桥 SPI 初始化                                                     |
| DXE  |       CF9_RESET       | 0x42 |                                    设置重置服务(翻译存疑,原文 Setup Reset Service)                                     |
| DXE  |  SB_SERIAL_GPIO_INIT  | 0x43 |                                                    南桥 GPIO 初始化                                                    |
| DXE  |       SMMACCESS       | 0x44 |                                                  设置 SMMACCESS 服务                                                   |
| DXE  |        NB_INIT        | 0x45 |                                                   北桥中间阶段初始化                                                   |
| DXE  |       SIO_INIT        | 0x46 |                                                 超级 IO 中间阶段初始化                                                 |
| DXE  |     LEGACY_REGION     | 0x47 |              设置传统内存区域,[一些参考](https://uefi.org/sites/default/files/resources/PI_Spec_1_6.pdf)               |
| DXE  |        SB_INIT        | 0x48 |                                                   南桥中间阶段初始化                                                   |
| DXE  | IDENTIFY_FLASH_DEVICE | 0x49 |                                                    识别 flash 设备                                                     |
| DXE  |       FTW_INIT        | 0x4A |                                                       容错写验证                                                       |
| DXE  |     VARIABLE_INIT     | 0x4B |                                                     变量服务初始化                                                     |
| DXE  |  VARIABLE_INIT_FAIL   | 0x4C |                                                   初始化变量服务失败                                                   |
| DXE  |       MTC_INIT        | 0x4D |                                                       MTC 初始化                                                       |
| DXE  |       CPU_INIT        | 0x4E |                                                   CPU 中间阶段初始化                                                   |
| DXE  |      MP_CPU_INIT      | 0x4F |                                                 多处理器中间阶段初始化                                                 |
| DXE  |      SMBUS_INIT       | 0x50 |                                                  SMBus 驱动程序初始化                                                  |
| DXE  |   SMART_TIMER_INIT    | 0x51 |                                [8259](https://en.wikipedia.org/wiki/Intel_8259) 初始化                                 |
| DXE  |      PCRTC_INIT       | 0x52 |                  [RTC](https://zh.m.wikipedia.org/zh-hk/%E5%AF%A6%E6%99%82%E6%99%82%E9%90%98) 初始化                   |
| DXE  |       SATA_INIT       | 0x53 |                                                   SATA 控制器初始化                                                    |
| DXE  |  SMM_CONTROLER_INIT   | 0x54 |               [SMM 控制器](https://zh.m.wikipedia.org/zh-hk/%E5%AF%A6%E6%99%82%E6%99%82%E9%90%98)初始化                |
| DXE  |   LEGACY_INTERRUPT    | 0x55 |                                                      传统中断服务                                                      |
| DXE  |    RELOCATE_SMBASE    | 0x56 |                                                      迁移 SMMBASE                                                      |
| DXE  |       FIRST_SMI       | 0x57 |                                           SMI 测试(参考上文 smm 控制器部分)                                            |
| DXE  |       VTD_INIT        | 0x58 |                                     VTD 初始化(注意与 intel 的 VT-xCPU 虚拟化区别)                                     |
| DXE  |   BEFORE_CSM16_INIT   | 0x59 |                                    传统 BIOS 初始化(看到 CSM 就知道这不是个好同志)                                     |
| DXE  |   AFTER_CSM16_INIT    | 0x5A |                                                   传统中断功能初始化                                                   |
| DXE  |    LOAD_ACPI_TABLE    | 0x5B |                                                     ACPI 表初始化                                                      |
| DXE  |      SB_DISPATCH      | 0x5C |                                              南桥 SMM dispatch 服务初始化                                              |
| DXE  |    SB_IOTRAP_INIT     | 0x5D |                                                 南桥 IOTRAP 服务初始化                                                 |
| DXE  |    SUBCLASS_DRIVER    | 0x5E |                                                      建立 AMT 表                                                       |
| DXE  |       PPM_INIT        | 0x5F |                                                       PPM 初始化                                                       |
| DXE  |     HECIDRV_INIT      | 0x60 | [HECI](https://en.wikipedia.org/wiki/Host_Embedded_Controller_Interface)DRV(Host Embedded Controller Interface) 初始化 |

#### BDS & POST BDS 阶段

BDS 阶段我们才进入到真正寻找操作系统这件事,从 DXE dispatcher 接管控制权后，需要链接磁盘，之后根据配置的变量去选择设备启动，如果启动失败则会返回到 DXE dispatcher.

| 阶段 |          功能名称           | 代码 |                           说明                            |
| :--: | :-------------------------: | :--: | :-------------------------------------------------------: |
| BDS  |          ENTER_BDS          | 0x10 |                       进入 BDS 阶段                       |
| BDS  |       INSTALL_HOTKEY        | 0x11 |                      安装快捷键服务                       |
| BDS  |          ASF_INIT           | 0x12 |                        ASF 初始化                         |
| BDS  |    PCI_ENUMERATION_START    | 0x13 |                         PCI 枚举                          |
| BDS  |    BEFORE_PCIIO_INSTALL     | 0x14 |                     PCI 资源分配完毕                      |
| BDS  |     PCI_ENUMERATION_END     | 0x15 |                       PCI 枚举完成                        |
| BDS  |     CONNECT_CONSOLE_IN      | 0x16 |        键盘鼠标初始化(没错!现在才有键盘鼠标的事情)        |
| BDS  |     CONNECT_CONSOLE_OUT     | 0x17 |                      视频设备初始化                       |
| BDS  |       CONNECT_STD_ERR       | 0x18 |                    错误报告设备初始化                     |
| BDS  |       CONNECT_USB_HC        | 0x19 |                   USB 主机控制器初始化                    |
| BDS  |       CONNECT_USB_BUS       | 0x1A |                    USB 总线驱动初始化                     |
| BDS  |     CONNECT_USB_DEVICE      | 0x1B |                       USB 设备链接                        |
| BDS  |      NO_CONSOLE_ACTION      | 0x1C |                   控制台设备初始化失败                    |
| BDS  |  DISPLAY_LOGO_SYSTEM_INFO   | 0x1D |                         显示 logo                         |
| BDS  |    START_IDE_CONTROLLER     | 0x1E |                     IDE 控制器初始化                      |
| BDS  |    START_SATA_CONTROLLER    | 0x1F |                     SATA 控制器初始化                     |
| BDS  |  START_ISA_ACPI_CONTROLLER  | 0x20 |                     SIO 控制器初始化                      |
| BDS  |        START_ISA_BUS        | 0x21 |                  ISA 总线驱动程序初始化                   |
| BDS  |        START_ISA_FDD        | 0x22 |                      软盘设备初始化                       |
| BDS  |      START_ISA_SEIRAL       | 0x23 |                        串口初始化                         |
| BDS  |        START_IDE_BUS        | 0x24 |                      IDE 设备初始化                       |
| BDS  |       START_AHCI_BUS        | 0x25 |                      AHCI 设备初始化                      |
| BDS  |     CONNECT_LEGACY_ROM      | 0x26 | 调度[备用 ROMs](https://en.wikipedia.org/wiki/Option_ROM) |
| BDS  |  ENUMERATE_ALL_BOOT_OPTION  | 0x27 |                  获得所有启动设备的信息                   |
| BDS  |    END_OF_BOOT_SELECTION    | 0x28 |                      启动项选择结束                       |
| BDS  |         ENTER_SETUP         | 0x29 |                       进入设置菜单                        |
| BDS  |     ENTER_BOOT_MANAGER      | 0x2A |                     进入引导项管理器                      |
| BDS  |     BOOT_DEVICE_SELECT      | 0x2B |                     尝试引导操作系统                      |
| BDS  | EFI64_SHADOW_ALL_LEGACY_ROM | 0x2C |                     尝试驱动其他 ROM                      |
| BDS  |         ACPI_S3SAVE         | 0x2D |            将 S3 睡眠所需的数据保存在 DRAM 中             |
| BDS  |     READY_TO_BOOT_EVENT     | 0x2E |                   准备好进入操作系统力!                   |
| BDS  |       GO_LEGACY_BOOT        | 0x2F |                  legacy 模式引导操作系统                  |
| BDS  |        GO_UEFI_BOOT         | 0x30 |                   UEFI 模式引导操作系统                   |
| BDS  |  LEGACY16_PREPARE_TO_BOOT   | 0x31 |                  legacy 模式启动操作系统                  |
| BDS  |     EXIT_BOOT_SERVICES      | 0x32 |            通过 HECI 向 ME 发送 POST 过程结束             |
| BDS  |      LEGACY_BOOT_EVENT      | 0x33 |      legacy 模式进入操作系统前的最后一次芯片组初始化      |
| BDS  |    ENTER_LEGACY_16_BOOT     | 0x34 |                legacy 模式顺利启动操作系统                |
| BDS  |    RECOVERY_START_FLASH     | 0x35 |                       BIOS 开始刷写                       |

POST BDS

|   阶段   |     功能名称     | 代码 |                                  说明                                   |
| :------: | :--------------: | :--: | :---------------------------------------------------------------------: |
| POST_BDS |  NO_BOOT_DEVICE  | 0xF9 |                              没得启动设备                               |
| POST_BDS |   START_IMAGE    | 0xFB |                              UEFI 引导镜像                              |
| POST_BDS |   ENTER_INT19    | 0xFD | 旧版 [INT19](http://blog.chinaunix.net/uid-480653-id-2114769.html) 入口 |
| POST_BDS | JUMP_BOOT_SECTOR | 0xFE |                           尝试使用 INT19 启动                           |

以下是 AMI BIOS 的启动过程，由于此 BIOS 过于古早，翻译部分弃坑

<details>
<summary>AMI BIOS启动过程 1991年2月 英文<a href="http://www.bioscentral.com/postcodes/amibios.htm">Ref:BIOSCentral</a></summary>
<br>
  
|  Name  |  Post Procedures  |  启动阶段 |  
| :----: | :----: | :----: |  
|  NMI disable  |  NMI interrupt line to the CPU is disabled by setting bit 7 I?O port 70h (CMOS)  |  不可屏蔽中断禁用，类似于复位信号恢复 |  
| Power On Delay  | Once the keyboard controller gets power, it sets the hard and soft reset bits.  Check the keyboard controller or clock generator if a failure occurs |  检查键盘控制器电源，检查键盘控制器或时钟发生器是否失效  |  
| Initialize Chipsets | Check the BIOS, CLOCK and chipsets | 初始化芯片组、时钟和BIOS |  
| Reset Determination | The BIOS reads the bits in the keyboard controller to see if a hard or soft reset is required (a soft reset will not test memory above 64K).  Failure could be the BIOS or keyboard controller | 读取键盘确认是否需要软/硬重置，软重置不会重置64K以上的存储，其他可能导致软/硬重置的因素可能是BIOS或者键盘 |  
| ROM BIOS Checksum | 	The BIOS performs a checksum on itself and adds a preset factory value that should make it equal to 00.  If a failure occurs, check the BIOS chips | BIOS执行BIOS程序校验和自检，若校验和检测阶段出错，检查BIOS存储芯片 |  
| Keyboard Test | 	A command is sent to the 8042 keyboard controller which performs a test and sets a buffer space for commands.  After the buffer is defined the BIOS sends a command byte, writes data to the buffer, checks the high order bits of the internal keyboard controller and issues a No Operation (NOP) command | 向8042键盘控制器发送测试命令并设置命令缓存，之后会向缓存写数据并检查键盘是否会响应"误操作"命令 |  
| CMOS | Shutdown byte in CMOS RAM offset 0F is tested, the BIOS checksum calculated and diagnostic byte 0E updated before the CMOS RAM area is initialized and updated for date and time.  Check the RTC and CMOS chip or battery if a failure occurs |  |  
| DMA (8237) and PIC (8259) Disable | The DMA and Programmable Interrupt Controller are disabled before the POST proceeds and further.  Check the 8237 or 8259 chips if a failure occurs | 在后续POST过程中禁用<a href="https://nec.edu.np/faculty/pramodg/8237_DMA.pdf">DMA</a>和<a href="https://en.wikipedia.org/wiki/Intel_8259">PIC</a>控制器，如果有此阶段的错误代码检查8237和8259芯片 |  
| Video Disable | 	The video controller is disabled and port B initialized.  Check the video adapter if a failure occurs | 禁用视频控制器（ |  
| Chipset Initialized and Memory Detected | Memory addressed in 64K blocks.   Failure would be in the chipset.  If all memory is not seen, failure could be in a chip in the block after the last one seen |  |  
| PIT Test | The timing functions of the 8254 Programmable Interrupt Timer are tested.  The PIT and RTC chips normally cause errors here |  |  
| Memory Refresh | PIT's ability to refresh memory is tested.  If an XT, DMA controller #1 handles this.  Failure is normally the PIT (8254) in AT's or the 8237, DMA #1, in XT's |  |  
| Address Line | Test the address lines in the first 64K of RAM.  If a failure occurs, an address line may be the problem |  |  
| Base 64K | Data patterns are written to the first 64K of RAM, unless there is a bad RAM chip in which case you will get a failure |  |  
| Chipset Initialization | The PIT, PIC and DMA controllers are initialized |  |  
| Set Interrupt Table | Interrupt vector table used by PIC is installed in low memory, the first 2K |  |  
| 8042 Keyboard Controller Check | The BIOS reads the buffer area in the keyboard controller I/O port 60.  Failure here is normally the keyboard controller |  |  
| Video Tests | The type of video adapter is checked for, then a series of tests are performed on the adapter and monitor |  |  
| BIOS Data Area | The vector table is checked for proper operation and video memory verified before protected mode tests are entered into.   This is done so that any errors found are displayed on the monitor |  |  
| Protected Mode Tests | Perform reads and writes to all memory locations below 1MB.  Failure at this point indicate a bad RAM chip, the 8042 Keyboard Controller or a data line |  |  
| DMA Chips | The DMA registers are tested using a data pattern |  |  
| Final Initialization | 	these differ with each version.   Typically, the floppy and hard drives are tested and initialized and a check is made for serial and parallel devices.  The information gathered is then compared against the contents of the CMOS and you will see the results of any failures on the monitor |  |  
| BOOT | 	The BIOS hands over control to the Int 19 bootloader.  This is where you would see error messages such as non-system disk |  |  
  
  
</details>

以下是 AMI BIOS 代码对应的含义参考，由于此 BIOS 过于古早，翻译部分弃坑

<details>
<summary>AMI BIOS Post Codes (After April 1990): <a href="http://www.bioscentral.com/postcodes/amibios.htm">Ref:BIOSCentral</a></summary>
<br>

| code |                                meaning                                |
| :--: | :-------------------------------------------------------------------: |
|  01  |     NMI is disabled and the i286 register test is about to start      |
|  02  |                     i286 register test has passed                     |
|  03  |          ROM BIOS checksum test (32KB from E8000h) passed OK          |
|  04  |        Passed keyboard controller test with and without mouse         |
|  05  |      Chipset initialized...DMA and interrupt controller disabled      |
|  06  |         Video system disabled and the system timer checks OK          |
|  07  |             8254 programmable interval timer initialized              |
|  08  |            Delta counter channel 2 initialization complete            |
|  09  |            Delta counter channel 1 initialization complete            |
|  0A  |            Delta counter channel 0 initialization complete            |
|  0B  |                            Refresh started                            |
|  0C  |                         System timer started                          |
|  0D  |                           Refresh check OK                            |
|  10  |                 Ready to start 64KB base memory test                  |
|  11  |                         Address line test OK                          |
|  12  |                       64KB base memory test OK                        |
|  15  |                ISA BIOS interrupt vectors initialized                 |
|  17  |                       Monochrome video mode OK                        |
|  18  |                         CGA color mode set OK                         |
|  19  |           Attempting to pass control to video ROM at C0000h           |
|  1A  |                        Returned from video ROM                        |
|  1B  |                          Shadow RAM enabled                           |
|  1C  |                   Display memory read/write test OK                   |
|  1D  |              Alternate display memory read/write test OK              |
|  1E  |                 Global equipment byte set for proper                  |
|  1F  |                   Ready to initialize video system                    |
|  20  |                      Finished setting video mode                      |
|  21  |                        ROM type 27256 verified                        |
|  22  |                   The power-on message is displayed                   |
|  30  |              Ready to start the virtual mode memory test              |
|  31  |                   Virtual memory mode test started                    |
|  32  |                   CPU has switched to virtual mode                    |
|  33  |                   Testing the memory address lines                    |
|  34  |                   Testing the memory address lines                    |
|  35  |                        Lower 1MB of RAM found                         |
|  36  |                   Memory size computation checks OK                   |
|  37  |                        Memory test in progress                        |
|  38  |                    Memory below 1MB is initialized                    |
|  39  |                    Memory above 1MB is initialized                    |
|  3A  |                       Memory size is displayed                        |
|  3B  |                  Ready to test the lower 1MB of RAM                   |
|  3C  |                      Memory test of lower 1MB OK                      |
|  3D  |                       Memory test above 1MB OK                        |
|  3E  |                Ready to shutdown for real-mode testing                |
|  3F  |                    Shutdown Ok - now in real mode                     |
|  40  |           Cache memory now on...Ready to disable gate A 20            |
|  41  |                    A20 line disabled successfully                     |
|  42  |                     i486 internal cache turned on                     |
|  43  |                  Ready to start DMA controller test                   |
|  50  |                       DMA page register test OK                       |
|  51  |                Starting DMA controller 1 register test                |
|  52  | DMA controller 1 test passed, starting DMA controller 2 register test |
|  53  |                     DMA controller 2 test passed                      |
|  54  |             Ready to test latch on DMA controller 1 and 2             |
|  55  |                 DMA controller 1 and 2 latch test OK                  |
|  56  |                 DMA controller 1 and 2 configured OK                  |
|  57  |         8259 programmable interrupt controller initialized Ok         |
|  70  |                        Start of keyboard test                         |
|  71  |                        Keyboard controller OK                         |
|  72  |           Keyboard test OK...Starting mouse interface test            |
|  73  |              Keyboard and mouse global initialization OK              |
|  74  |          Display setup prompt.. Floppy setup ready to start           |
|  75  |                      Floppy controller setup OK                       |
|  76  |                    hard disk setup ready to start                     |
|  77  |                     Hard disk controller setup OK                     |
|  79  |                    Ready to initialize timer data                     |
|  7A  |                      Timer data area initialized                      |
|  7B  |                       CMOS battery verified OK                        |
|  7E  |                       CMOS memory size updated                        |
|  7F  |               Enable setup routine if Delete is pressed               |
|  80  |             Send control to adapter ROM at C800h to DE00h             |
|  81  |                        Return from adapter ROM                        |
|  82  |                   Printer data initialization is OK                   |
|  83  |                   RS-232 data initialization is OK                    |
|  84  |                        80x87 check and test OK                        |
|  85  |                    Display any soft error message                     |
|  86  |                     Give control to ROM at E0000h                     |
|  A0  |                        Program the cache SRAM                         |
|  A1  |                       Check for external cache                        |
|  A2  |                  initialize EISA adapter card slots                   |
|  A3  |                   Test extended NMI in EISA system                    |
|  00  |                      Call the INT19 boot loader                       |

</details>

# 参考文献

未严格按照标准参考文献格式排版，见谅

Intel SDM 白皮书，没什么好说的，看的一知半解
[Intel® 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)

一款开源的 UEFI builder Spec,一样看的很懵.
[EDK II Build Specification](https://edk2-docs.gitbook.io/edk-ii-build-specification/)

UEFI 官方文档
[UEFI Specification 2.10](https://uefi.org/specs/UEFI/2.10/index.html#)

宏碁的某款笔记本说明书
[ACER 522 用户手册](https://manualsbrain.com/zh/manuals/1197982)
