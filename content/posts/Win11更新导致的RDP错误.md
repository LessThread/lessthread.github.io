---
title: Win11 KB5055528 更新导致RDP错误
date: 2025-04-14 20:11:02
tags:
categories:
description: Win11 KB5055528 更新导致RDP错误解决方法
mathjax: true 
showTableOfContents: true
---

## 系统环境
windows 11 23H2 专业版 

## 问题表现
更新 KB5055528 后,其他 windows 电脑在局域网中,无法通过远程桌面连接此台win11设备.具体表现为RDP可以正常通过账密验证,最后出现

> 你的远程桌面服务会话已结束 可能是下列原因之一...

## 解决方案
卸载 KB5055528 更新  
使用管理员权限在终端中执行
```
wusa /uninstall /kb:5055528
```
## 附录
- https://techcommunity.microsoft.com/discussions/windows11/kb5055528-breaks-rdp/4404110

- https://www.anoopcnair.com/windows-11-kb5055523-kb5055528-april-2025-patch/