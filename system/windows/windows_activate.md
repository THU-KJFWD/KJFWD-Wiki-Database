---
title: 激活Windows
description: 可以不用学校的屑脚本了
published: true
date: 2022-11-10T18:37:41.205Z
tags: 
editor: markdown
dateCreated: 2022-11-10T18:04:30.521Z
---

# 写在前面

Windows操作系统是全球最流行的操作系统之一,它提供了丰富(且糟糕)的功能,为用户提供了无与伦比(差)的计算体验.然而,为了~~不用大黑桌面和右下角温馨提示~~您的数据安全和版权保护,激活Windows系统是非常重要的.

根据New Bing,使用正版 Windows 系统可以带来如下妙处:
1. 安全性更高：正版Windows可以随时获得安全升级,避免盗版软件带来的病毒和木马风险,保护个人隐私信息.
2. 支持售后：正版Windows可以获得微软及授权合作伙伴的专业售后服务支持,随时解决电脑问题.
3. 规避法律风险：正版Windows可以帮助企业和个人遵守相关法律法规,避免因使用盗版而被罚款或起诉.
4. 系统更稳定：正版Windows可以保证软件质量和性能,避免IT系统故障和数据丢失.
5. 享受完整功能：正版Windows具有丰富而完整的计算机体验功能,如个性化设置、云服务、应用商店等.

真是 和尚挖墙——妙死了,所以快来激活你的 Windows 操作系统吧!

# 预备芝士

Windows 的激活密钥大致上可以分为三类:

## 零售版密钥

有效零售版密钥包括家庭版和专业版,可通过购买彩盒（FPP）或者从微软商店购得.

这种密钥在授权上只能在一台设备上使用.每一个零售版密钥都有若干次联网激活和电话激活的机会.

零售版通道的密钥在更换主板后可以对数字许可证进行复制且不消耗产品密钥的激活次数.若系统从低版本的零售版产品升级而来,则只允许复制一次.若是Windows10的零售版密钥,理论上可以进行任意次的复制且不需要重新输入产品密钥.

多台电脑使用同一个零售版密钥的情况会消耗密钥的激活次数,且违反授权规则,但无论是第几个使用零售版密钥的机器,它们之间无差别.

购买FPP或者微软商店电子下载版的朋友要保护好自己的零售版密钥,尽可能减少零售版密钥的输入次数以免消耗激活机会.零售版密钥多余的激活机会是为了解决更换主板且转移不了的问题的,而不是为了允许在多台设备上使用.

零售版通道密钥中有一类特殊的密钥,叫做通用零售版密钥,这类密钥通常是安装消费者版镜像的系统的默认密钥.这种密钥无法直接激活系统,它只能通过前文所述的第二种产品密钥添加方式添加到机器中.在机器具有数字许可证时,具有通用零售版密钥的设备可以联网自动激活.

>目前已知通用零售版密钥如下,其中N表示不带Windows Media Player的版本  
>家庭版：YTMG3-N6DKC-DKB77-7M9GH-8HVX7    
>家庭版N：4CPRK-NM3K3-X6XXQ-RXX86-WXCHW  
>家庭单语言版：BT79Q-G7N6G-PGBYW-4YWX6-6F4BT  
>家庭限定国家版：N2434-X9D7W-8PF6X-8DV9T-8TYMD  
>教育版：YNMGQ-8RYV3-4PGQ3-C8XTP-7CFBY  
>教育版N：84NGF-MHBT6-FXBX8-QWJK7-DRR8H  
>专业版：VK7JG-NPHTM-C97JM-9MPGT-3V66T  
>专业版N：2B87N-8KFHP-DKV6R-Y2C8J-PKCKT  
>专业教育版：8PTT6-RNW4C-6V7J2-C2D3X-MHBPB  
>专业教育版N：GJTYN-HDMQY-FRR76-HVGC7-QPF8P  
>专业工作站版：DXG7C-N36C4-C4HTG-X4T3X-2YV77  
>专业工作站版N：WYPNQ-8C467-V2W6J-TX4WX-WT2RQ  
{.is-info}

