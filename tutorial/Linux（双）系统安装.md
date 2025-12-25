# Linux（双）系统安装

## I. 什么是 Linux？

**狭义上**：  
Linux 特指 **Linux Kernel（Linux 内核）**。

**广义上**：  
Linux 指基于 Linux 内核的一整套操作系统，通常称为：

> **Linux 发行版（Linux Distribution / Distro）**

---

### Linux 发行版分类

常见的有：

- **Debian 系**（最常见）：Debian、Ubuntu（及衍生：Linux Mint）、Kali Linux、Raspbian（树莓派）

- **Red Hat 系**（企业级）：RHEL、CentOS、Fedora

- **SUSE 系**（企业级）：openSUSE、SUSE Linux Enterprise

- **Arch 系** ~~（小药娘系）~~：Arch Linux、Manjaro

- **AOSP 系**：Android（Linux内核）

- **OpenWRT**（路由器用）

我们科服日常给客户装的，**绝大多数是 Debian 系，尤其 Ubuntu**。

---

### Linux 内核是干嘛的？

Linux 内核相当于是：

> **大脑 + 管家**

- 让应用程序正常使用硬件（CPU / 内存 / 网卡）
- 负责资源分配与保护
- 保证系统稳定、安全运行

---

## II. 为啥客户要用 Linux？

- 我：  
  > cnm Windows 天天蓝屏，装开发环境急死人

- 生医的客户：  
  > 我的专业软件只能跑在 Linux 上

- 计院的客户：  
  > 跑 nn 用 Linux 更好

- 自动化的客户：  
  > 机器人上位机要用 Linux

---

### Linux 双系统 / WSL2 / 虚拟机：到底装哪个？

在给客户装 Linux 之前，**本质上要先回答一个问题**：

> **客户真正需要的是什么样的 Linux 环境？**

常见选择有三种：

- **Linux + Windows 双系统**
- **WSL2（Windows Subsystem for Linux）**
- **虚拟机（VirtualBox / VMware / ESXi 等）**

下面从**适用场景、优劣和什么时候只能选它**来对比。

---

#### ✅ Linux （双）系统

**适合人群：**

- 需要 **原生 Linux 桌面环境**
- 需要 **完整硬件支持**（显卡 / 网卡 / USB / 机器人 / 采集卡）
- 能接受折腾分区、引导
- 用的是 **较新的 Linux 发行版（如 Ubuntu 22.04 / 24.04）**

**优点：**

- 性能最好，**无中间层**
- 硬件支持完整（显卡、USB、PCIe 等）
- Linux 就是“主系统”，环境最真实

**缺点：**

- **电源管理策略并非以该设备为设计目标**  
  Linux 的电源管理是通用方案，对很多消费级笔记本并非“原厂调校”，常见问题包括：
  - 续航明显短于 Windows
  - 休眠 / 唤醒不稳定
  - 风扇策略激进或失控

- **触控板 / 手势 / 键盘热键体验较弱**  
  尤其是轻薄本：
  - 多指手势不如 Windows / macOS
  - FN 热键、亮度调节偶发失效
  - 高刷新率屏幕调度不稳定

- **显卡体验不确定性高（尤其是混合显卡）**  
  - 核显 + 独显切换逻辑复杂
  - NVIDIA 闭源驱动与内核 / Wayland 耦合度高
  - 更新内核或驱动可能直接炸桌面

**⚠️ 特别提醒：**

> **新电脑 + 老 Linux（如 Ubuntu 18.04）≈ 灾难**

即便能强行装上，也可能出现：

- 没有核显 / 独显驱动
- 没有 Wi-Fi / 蓝牙
- 触摸板 / 键盘异常
- 电源管理和性能调度严重失效

这种情况下，**~~不建议双系统~~**推荐虚拟机。

---

#### ⚠️ WSL2

**适合人群：**

- **只需要命令行**
- 做点开发 / 编译 / 跑脚本
- 不依赖 GUI
- 不操作复杂硬件

**优点：**

