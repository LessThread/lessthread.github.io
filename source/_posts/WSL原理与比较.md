---
title: WSL原理与比较
date: 2023-09-29 14:01:22
tags: 虚拟化 
categories: 笔记
---

# WSL是如何在windows上运行的
以下内容是给基于我日常使用得出的经验，在没有注明来源的地方可能有不准确的描述，WSL的具体使用指南和操作流程微软官方和网络上已有较多资料，善用搜索即可顺利构建WSL开发环境。

## wsl是什么
> 适用于 Linux 的 Windows 子系统可让开发人员按原样运行 GNU/Linux 环境 - 包括大多数命令行工具、实用工具和应用程序 - 且不会产生传统虚拟机或双启动设置开销。  

以上内容摘自Microsoft官网，是对WSL的基本介绍，总的来说，WSL是打通Windows和Linux的一套兼容层实现，比传统的虚拟机性能更好，使用更方便。

注意的是，尽管WSL1和WSL2对外提供了类似的兼容层表现，但是他们底层的实现有很大的区别，在某些特性上有着较大的不同，尤其是在网络和文件系统和内核表现上，我们之后会介绍到这一点，您可以根据自己的需求选择合适的WSL版本。


## wsl1是什么
WSL1是基于Windows NT内核的子系统实现。简单来说，就是把终端中的Linux指令`转换`为Windows指令进行实现，WSL在这里提供`中间层`的作用。这种翻译实现方式被也称为PICO进程。

WSL1的文件系统对Windows来说就是一个磁盘分区，被视为Windows系统内的一部分；在网络方面，WSL使用Windows的网络模块，可以通过网络直接访问WSL内的网络进程。
## wsl2是什么

> WSL 2 是适用于 Linux 的 Windows 子系统体系结构的一个新版本，它支持适用于 Linux 的 Windows 子系统在 Windows 上运行 ELF64 Linux 二进制文件。 它的主要目标是提高文件系统性能，以及添加完全的系统调用兼容性。

以上内容摘自Microsoft官网，WSL2实际上使用的是独立的Linux内核，由Windows的Hyper-V管理。简单来说，就是一个嵌入式的轻量`虚拟机`，它比软件式（例如VMware）实现的Linux更亲和Windows原生环境，跨系统的文件访问相较于普通虚拟机也更加方便。


## wsl1和wsl2在Windows资源所需上的区别
总的来说，WSL1是一个普通的Windows`程序`，他将Linux命令转换为Windows下来实现，这使得WSL1不需要依赖虚拟化功能和Hyper-V，只需在Windows功能中打开`Linux子系统`即可。

在功能上，WSL1对跨OS文件系统的功能更好，对Windows的资源需求更少，更轻量和更快速，网络访问更直接；但是不一定能支持所有的应用，也无法托管类似Docker的应用程序。

WSL2则是一个具有完整Linux内核的`虚拟机`，它通过虚拟化实现完整的Linux支持，相较于WSL1，它所需资源更多，跨OS文件系统的性能更弱，网络协调也更复杂（WSL2貌似是单独的子网实现，有完整的Linux网络协议栈，详情配置和补丁请使用搜索引擎了解）；但是WSL2能提供完全的Linux应用支持，包括Docker（docker可以使用Docker for Windows Desktop,来减少对网络的手动配置）。

更详细的使用场景区别请参考[微软官网](https://learn.microsoft.com/zh-cn/windows/wsl/compare-versions?source=recommendations)

一言以蔽之，WSL1少功能，更轻更快；WSL2完整，更重更慢，当然相较于其他虚拟机，WSL2还是更轻量级的。