## Multiple Activation Key (MAK)


这种密钥是通过微软的批量许可（VLSC）获得的.这种密钥的管理员可以登录VLSC中心进行密钥管理.

MAK密钥是一种可以在多台设备上使用的密钥.每使用一次激活次数就会减少一次,激活次数为0时,该密钥将不再允许进行联网激活,能否进行电话激活未知.

密钥管理员可以通过“充值”增加MAK密钥的激活次数.通过Windows ADK中VAMT工具可以查询MAK密钥的剩余激活次数.通过slmgr.vbs -dlv查询到的剩余重置计数不是MAK密钥的剩余激活次数.

使用MAK密钥激活的机器可以获得数字许可证（企业版LTSC和IoT企业版LTSC例外）.获得的MAK通道的数字许可证与Retail密钥所获得的数字许可证类似.MAK密钥获得的数字许可证可以通过疑难解答中的"我最近更改了硬件"选项进行数字许可证的复制,复制次数至少一次.

MAK密钥可能被MAK密钥的管理员或者微软注销,注销之后VAMT工具可以查询到The activation server reported that the product key has been blocked.目前尚不清楚MAK密钥注销后之前产生的数字许可证是否会被一并注销.MAK密钥理论上仅能供购买批量许可的单位内部使用.

## Generic Volume License Key

这类密钥通常是安装商业版镜像的系统的默认密钥,也是目前广泛使用的KMS工具所依赖的密钥,需要注意:使用非授权的 KMS 服务器构成法律意义上的盗版行为.这种密钥输入后需要通过Key Management Service (KMS) 服务器激活,KMS服务器每次可以将机器激活180天.企业版G和企业版GN的KMS激活时限为410年.这种方式的激活理论上也仅供单位内部使用.

>已知的GVLK密钥,其中N表示不带Windows Media Player的版本,G代表政府版,S代表LTSB或者LTSC版.    
>家庭版：33QT6-RCNYF-DXB4F-DGP7B-7MHX9,TX9XD-98N7V-6WMQ6-BX7FG-H8Q99  
>家庭版N：3KHY7-WNT83-DGQKR-F7HPR-844BM  
>家庭限定国家版：PVMJN-6DFY6-9CCP6-7BKTT-D3WVR  
>家庭单语言版：7HNRX-D7KGG-3K4RQ-4WPJ4-YTDFH  
>教育版：NW6C2-QMPVW-D7KKK-3GKT6-VCFB2  
>教育版N：2WH4N-8QGBV-H22JP-CT43Q-MDWWJ  
>企业版：96YNV-9X4RP-2YYKB-RMQH4-6Q72D,NPPR9-FWDCX-D2C8J-H872K-2YT43  
>企业版G：YYVX9-NTFWV-6MDM3-9PT4T-4M68B  
>企业版GN：44RPN-FTY23-9VTTB-MP9BX-T84FV  
>企业版N：DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4,WGGHN-J84D6-QYCPR-T7PJ7-X766F  
>企业版S：DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ,M7XTQ-FN8P6-TTKYV-9D4CC-J462D,PG7H6-7RNT3-R4MGR-HMJK2-J462D  
>企业版SN：92NFX-8DJQP-P6BBQ-THF9C-7CG2H,QFFDN-GRT3P-VKWWX-X7T3R-8B639,RW7WN-FMT44-KRGBK-G44WK-QV7YK,2D7NQ-3MDXF-9WTDT-X9CCP-CKD8V  
>专业版：W269N-WFGWX-YVC9B-4J6C9-T83GX  
>专业教育版：6TP4R-GNPTD-KYYHQ-7B7DP-J447Y  
>专业教育版N：YVWGF-BXNMC-HTQYQ-CPQ99-66QFC  
>专业版N：MH37W-N47XK-V7XM9-C7227-GCQG9  
>专业工作站版：NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J  
>专业工作站版N：9FNHH-K3HBT-3W4TD-6383H-6XYWF  
{.is-info}

