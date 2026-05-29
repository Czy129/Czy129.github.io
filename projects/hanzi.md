---
layout: post
title: 手写汉字识别系统
subtitle: 基于卷积神经网络（CNN）的中文手写汉字识别模型
permalink: /projects/hanzi/
---

## 项目概述

本项目是**智能科学与技术专业**大二《机器学习》课程的课程设计项目。旨在利用卷积神经网络（CNN）构建一个能够准确识别**手写中文汉字**的深度学习模型。系统支持国家标准 GB2312 一级字库中的 3755 个常见汉字，通过用户手写输入或上传图片即可完成实时识别。

| 指标 | 数值 |
|------|------|
| 可识别汉字数 | **3755** |
| 识别准确率 | **96.8%** |
| 训练样本量 | **120万+** |
| 模型大小 | **8.7 MB**（ONNX） |
| 单图推理 | **~12 ms** |

## 技术方案

模型采用经典的卷积神经网络结构，包含多个卷积层、池化层和全连接层。训练数据来自 `CASIA-HWDB` 离线手写汉字数据集。

```
网络结构
Input (64×64 灰度图)
  └─ Conv2d(channels: 1→64, kernel: 3×3)
  └─ BatchNorm2d + ReLU + MaxPool2d(2×2)
  └─ Conv2d(channels: 64→128, kernel: 3×3)
  └─ BatchNorm2d + ReLU + MaxPool2d(2×2)
  └─ Conv2d(channels: 128→256, kernel: 3×3)
  └─ BatchNorm2d + ReLU + MaxPool2d(2×2)
  └─ AdaptiveAvgPool2d → Flatten
  └─ Dropout(0.5) → Linear(256*4*4→1024) → ReLU
  └─ Dropout(0.3) → Linear(1024→3755) → Softmax
```

## 关键技术点

1. **数据预处理**：对 CASIA-HWDB 数据集进行灰度化、二值化、大小归一化至 64×64 像素；通过随机旋转、平移、缩放等方式进行数据增强。
2. **模型优化**：使用 Adam 优化器，初始学习率 0.001；引入 ReduceLROnPlateau 学习率衰减策略。
3. **部署方案**：模型导出为 ONNX 格式，使用 Flask 搭建 Web 服务端，前端 Canvas 绘画板接收手写输入，返回 top-5 识别结果。

## 标签

`PyTorch` `CNN` `Python` `OpenCV` `Matplotlib`

## 收获

通过本项目，我深入理解了卷积神经网络在图像分类任务中的工作原理，掌握了 PyTorch 框架的数据加载、模型定义、训练与评估全流程。体会到数据质量对模型性能的决定性影响——引入数据增强后准确率提升约 4%。
