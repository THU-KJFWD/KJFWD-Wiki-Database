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

![noteverymonkeyisphotographer.jpg](/manual/img/monkeywithdlsr.jpg)

#### BUT everyone with POST CARD can diagnose PC with it!

诊断卡真的是最容易使用、且效率最高的工具了捏！

## 第一步，你需要一部安卓手机并下载[APP](http://nas.kjfwd.com:31200/sharing/nrdO5IuJ2)  

安装APP就不用教了吧

## 第二步，给诊断卡蓝牙模块供电，并且蓝连牙

请不要将诊断卡的供电接到待测试设备上，谁知道他的5V供电会不会突然没了捏
插上电之后，应该可以看到蓝色指示灯了，参考APP引导蓝连牙  

## 第三步，好多插口，诊断卡应该插在哪里捏？

> 新的主板通常不会把诊断代码发送给PCI-E总线上的设备，因此没有PCI插槽的主板可能只有LPC和USB接口可以用来插诊断卡了 
> <p align="right">--翻译自英文 <a href="https://en.wikipedia.org/wiki/POST_card/">Wikipedia</a> </p>

![postcard.jpg](/manual/img/postcard.jpg)

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