## OEM 制造商密钥

#### OEM:DM

这种密钥是新款  OEM激活密钥,也是 Windows 10 时代后最主要的 OEM 密钥,它由 OEM 事先将硬件信息和 OEM 密钥上传微软服务器,在机器联网激活时,上传当前机器的硬件信息和主板上的OEM密钥进行比对,比对一致才可激活.这种密钥避免了更换主板上固化的产品密钥来激活的情况,理论上不可被破解.这种密钥产生的数字许可证不可转移.

#### OEM:System Locked Pre-installation

这种SLP密钥是早期的OEM激活密钥,Windows10可能已经没有这种密钥了.

一般情况下同一个OEM的同一种产品使用的是一个密钥,这种密钥被固化到了主板上,配合数字证书可以不联网进行产品激活.

#### OEM:Certificate Of Authenticity

这种密钥一般贴在OEM机器的外壳上,供重装系统使用.一个OEM:COA密钥只授权在一台机器上使用.尽量避免将OEM:COA密钥泄露.或许还有人买这玩意收藏的吧.

#### OEM:NONSLP通道：

这种密钥和零售密钥类似,可以理解为OEM授权的零售密钥,只不过通过这种密钥获取的数字许可证理论上不可复制.

此外还有一类特殊的OEM:NONSLP通道的密钥,称为通用OEM:NONSLP密钥,这种密钥无法激活设备,它是专门为获得了数字许可证的设备使用的,  

>目前已知的通用OEM:NONSLP密钥如下  
>专业版：9YN7X-HKC98-RH9B6-672Q4-VMH24  
>企业版：XGVPP-NMH47-7TTHJ-W3FW7-8HV2C  
>企业版S：NK96Y-D9CD8-W44CQ-R8YTK-DYJWX  
>企业版G：FV469-WGNG4-YQP66-2B2HY-KD8YX  
>企业版GN：FW7NV-4T673-HF4VX-9X4MM-B4H4T  
{.is-info}



## 数字许可证

数字许可证与你的硬件和你的 Microsoft 帐户关联,因此某些情况下重装系统后联网后即可激活.使用 MicroSoft 账户与关联的数字许可证可以将激活复制给多台电脑,但具体是否可以这样操作要参考密钥类型.

# 如何激活呢?

### 新装系统激活:适用于新组装的台式机等情况

激活前先确保联网正常!

Powershell(管理员模式):

>slmgr -skms kms.cic.tsinghua.edu.cn   
>slmgr -ipk 版本对应秘钥    
>slmgr -ato  
{.is-info}

>注意!使用诸如skms.netnr.eu.org的第三方SKMS是法律上的盗版行为!
{.is-warning}

此处常用密钥:
>专业版：W269N-WFGWX-YVC9B-4J6C9-T83GX  
>教育版：NW6C2-QMPVW-D7KKK-3GKT6-VCFB2  
{.is-info}


### 重装系统激活:OEM密钥且无意更改版本  

应先参考[本页面](https://support.microsoft.com/zh-cn/windows/%E5%9C%A8%E6%9B%B4%E6%8D%A2%E7%A1%AC%E4%BB%B6%E5%90%8E%E9%87%8D%E6%96%B0%E6%BF%80%E6%B4%BB-windows-2c0e962a-f04c-145b-6ead-fb3fc72b6665)的方法,先将数字许可证绑定到机主本人的微软账号上,再利用数字许可证激活重装后的系统.

### OEM 家庭版升级专业版

##### 已有Retail/MAK专业版密钥

直接输入激活即可

##### 想用学校的KMS服务

参考新装系统激活部分,有可能遇到非核心版本windows不能激活的情况,可以尝试如下命令

>DISM.exe /Online /Get-TargetEditions  
> 查看可供升级的windows 系统版本
> DISM /online /Set-Edition:Professional /ProductKey:W269N-WFGWX-YVC9B-4J6C9-T83GX /AcceptEula
> 理论上应该可行!
{.is-info}