- 安装成本最低
- 不涉及分区 / 引导
- 和 Windows 文件互通方便

**缺点（也是不推荐的原因）：**

- **GUI 支持不完整，体验割裂**
- **硬件访问能力有限**（USB、串口、机器人等很麻烦）
- 网络栈抽象层多，调试困难
- 文件系统权限模型诡异
- 跨文件系统性能问题明显

**结论一句话：**

> WSL2 不是 Linux 桌面解决方案，它只是一个“能跑 Linux 命令的环境”。

---

#### ✅ 虚拟机

**适合人群：**

- **新电脑，但必须用老 Linux**（如 Ubuntu 18.04）
- 只需要跑特定软件 / 教学环境
- 对硬件直通要求不高
- 希望 **即开即用、即关即删**
- **苹果用户**

**优点：**

- **对宿主硬件要求低**
- 老系统也能稳定运行
- 不用担心驱动缺失
- 出问题直接删虚拟机重来
- 也可以识别usb设备等

**缺点：**

- 性能略低（但够用）
- GUI 流畅度不如双系统
- 部分硬件访问受限

---

## III. 安装Linux双系统需要会的亿点事

- ✅ 安装系统 / 双系统（重点）
- ✅ 基础故障排查
- ✅ 驱动问题
- ✅ 简单网络配置
- ✅ Nvidia 驱动
- ✅ Out-of-tree 驱动（DKMS 报错）

---

### 开始装系统

### 装什么？

- **Ubuntu Desktop**
- **LTS 版本**（目前是 24.04.x）
- **amd64**
- Server 版让工程师装（TUI 不好教）

---

## 安装前准备

### ISO 选择

选择与客户电脑年限相符且适合其需求的系统

科服有 PE 盘, 里面有一些有 Ubuntu 的 ISO，如Ubuntu22和Ubuntu24

