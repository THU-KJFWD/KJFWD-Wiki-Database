---
title: Windows系统重装
description: 快进到重开
published: true
date: 2022-11-30T06:15:17.503Z
tags: 
editor: markdown
dateCreated: 2022-11-30T05:50:38.526Z
---

# Windows系统重装

## 前言

## 重装步骤

> **TL;DR**

{.is-info}

## 补充知识

### 启动U盘制作

### 第三方安装器

### 常见bug集锦

- 无法关闭Secure Boot
	- BIOS无选项
	- Linpus lite boot failed

- 安装程序出错
	- 读不出硬盘
  - 只能安装到GPT磁盘
  - 磁盘可能很快会出现故障

### Windows引导补充知识

- EFI与Legacy引导
	- MBR与GPT分区表
- 四个常见分区
  - EFI分区（启动分区，100MB，无需手动创建/删除）
  - MSR分区（系统保留，16MB~128MB，无需手动创建/删除）
  - 系统盘（可占满剩余空间，推荐100G以上）
  - 恢复分区（500~600MB，无需手动创建/删除）包含Windows恢复环境
