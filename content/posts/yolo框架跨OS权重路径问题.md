---
title: yolo5框架跨OS权重路径问题
date: 2025-04-30 20:11:02
tags:
categories: 踩坑合集
description: yolo5框架跨OS权重路径问题
mathjax: true 
showTableOfContents: true
---

## 系统环境
Windows 10 | Debian 12

## 问题表现
在 Windows 上用 yolo5 训练出了best.pt模型权重,转移到 Linux 上部署测试的时候出现
```
> python detect.py --weights path/to/best.pt --source test_img/0338.jpg --conf 0.25 --save-txt  --save-crop

...
File "/root/miniconda3/envs/yolo-aquatrash/lib/python3.8/pathlib.py", line 1044, in new
raise NotImplementedError("cannot instantiate %r on your system"
NotImplementedError: cannot instantiate 'WindowsPath' on your system
...
```

## 原因
YOLOv5 默认在训练完成后会将模型权重（.pt 文件）与训练配置（opt 字典）一起保存​,opt 字典通常包含数据集路径、权重路径等信息.  

如果使用 Windows 训练，这些路径可能以 `WindowsPath` 对象形式存储。当在 Linux 系统上加载报错.

## 解决方案
1. 换个平台部署
2. 去掉opt字段重新训练
3. 从 checkpoint 字典中删除 opt 字段,代码参考如下
```python
import os
import sys
import torch

model_path = 'best.pt'
ckpt = torch.load(str(weights_path), map_location='cpu')

if 'opt' in ckpt:
    del ckpt['opt']

new_model_path = './aquatrash_detection6-change/weights/best_fixed.pt'
torch.save(checkpoint, new_model_path)
```

## 附录
- https://blog.csdn.net/weixin_44984705/article/details/141062113
- https://github.com/orgs/ultralytics/discussions/8575
- https://github.com/ultralytics/yolov5/issues/12911