在 [TUNA](https://mirrors.tuna.tsinghua.edu.cn "TUNA 清华大学开源软件镜像站") 的「常用发行版 ISO 和应用软件安装包直接下载」可以找到 Ubuntu 的 ISO（<https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases> ）。

## Ubuntu 镜像

### 24.04.3 系列（当前 LTS）

![Ubuntu24.04.3系列](Linux/tuna-ubuntu-images.png =600x)

> 对我们来说，通常选择的镜像为**24.04.3 (amd64, Desktop LiveDVD)**：
>
> - `amd64`：x86-64 架构  
> - `Desktop`：桌面版  
> - `LiveDVD`：可以直接从光盘 / U 盘启动  
>
> `24.04` 是大版本，`.3` 是小版本。越新越好：预装软件更新，装完之后需要升级的东西就越少。
---

## 检查客户的电脑是哪年的

这个很重要！！！因为：**新电脑 + 旧 Linux = 缺驱动地狱**。  
以 Ubuntu 24.04.3 LTS 为例，大致可以这么粗分：

- **2023 年以前的电脑**：  
  一般没啥问题，24.04.3 直接上就行。

- **2023–2024 年的电脑**：  
  可能需要 HWE 内核（稍微新一点的内核）才有驱动。

**如何查看当前内核版本：**

  ```bash
  uname -r
  ```

  >输出示例：6.14.0-36-generic

  如何判断是不是 HWE 内核：

  Ubuntu 24.04 默认 GA 内核是 6.8.x

  高于默认 GA 版本号的内核（如 6.9 / 6.10 / 6.14），一般就是 HWE 内核

  版本名里通常显示为 generic，但版本号本身更重要

- **2024.04 以后新出的电脑**：  
  只能靠 HWE 碰运气，运气再差点，甚至要 mainline kernel 才有驱动。

  如何升级到 HWE 内核（需要联网）：

  ```bash
  sudo apt update
  sudo apt install \
  linux-image-generic-hwe-24.04 \
  linux-headers-generic-hwe-24.04 \
  reboot
  ```

如果一个客户拿着 2024 年的新电脑，却要求装 **Ubuntu 20.04 LTS**，那就可以考虑直接劝退 (x)。  
一般装完会变成：

- 没有核显驱动，卡得要死
- 没有 Wi-Fi 驱动，只能用有线
- 没有蓝牙驱动，用不了
- 没有触摸板驱动，触摸板完全没反应 / 特别难用
- 甚至：没有合适的 CPU 驱动，性能调度有大问题

---

## 装机盘准备

可以把 ISO 拷贝进 PE 盘备用；  
当然，也可以用 `dd` / Rufus 之类的工具，**直接把 ISO 写进 U 盘** 做启动盘。

> 对于装Ubuntu22和24，你只需要从科服一号柜拿出蓝色或黑色U盘即可，该款U盘有type-c和USB-A双头接口


## BIOS设置

我们只讲 UEFI 启动, 不讲传统 BIOS 启动 (这样的客户让他换电脑x)

首先查看客户的个人微机，判定其使用的品牌型号，查找在开机时按什么按键进入BIOS设置选项

## 各 OEM 笔记本进入 BIOS / 启动菜单按键速查表

> 进入 BIOS 的时机：**开机后、系统启动前，不停连按**（不是长按）

| 厂商（OEM） | 进入 BIOS | 启动菜单（Boot Menu） | 备注 |
|---|---|---|---|
| **联想 Lenovo** | `F2` / `Fn + F2` | `F12` | 部分机型有侧边 **NOVO** 键 |
| **ThinkPad** | `F1` | `F12` | 老型号有 `Enter → F1` |
| **戴尔 Dell** | `F2` | `F12` | 出现 Logo 后狂按 |
| **惠普 HP** | `Esc → F10` | `Esc → F9` | 先 `Esc` 再选 |
| **华硕 ASUS** | `F2` | `Esc` / `F8` | 新机多为 `F2` |
| **宏碁 Acer** | `F2` | `F12` | 需在 BIOS 里启用 `F12` |
| **微星 MSI** | `Del` | `F11` | |
| **技嘉 GIGABYTE** | `F2` / `Del` | `F12` | |
| **雷蛇 Razer** | `F1` | `F12` | |
| **微软 Surface** | **音量 + + 电源** | 无传统菜单 | 关机状态按 |
| **三星 Samsung** | `F2` | `Esc` | |
| **东芝 Toshiba** | `F2` | `F12` | |
| **索尼 VAIO** | `Assist` | `Assist` | 独立物理键 |
| **神舟 HASEE** | `F2` / `Del` | `F7` / `F12` | 主板差异大 |
| **机械革命** | `F2` | `F12` | |
| **雷神 / 未来人类** | `F2` | `F12` | 模具机居多 |
| **七彩虹** | `F2` / `Del` | `F11` | |

> 看完了吗，相信你已经进BIOS了。其实科服盘里有**一键进入BIOS.bat**

为了后续的操作顺手，建议改以下设置项

1. 安全启动（secure boot），一般在Advanced/Security/Boot等选项里去找。以华硕ROG Strix B250-F Gaming为例，进BIOS—Boot—Secure boot菜单下，进入key Management里，选择Clear Secure Boot keys，重启进BIOS即可看到Secure Boot已关闭  
2. 关闭RST  

## 关闭 RST / VMD

适用场景：  

- 安装时看不到 NVMe / 硬盘  
- 安装程序直接提示需关闭iRST

由于关闭RST可能导致Windows无法启动，所以要先在Windows中开启安全模式。具体操作步骤如下：

1. 在Windows中，按下Win+R，输入msconfig，回车，选择引导选项卡，勾选安全引导，点击应用。
2. 重启电脑，进入BIOS，根据下面的指引关闭RST
3. 保存退出后，电脑会自动进入Windows安全模式。此时再打开msconfig，取消勾选安全引导，点击应用，重启电脑即可正常进入Windows系统。接下来即可继续安装步骤。

---

### 常见名词对照

| BIOS 里可能看到的名字 | 实际含义 |
|---|---|
| Intel RST | Intel Rapid Storage Technology |
| VMD Controller | NVMe 的 RST / RAID 控制器 |
| RAID / RAID On | Intel RST 模式 |
| AHCI | 标准 SATA / NVMe 模式（Linux 友好） |

---

### OEM 设置路径速查表

| 厂商 | BIOS 路径（常见） | 改法 |
|---|---|---|
| **联想 Lenovo** | `Configuration → Storage → SATA Mode` | `Intel RST` → **AHCI** |
| **ThinkPad** | `Config → Storage` / `Config → Serial ATA` | `RST` → **AHCI** |
| **戴尔 Dell** | `System Configuration → SATA Operation` | `RAID On` → **AHCI** |
| **惠普 HP** | `Advanced（可能进BIOS没看到Advanced，这时候要点击左上角开启Advanced） → Storage Configuration` | `RAID` → **AHCI** |
| **华硕 ASUS** | `Advanced → Intel Rapid Storage` | `Enable VMD` → **Disable** |
| **宏碁 Acer** | `Main → SATA Mode` | `RST` → **AHCI** |
| **微星 MSI** | `Advanced → Integrated Peripherals` | `RAID` → **AHCI** |
| **技嘉 GIGABYTE** | `Settings → IO Ports` | `Intel RST` → **Disable** |
| **雷蛇 Razer** | `Advanced → Storage` | `RST/VMD` → **Disable** |
| **神舟 / 蓝天模具** | `Advanced → SATA Configuration` | `RAID` → **AHCI** |
| **机械革命** | `Advanced → Storage Configuration` | `RAID` → **AHCI** |

---

### 更改启动顺序（Change Boot Order）  
在Boot选项中，将科服U盘提到最上面（选中启动项并按F5、F6来上下移动）  
最终呈现结果：

```text
SanDisk 3.2Gen1
Windows Boot manager
xxx
```

没问题就可以保存（F10）退出了  

---

## 开始装Linux系统

> 对于双系统, 一定要 先装 Win, 再装 Linux

### 分区

首先进入科服U盘后，你会看到一系列的ISO镜像，先选择进入FirePE或者WePE给磁盘分区

**询问好客户准备如何使用Linux系统，将原有磁盘分区挤出来一部分给Ubuntu来用**，使用DiskGenius对本地磁盘右键，选择缩小分区，选择合适的字节即可挤出一部分空闲分区

![DiskGenius划分分区](Linux/dg-divide-partition.png =600x)

> 需要注意的是, 我们进行分区操作的时候, 如果是 NTFS / ReFS, 则应该使用 Win PE / 磁盘管理 / DiskGenius 等工具进行操作; 如果是 Ext4 / Btrfs / XFS 等 Linux 文件系统, 则应该使用 Linux 里面的 GParted, resize2fs, btrfs filesystem resize 等方法操作. **不要在 Windows 里面操作 Linux 分区, 也不要在 Linux 里面操作 Windows 分区**
>
> 原因有二: 一方面, 那些会检查分区完整性的软件 (如 DiskGenius) 在操作 Ext4 的时候的检查比 fsck.ext4 更严格, 一个能通过 fsck 检查并用 resize2fs 成功扩展的分区, 可能在 DG 里面检查就不通过; 能通过 chksdk 的在 ntfsresize 里面也可能不通过; 另一方面, 即便检查通过, 这样操作导致文件系统损坏的风险也更高.

### 进入安装镜像

分区结束后保存重启，回到科服U盘选镜像这一步，选择Ubuntu22或24，轻按键盘上的Enter键。  

这时你会看到一个黑色底色，题头为GNU GRUB version 2.12的界面，有 **Try or Install Ubuntu** 等备选项

![GRUB启动界面](Linux/grub-boot-menu.png =600x)

可能需要设置`nomodeset`参数以防止显卡驱动问题导致无法进入安装界面（此修改仅用于本次启动，不会保存）。

**操作步骤：**

1. 选中`Try or Install Ubuntu`，轻按键盘上的Enter键，进入Live CD
2. 如果无法进入，使用`safe graphics`选项启动  
3. 如果还是不行，继续下面的步骤：回到GRUB界面：

- 按下键盘上的`e`键进入 GRUB 编辑模式
- 找到以`linux`开头的那一行，定位到行尾的`quiet splash`参数：
- `quiet`：静默启动
- `splash`：显示启动画面
- 删除这两个参数，然后在行尾添加`nomodeset`（禁用内核模式设置，让内核使用基本的显卡驱动启动）
- 修改前：`... quiet splash`
- 修改后：`... nomodeset`
- 按下`F10`键或`Ctrl + X`组合键启动进入 Ubuntu 安装界面

### 开始安装

进入Ubuntu安装界面后，会弹出一个Try or Install Ubuntu的窗口，此时先退出安装程序————直接点击右上角的`X`关闭窗口，进入Live桌面环境。
点击左下角的九宫格图标，找到并打开`GParted`分区工具，检查之前挤出的空闲分区是否存在，确认空闲分区标识（如nvme0n1p4），无误后关闭GParted。  

> 注：在Linux系统中，硬盘设备通常以`/dev/`开头，后面跟随设备类型和编号。常见的设备命名规则如下：
>
>- /dev/sda, nvme0n1, vda分别代表不同类型的硬盘设备：
>
- SATA 盘和 USB 盘一般是 /dev/sdX, 比如 /dev/sda, /dev/sdb…
- NVMe 盘一般是 /dev/nvmeXnY, 比如 /dev/nvme0n1, /dev/nvme1n1…
- VirtIO (虚拟磁盘) 一般是 /dev/vdX, 比如 /dev/vda, /dev/vdb…

> p 后面的数字代表分区号，比如 /dev/sda1 是第一块硬盘的第一个分区，/dev/nvme0n1p4 是第一块 NVMe 硬盘的第四个分区。

> **设备与根文件系统简介**
>
> 在 Linux 中，所有内容被抽象为一棵文件树，硬件设备也以文件的形式出现在 `/dev` 下。常见的设备命名规则：
>
> - `/dev/sdX`：SATA / USB 磁盘（例如 `/dev/sda`, `/dev/sdb`）
> - `/dev/nvmeXnY`：NVMe 磁盘（例如 `/dev/nvme0n1`）
> - `/dev/vdX`：VirtIO 虚拟磁盘（例如 `/dev/vda`）
> - `p` 后面的数字表示分区号，例如 `/dev/sda1` 是第一块盘的第一个分区，`/dev/nvme0n1p4` 是第一块 NVMe 磁盘的第四个分区。
>
> 下面是根目录（`/`）下常见的一级目录及其用途，作为快速参考：
>
> - `/bin`：基础系统命令（现在通常为 `/usr/bin` 的链接）
> - `/boot`：启动加载器与内核文件
> - `/dev`：设备文件（表示硬件设备）
> - `/etc`：系统配置文件
> - `/home`：普通用户的家目录（如 `/home/username`）
> - `/lib`：系统库文件和内核模块（常为 `/usr/lib` 的链接）
> - `/media`：自动挂载的可移动设备（如 U 盘）
> - `/mnt`：临时挂载点
> - `/opt`：可选第三方软件
> - `/proc`：虚拟文件系统，动态显示内核和进程信息
> - `/root`：`root` 用户的家目录
> - `/run`：运行时数据
> - `/sbin`：系统管理命令（通常需要 root 权限）
> - `/srv`：服务数据（例如网站文件）
> - `/sys`：系统硬件信息（类似 `/proc`）
> - `/tmp`：临时文件；重启后可能被清空
> - `/usr`：用户程序和数据（如 `/usr/bin`）
> - `/var`：可变数据（日志、邮件等）

双击桌面上的`Install Ubuntu 24.04 LTS`图标，开始安装Ubuntu系统。

按照安装向导的步骤进行操作：  

1. 选择语言（English）
2. 选择键盘布局（如US English）
3. 选择安装类型（最小安装 Minimal installation）、不联网安装
4. 选择不安装第三方软件（除非客户有特别需求）
5. 选择安装类型：**选择“其他选项（Something else）”进行手动分区**  
6. 在分区界面，选择之前挤出的空闲分区，点击`+`按钮创建新分区：
   ![Ubuntu分区界面](Linux/ubuntu-partition-setup.png =600x)
   - EFI 分区, 512MB, FAT32, 挂载点选择`/boot/efi`，标记为`EFI System Partition`.如果是双系统安装，EFI 分区直接用 Windows 的就行，不需要再创建新的EFI分区
   - 根分区（/）：建议大小至少20GB，文件系统选择`ext4`，挂载点选择`/`
    ![Ubuntu创建分区](Linux/ubuntu-create-partition.png =600x)
    ![Ubuntu挂载分区点](Linux/ubuntu-mount-points.png =600x)
   - 交换分区（swap）：建议大小为物理内存的1-2倍，如果客户的内存 16GB, 就放个 4GB，如果内存 32GB 及以上，可以不创建交换分区，直接用交换文件，或者给2GB即可
   - 其他分区（如/home）：根据客户需求创建

   最后应该如图所示：
   ![Ubuntu分区完成示例](Linux/ubuntu-partition-complete-example.png =600x)
7. 如果客户的磁盘上已经有分区表, 选中磁盘, 点 “New Partition Table” (新建分区表);  
![Ubuntu新建分区表](Linux/ubuntu-new-partition-table.png =600x)
点一下底下的 “Device for Bootloader Installation” 选项, 选中你要装系统的硬盘 (不是分区, 是整个硬盘). 此时安装工具会自动帮你建立一个 EFI 分区.
![Ubuntu选择引导安装位置](Linux/ubuntu-bootloader-installation-device.png =600x)
可以点击这个分区然后点 “Change” 改大小和挂载点为 `/boot/efi`。如果你已经手动创建了 EFI 分区, 那么这里就选中你刚才创建的那个 EFI 分区即可.

>冷知识: ubuntu的安装程序只接受在第一个flag为boot esp的分区上安装引导: 要指定安装的分区需要先把原本esp分区的flag去掉: 并手动在目标esp分区上添加flag（）
>同时又因为os-probe似乎只搜索带flag的分区: 所以在开始安装后需要再把原esp的flag加上: 以免grub里没有windows（）

![Ubuntu修改分区flag](Linux/ubuntu-change-partition-flag.png =600x)

8. 点击`安装现在（Install Now）`，确认分区更改并继续

### 用户设置

搞完之后, Next, 接受风险, 分区, 然后就开始账户设置

- Your name: 可以写中文 / 符号等, 是一个显示用的  
- Your conputer’s name: 电脑在网络上的标识, 这个不太重要, 只能用英文数字-
- Username: 系统中的用户名 (登录用), 只能用英文数字-, 建议别太长
- Password: 密码, 建议复杂一点
- Require my password to log in: 每次开机都需要密码登录
- ~~Log in automatically: 自动登录，不需要密码~~  
![Ubuntu用户设置](Linux/ubuntu-user-setup.png =600x)

### 时区设置

在地图上点一下中国, 时区名应该是 Asia/Shanghai

等待安装完成后, 重启电脑, 拔掉U盘, 进入你刚装好的Ubuntu系统桌面.  恭喜你, 你已经成功安装了Ubuntu双系统!

## 后续工作

**联网**：点击右上角的网络图标，选择 Wi-Fi 或有线连接，输入密码连接网络。

1. 首先打开终端, 更新软件源和系统，修改为清华源:  
    Ubuntu22及以下版本:

   ```bash
   sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak # 备份原 sources.list   
   sudo nano /etc/apt/sources.list  # 编辑 sources.list 文件
   ```

   在打开的文件中，将所有的 `http://archive.ubuntu.com/ubuntu/` 和 `http://security.ubuntu.com/ubuntu/` 替换为 `https://mirrors.tuna.tsinghua.edu.cn/ubuntu/`。Ctrl+X保存，按Y并退出后，运行以下命令更新软件包列表并升级系统：

   ```bash
   sudo apt update && sudo apt upgrade -y && sudo apt dist-upgrade -y
   ```

    Ubuntu24及以上版本:

    ```bash
    sudo mv /etc/apt/sources.list.d/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources.bak # 备份原 ubuntu.sources
    sudo nano /etc/apt/sources.list.d/ubuntu.sources  # 编辑 ubuntu.sources 文件
    ```

    在打开的文件中，替换为以下内容。

    ```bash
    Types: deb
    URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu
    Suites: noble noble-updates noble-backports
    Components: main restricted universe multiverse
    Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
    
    Types: deb
    URIs: http://security.ubuntu.com/ubuntu/
    Suites: noble-security
    Components: main restricted universe multiverse
    Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
    ```

    ```bash
    sudo apt update && sudo apt upgrade -y && sudo apt full-upgrade -y
    ```

2. 安装NVIDIA驱动（如果有NVIDIA显卡）
点击左下角的九宫格图标，搜索并打开`Software Upadter（软件和更新）`，切换到`Additional Drviers（附加驱动）`选项卡，选择nvidia-open-xxx，点击`应用更改`进行安装。安装完成后，重启电脑使驱动生效。

3. 重启电脑，检查驱动是否安装成功

   ```bash
   nvidia-smi
   ```

   如果看到 NVIDIA GPU 的信息，说明驱动安装成功。

4. 安装特定的Out-of-tree驱动或者软件（如果需要）
   根据客户需求，安装特定的Out-of-tree驱动或软件
   如果有deb包，可以直接用`sudo dpkg -i package.deb`或者`sudo apt install ./package.deb`安装。
   如果是源码包，解压后进入目录，通常需要运行：

   ```bash
   sudo install build-essential dkms linux-headers-$(uname -r) make gcc    # 安装编译工具和内核头文件
   sudo ./configure    # 配置编译环境
   sudo make           # 编译源码
   sudo make install   # 安装驱动
   sudo make dkms    # 如果支持DKMS，安装到DKMS
   ```

   或者按照README中的说明进行安装。
   如果安装失败，或者遇到依赖问题，可以尝试：

   ```bash
   sudo apt --fix-broken install
   ```

   实在装不上的话卸载

   ```bash
   sudo apt purge package-name -y     # deb包卸载
   
   sudo make uninstall   # 源码包卸载
   ```

5. 修改默认grub启动项为 Windows
    编辑grub配置文件：
  
    ```bash
    sudo nano /etc/default/grub
    ```
  
    找到`GRUB_DEFAULT=0`，将`0`改为对应Windows的启动项编号（从0开始计数，一般为2）。保存并退出后，更新grub：
  
    ```bash
    sudo update-grub
    ```
  
    重启电脑，默认会进入Windows。

6. 设置Ubuntu时间同步Windows时间，避免双系统时间不同步问题：

   ```bash
   sudo timedatectl set-local-rtc 1 --adjust-system-clock
   ```

7. 自动挂载NTFS分区
    编辑`/etc/fstab`文件：
  
    ```bash
    sudo nano /etc/fstab
    ```
  
    添加以下行（将`/dev/sdXY`替换为实际的NTFS分区设备名，`/mnt/windows`替换为挂载点）：
  
    ```text
    /dev/sdXY  /mnt/windows  ntfs-3g  defaults,uid=1000,gid=1000,dmask=027,fmask=137  0  0
    ```
  
    保存并退出后，创建挂载点并挂载：
  
    ```bash
    sudo mkdir -p /mnt/windows
    sudo mount -a
    ```
  
    这样每次启动时，NTFS分区会自动挂载。

参考资料：  
给科服的Linux课程（<https://aajax.top/2025/10/02/LinuxLessonForKJFWD/）>
