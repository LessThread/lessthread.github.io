+++
date = '2025-01-27T21:30:34+08:00'
draft = false
title = 'CUDA与深度学习环境简介'
description = '对Noise2Same模型的优化与对比'   
showTableOfContents = true
+++
# CUDA 简介
引用自wiki:
> CUDA（Compute Unified Device Architecture，统一计算架构）是由英伟达NVIDIA所推出的一种软硬件集成技术，是该公司对于GPGPU的正式名称。透过这个技术，用户可利用NVIDIA的GPU进行图像处理之外的运算.

简单来说,就是让显卡能够运行除了图形相关运算外,也可以作为计算卡加速其他的可编程计算,包括 AI 相关的推理. 

由于 GPU 计算单元远超 CPU ,所以大多数 AI 模型需要使用 GPU 支持以达到可接受的 训练/推理 时间.现行的主流深度学习框架 (torch,tensorflow) 对CUDA的适配最为良好和易用,因此推荐初学者和 AI 一般用户使用 Nvidia 显卡.

# 从 CUDA 到 python
```
底 层
  |     GPU 硬件        实际负责运算的`基本硬件`
  | 
  |     驱动程序        操作系统层,直接指挥硬件的工作,图形渲染和科学计算都需要的`基本软件`
  | 
  |     CUDA 工具包     提供基本可编程性,高级用户可以通过 CUDA 编程实现GPU计算
  | 
  |     Conda 环境      隔离python环境,使得 同一操作系统/环境 下切换不同python的需求更方便
  | 
  |     python          深度学习框架的依赖语言
  | 
  ↓     深度学习框架     提供高可编程的 AI 框架
顶 层
```

接下来让我们举例说明其中的兼容性  


## GPU 硬件   
依据显卡架构提供支持CUDA版本的上限 (下面表格摘自[wiki百科](https://zh.wikipedia.org/wiki/CUDA),更详细的情况请查阅英文wiki,中文更新略有延迟)  

| CUDA 版本 | 支持的计算能力 | 微架构 | 备注 |
| --- | --- | --- | --- |
| 1.0[10] | 1.0 – 1.1 | Tesla | - |
| 1.1 | 1.0 – 1.1+x | Tesla | - |
| 2.0 | 1.0 – 1.1+x | Tesla | - |
| 2.1 – 2.3.1[11][12][13][14] | 1.0 – 1.3 | Tesla | - |
| 3.0 – 3.1[15][16] | 1.0 – 2.0 | Tesla, Fermi | - |
| 3.2[17] | 1.0 – 2.1 | Tesla, Fermi | - |
| 4.0 – 4.2 | 1.0 – 2.1+x | Tesla, Fermi | - |
| 5.0 – 5.5 | 1.0 – 3.5 | Tesla, Fermi, Kepler | - |
| 6.0 | 1.0 – 3.5 | Tesla, Fermi, Kepler | - |
| 6.5 | 1.1 – 5.x | Tesla, Fermi, Kepler, Maxwell | 最后支持计算能力 1.x (Tesla) 的版本 |
| 7.0 – 7.5 | 2.0 – 5.x | Fermi, Kepler, Maxwell | - |
| 8.0 | 2.0 – 6.x | Fermi, Kepler, Maxwell, Pascal | 最后支持计算能力 2.x (Fermi) 的版本；GTX 1070Ti 不受支持 |
| 9.0 – 9.2 | 3.0 – 7.2 | Kepler, Maxwell, Pascal, Volta | Pascal GTX 1070Ti 不受 CUDA SDK 9.0 支持，但受 CUDA SDK 9.2支持 |
| 10.0 – 10.2 | 3.0 – 7.5 | Kepler, Maxwell, Pascal, Volta, Turing | 最后支持计算能力 3.x (Kepler) 的版本；CUDA SDK 10.2 是最后能用于 macOS 的官方版本，在未来的版本中 macOS 将不被支持 |
| 11.0 – | 3.5 - 8.6 | Maxwell, Pascal, Volta, Turing, Ampere | - |

在选择硬件时,请结合您项目的实际需求,**请特别注意, CUDA 虽然有较好的兼容性设计,但是仍然存在较新显卡不能支持低版本 CUDA 的情况(详细情况请查询 wiki 的 CUDA 各版本算力支持范围)**

## GPU 驱动
依据显卡nvida官网上选择,如果您没有特殊需求,建议使用最新版本驱动
安装完成后,使用`nvidia-smi` 即可查看 GPU 状况.
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 526.98       Driver Version: 526.98       CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name            TCC/WDDM | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA P106-100    TCC   | 00000000:10:00.0 Off |                  N/A |
| 33%   50C    P0    29W / 120W |      8MiB /  6144MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

- `NVIDIA-SMI` 为 Nvidia-smi 版本  
- `Driver Version` 为驱动版本
- `CUDA Version` 为该驱动支持的 CUDA 最高版本 

## CUDA 版本
请依据项目提供的条件和自身驱动选择合适的 CUDA 版本,如果项目未指明,请根据 python/torch/tf 框架版本结合项目时间推测,通常可使用 `CUDA 11.x `版本  

您可以通过下面的方法安装cuda

1. 直接安装,通过 nvidia 官网安装相应版本的 CUDA 工具包  

2. 通过conda安装 ( 如果您正在服务器上,请考虑使用非 root 安装 miniconda 方法 ), 这里以 pytorch 为例
    ```shell
    conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
    ```
    您可以参考所需的学习框架版本,安装合适的 CUDA. 也可以在相关学习框架的官网上查找适配的 CUDA 版本

## conda安装
推荐使用 [miniconda](https://docs.anaconda.com/free/miniconda/index.html)

## python 和相关包安装
略
