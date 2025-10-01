---
date : '2025-10-01T11:30:34+08:00'
title : 'windows tiny安装'
description : 'windows tiny安装'  
categories : '人工智能'
tags: ["CUDA"]
---
# tiny 简介
[项目地址](https://github.com/NVlabs/tiny-cuda-nn)

>This is a small, self-contained framework for training and querying neural networks. Most notably, it contains a lightning fast "fully fused" multi-layer perceptron (technical paper), a versatile multiresolution hash encoding (technical paper), as well as support for various other input encodings, losses, and optimizers.

总而言之就是NVIDIA的加速库，不少论文或者项目的实现会使用到。

# 安装注意事项
最简单的安装方式是：
```shell
pip install git+https://github.com/NVlabs/tiny-cuda-nn/#subdirectory=bindings/torch
```
但是由于依赖问题，成功率并不是特别高，可能会出现`找不到 crtdefs.h 等头文件`等问题tiny对msvc和cuda版本都有较为精确的要求，可以参考下面的配置结合实际情况调整

我的配置：
- NVIDIA RTX3060
- CUDA 12.1


在 Windows 平台上 tiny-cuda-nn 依赖 **MSVC 编译器**，而 MSVC 依赖 **Visual Studio Build Tools** 或完整版 Visual Studio。

## Visual Studio C++ 编译环境

  
  通过Visual Studio Installer 进行安装时需要勾选`工作负载` `“使用 C++ 的桌面开发” (Desktop development with C++)`
  以及下面这些单个组件

  - MSVC v143（或 v142）x64/x86 Build Tools
  - Windows 10 SDK（10.0.19041 或更高
  - 最新的 C++ CMake Tools for Windows（应该是可选）
  - MSVC C++ 核心功能
  
  注意使用 VS 2019 或 VS 2022 更老版本（v141 以下）可能导致问题。

* 安装完后，确保 `cl.exe` 可用：

  ```powershell
  cl
  ```

  如果提示版本号就正常，否则要运行：

  ```powershell
  "C:\Program Files\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
  ```

---

## CUDA 工具链

* 安装 **完整的 CUDA Toolkit**（与 PyTorch 编译版本一致，例如 cu121 对应 CUDA 12.1 Toolkit）。
* 安装路径建议不要有空格，比如：

  ```
  C:\CUDA\Toolkit
  ```
* 编译时确保 `nvcc --version` 可用：

  ```powershell
  nvcc --version
  ```
* 通常编译参数里会自动包含显卡架构，我的RTX3060对应 `sm_86`，如果不行，可手动指定：

  ```powershell
  set TCNN_CUDA_ARCHITECTURES=86
  ```

  然后继续你安装你的torch和conda环境
  直到编译安装tiny
  ```shell
  pip install git+https://github.com/NVlabs/tiny-cuda-nn/#subdirectory=bindings/torch
  ```

---

## 组合总结

| 组件            | 推荐版本                  | 说明                                   |
| ------------- | --------------------- | ------------------------------------ |
| Visual Studio | 2022（或 2019）          | 安装“Desktop development with C++”工作负载 |
| MSVC          | v143                  | CUDA 11.5+ 和 12.x 都支持                |
| Windows SDK   | 10.0.19041 或更高        | 必须，否则找不到 crtdefs.h 等头文件              |
| CUDA Toolkit  | 12.1（或 12.4）          | 与 PyTorch 版本一致                       |
| PyTorch       | 2.4.0+cu121（或 +cu124） | 官方提供预编译包，避免版本冲突                      |
| GPU 架构        | sm_86                 | RTX 3060 对应                          |

---

# 附：
如果只用 PyTorch，不编译额外组件是不需要单独安装完整 **CUDA Toolkit**，直接用 PyTorch 官方的 “自带 CUDA” 版本。

* 安装示例（CUDA 12.1 版），直接去官网复制命令也行
  ```powershell
  pip install torch==2.4.0+cu121 torchvision==0.19.0+cu121 --index-url https://download.pytorch.org/whl/cu121

但是我们这次需要额外编译 tiny ，是所以需要完整的工具包（你可以理解为torch自带的是runtime）
