---
title: Windows系统重装
description: 快进到重开
published: true
date: 2022-12-03T04:15:04.178Z
tags: 
editor: markdown
dateCreated: 2022-11-30T05:50:38.526Z
---

# Windows系统重装

## 前言
重装Windows

## 重装步骤

### 准备工作
> 数据无价，注意备份！
{.is-warning}


### U盘启动

### 安装程序

### 开箱体验

### 进桌面后

- 网络校时
- 系统激活

## 补充知识

### 启动U盘制作

### 第三方安装器

### Windows引导补充知识

- EFI与Legacy引导
	- MBR与GPT分区表
- 四个常见分区
  - EFI分区（启动分区，100MB，无需手动创建/删除）
  - MSR分区（系统保留，16MB~128MB，无需手动创建/删除）
  - 系统盘（可占满剩余空间，推荐100G以上）
  - 恢复分区（500~600MB，无需手动创建/删除）包含Windows恢复环境

### 常见bug集锦

- 无法关闭Secure Boot
	- BIOS无选项
	- Linpus lite boot failed

- 安装程序出错
	- 读不出硬盘
  - 只能安装到GPT磁盘
  - 磁盘可能很快会出现故障
  
- BitLocker


## 致谢
本教程基于多位同学的重装技术培训资料改编而成，包括
- 赵祺铭, 2022;
- 余冬杰, 2021;
- 毕聪博, 2020;
- 张宇东, 2019