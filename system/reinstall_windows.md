---
title: Windows系统重装
description: 快进到重开
published: true
date: 2023-03-17T14:25:28.259Z
tags: 
editor: markdown
dateCreated: 2022-11-30T05:50:38.526Z
---

# Windows系统重装

## 前言
Windows系统在长期使用后往往出现问题。系统特性使然，完美修复极为困难，尤其是涉及到系统更新失败、注册表损坏等情况。

重新安装可以得到一个**干净的系统**，但也意味着需要重新安装应用、配置环境等。重装与否依赖主观判断，如有疑问，欢迎咨询科服队员。

## 重装步骤

重新安装Windows时，会出现一些选项（详情请查阅[微软官方文档](https://support.microsoft.com/zh-cn/windows/%E9%87%8D%E6%96%B0%E5%AE%89%E8%A3%85-windows-d8369486-3e33-7d9c-dccc-859e2b022fc7#WindowsVersion=Windows_11)）
1. **重置系统 / 安装媒体重新安装**：系统未明显损坏，可考虑重置。
2. 对于**重置系统**，**保留个人文件 / 删除所有内容**：系统盘空余空间足够，可考虑保留文件。
3. 对于**安装媒体重新安装**，选择**Windows 10** / **11**？多数情况下，兼容性问题不大，以个人使用习惯为主。

![reset.png](/system/reinstall_image/reset.png =600x)
【图：典型的重置方法】

本文主要介绍：<u>采用安装媒体，删除所有内容</u>的安装方式。（以Windows 10为例）

> **数据无价**，注意备份！这是重装的最大风险。
即使选择“保留文件”，仍然强烈建议**提前备份重要内容**。
{.is-warning}

### 备份数据
1. 主要备份：**系统盘数据**（Windows安装分区）
1. 备份到**其他分区**，或**外置储存**（U盘/移动硬盘）
1. 最常见数据源：**用户文件夹**，路径为`%USERPROFILE%` (如`C:\Users\<用户名>`)
	- 包含：桌面、下载、文档、图片、视频、音乐等
	- **重点内容**：论文、实验数据、作业、微信聊天记录（`%USERPROFILE%\Documents\WeChat Files`）等
	- 如用户文件夹已经移动到其他分区，无需重复备份
   
### U盘启动

此时需准备好Ventoy安装盘（制作方法见[双系统安装](/system/bisystem)中的“使用 Ventoy”）

#### Step 1. 修改BIOS设置
1. 开机，按特定键进BIOS，按键一般为<kbd>F2</kbd> / <kbd>F10</kbd> / <kbd>Delete</kbd>
1. 关闭**安全启动**（Secure Boot）
1. 修改启动顺序，**USB HDD**放第一位（可能包含U盘品牌字样）
    
![old_bios.png](/system/reinstall_image/old_bios.png =x350) ![new_bios.png](/system/reinstall_image/new_bios.png =x350)
【左图：较老的BIOS，纯键盘操作；右图：较新的BIOS，键鼠操作】

#### Step 2. U盘引导
选择需要安装的Windows 10/11镜像（通常是**64位中文版**）
![ventoy.png](/system/reinstall_image/ventoy.png =600x)
【图：Ventoy引导界面】

### Step 3. 安装程序
安装流程：（不存在的步骤可跳过）
1. 若提示输入密钥，选择“我没有产品密钥”（进入系统后激活）
1. 选择版本
![install_version.png](/system/reinstall_image/install_version.png =600x)
【校园正版激活，选择**教育版**；笔记本重装，可选**家庭版**】
1. 接受许可条款
1. 选择“自定义：仅安装Windows”（不要选“升级”）
1. 磁盘分区
![install_partition.png](/system/reinstall_image/install_partition.png =600x)
1. 等待安装流程完成（可能会多次重启）

### Step 4. 开箱体验（OOBE）
进入桌面前的最后一步，主要完成账户设置。
联网可能触发自动更新，消耗时间未知。如提示联网，建议先离线操作。
![oobe_login.png](/system/reinstall_image/oobe_login.png =600x)
（仅联网出现）微软账户登陆选项。教育版系统强制要求工作账户，一般只能选择“改为域加入”（本地账户）。
![oobe_username.png](/system/reinstall_image/oobe_username.png =600x)
本地账户名不要包含中文、空格，否则可能导致部分软件报错。
![oobe_password.png](/system/reinstall_image/oobe_password.png =600x)
输入用户密码。（也可以先保留空白）
![postoobe.png](/system/reinstall_image/postoobe.png =600x)
其余选项均可保留默认值，直到进入此界面，稍作等待。
![predesktop.png](/system/reinstall_image/predesktop.png =600x)
进桌面前的最后一幕，典型的微软式中文。
### Step 5. 进桌面后
![desktop.png](/system/reinstall_image/desktop.png)
#### 安装驱动
对于笔记本，网卡驱动最为重要，联网后Windows会自动安装大部分其他驱动。
如无法联网，可采取以下方法：
1. 确认电脑型号，队员代为下载官网驱动，U盘拷贝安装。
1. 若有队员下载过完整版Snappy Driver Installer (SDI)，可采用该软件离线安装。（仅安装网卡驱动即可）
1. 采用有线网络连接。224房间有网线，连接后免认证。（部分电脑可能需要USB-网口转换器）

联网后建议下载厂商对应的维护软件，完善驱动安装。具体包括（待补充）：
- **联想**：下载安装[一键安装驱动](https://newsupport.lenovo.com.cn/driveDownloads_index.html)
- **Dell**：下载安装[Support Assist](https://www.dell.com/support/home/zh-cn?app=drivers)
- **ASUS**：下载安装[MyASUS](https://www.asus.com.cn/support/MyASUS-deeplink/)

#### 网络校时
建议更改时间服务器，使用清华大学TUNA协会提供的NTP，地址为`ntp.tuna.tsinghua.edu.cn`
![ntp.png](/system/reinstall_image/ntp.png =600x)
#### 系统激活
清华大学信息化用户服务平台提供了[Windows 10](https://software.tsinghua.edu.cn/thcic_microsoft/windows10/Active_Windows10.zip)和[Windows 11](https://software.tsinghua.edu.cn/thcic_microsoft/windows11/Active_Windows11.zip)的激活脚本，可按照[操作说明](https://software.tsinghua.edu.cn/thcic_microsoft/windows10/Windows10ActiveReadme.pdf)进行激活。（科服的Win10默认已激活）
![unactivated.png](/system/reinstall_image/unactivated.png =600x)
Win10/11的教育版密钥相同：`NW6C2-QMPVW-D7KKK-3GKT6-VCFB2`
![activation.png](/system/reinstall_image/activation.png =350x) ![activated.png](/system/reinstall_image/activated.png =350x)
完成激活。
#### 登陆微软账户
![account.png](/system/reinstall_image/account.png =350x) ![msacount.png](/system/reinstall_image/msacount.png =350x)
如上图所示。若系统原来没有密码，“现有密码”留空即可。
## 补充知识

### 启动U盘制作

Ventoy启动盘制作方法见[【双系统安装】](/system/bisystem)中的“使用 Ventoy”
**镜像下载**
1. 在[信息化服务平台its](https://its.tsinghua.edu.cn/ggrj.jsp?urltype=tree.TreeTempUrl&wbtreeid=1053)或[科服NAS](http://nas.kjfwd.com:31200/)上下载
![its.png](/system/reinstall_image/its.png =600x)
![nas.png](/system/reinstall_image/nas.png =400x)
1. 在微软官网上下载：[Win11](https://www.microsoft.com/zh-cn/software-download/windows11)或[Win10](https://www.microsoft.com/zh-cn/software-download/windows10)
![ms_win11.png](/system/reinstall_image/ms_win11.png =400x)
![ms_win10_mediatool.png](/system/reinstall_image/ms_win10_mediatool.png =400x)
Win10需要采用Media Creation Tool创建ISO，过程如下：
![mediatool1.png](/system/reinstall_image/mediatool1.png =400x)
![mediatool2.png](/system/reinstall_image/mediatool2.png =400x)
![mediatool3.png](/system/reinstall_image/mediatool3.png =400x)

**[Rufus](https://rufus.ie/zh/)**
良心小软，轻松创建USB启动盘，超快
![rufus.png](/system/reinstall_image/rufus.png =400x)

### 第三方安装器
解包安装，如dism++
![dism_extract.png](/system/reinstall_image/dism_extract.png =400x)
![dism_edition.png](/system/reinstall_image/dism_edition.png =400x)

### Windows引导补充知识

1. EFI与Legacy引导
	- MBR与GPT分区表
1. 四个常见分区
	![install_partition.png](/system/reinstall_image/install_partition.png =600x)
  - EFI分区（启动分区，100MB，无需手动创建/删除）
  - MSR分区（系统保留，16MB~128MB，无需手动创建/删除）
  - 系统盘（可占满剩余空间，推荐100G以上）
  - 恢复分区（500~600MB，无需手动创建/删除）包含Windows恢复环境

### 常见bug集锦

1. **无法关闭Secure Boot**
> 新的Ventoy启动盘已经支持Secure Boot，但需按照[官网教程](https://www.ventoy.net/cn/doc_secure.html)操作，才能正常引导U盘。
{.is-info}

- **BIOS无关闭“安全启动”选项**
![bios_clear_key.png](/system/reinstall_image/bios_clear_key.png =600x)

- **Linpus lite boot failed**（常见于联想电脑）
说明安全启动的方案不兼容，依旧参考上方[Ventoy官网教程](https://www.ventoy.net/cn/doc_secure.html)。
实测部分电脑，修改BIOS设置（关闭OS Optimized Defaults等）也能解决问题。

2. **安装程序出错**
- 读不出硬盘：大概率是Intel RST相关
见下图，在BIOS关闭（改为AHCI）。
![bios_rst.jpeg](/system/reinstall_image/bios_rst.jpeg =350x)
或下载[官网RST驱动](https://www.intel.cn/content/www/cn/zh/download/15667/intel-rapid-storage-technology-intel-rst-user-interface-and-driver.html)zip文件，解压后可在Windows安装程序中加载。
- 只能安装到GPT磁盘
![gpt_fail.png](/system/reinstall_image/gpt_fail.png =350x)
![dg_gpt.png](/system/reinstall_image/dg_gpt.png =350x)
- 磁盘可能很快会出现故障
- 不明错误

  
3. **Win11专属错误**（最新版Ventoy全局控制插件中，均有选项绕过）
- **不满足Win11的系统要求**：建议安装Win10。
	- 新版Ventoy的VTOY_WIN11_BYPASS_CHECK选项默认开启，跳过硬件检查
- **安装后无法离线设置/创建本地账户**：在OOBE界面Shift+F10打开CMD，输入OOBE\BYPASSNRO并回车，重启后即可离线设置
	- 网传采用no@thankyou.com登陆微软账户，可以触发本地账户
	- 有线连接拔网线也可，无线连接用netsh禁用网卡也可）
	- 新版Ventoy的VTOY_WIN11_BYPASS_NRO选项默认开启，允许离线完成OOBE

4. **BitLocker**：Windows磁盘加密功能，很多新笔记本默认开启
![bitlocker.png](/system/reinstall_image/bitlocker.png =350x)
- 主系统无法登陆，影响数据备份
- 数据盘（如D:）被加密，重装后无法自动解密
- 密钥：[登陆微软账户查看](https://account.microsoft.com/devices/recoverykey)
- 命令行关闭（管理员）：`manage-bde –off C:`

### 重装前建议尝试的修复操作
- Dism++修复（等同于dism命令）
![dism_restorehealth.png](/system/reinstall_image/dism_restorehealth.png =350x)
- sfc系统文件检查
![sfc.png](/system/reinstall_image/sfc.png =600x)

## 致谢
本教程基于多位同学的重装技术培训资料改编而成，特此致谢：
张宇东（2019），毕聪博（2020），余冬杰（2021），赵祺铭（2022）