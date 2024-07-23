---
title: Roofline
date: 2024-07-23 19:00:00
author: Cardioid
top: false
hide: false
cover: false
toc: false
mathjax: true
summary: Roofline
categories: 
tags:
  - 计算机与安全理论
---
Roofline模型用于分析AI模型在计算平台上的理论计算性能上限。
# 物理量
（1）计算量：针对单个输入，AI模型完成一次前向传播进行的浮点运算次数。公式为$H_{out}*W_{out}*K*K*C_{in}*C_{out}$（单位：FLOPs）。

（2）访存量：针对单个输入，AI模型完成一次前向传播进行的内存交换量。公式为$4*($模型参数占用内存$+$每层输出特征图占用内存$)$（单位：Byte）。

（3）计算强度：每字节内存的交换到底有多少用于浮点运算。公式为$I=\frac{计算量}{访存量}$（单位：FLOPs/Byte）。

（4）算力：计算平台每秒能完成的浮点运算次数上限$P_{peak}$（单位：FLOPS）。由于GPU各自独立支持单精度和双精度浮点运算，以NVIDIA显卡中FP64$:$FP32$=64:1$为例，GPU支持双精度浮点运算的算力理论峰值公式为$GPU主频*核心数量*单个时钟周期内能处理的浮点运算次数$，而单精度的就要在这个数据的基础上才除以$64$。

（5）带宽：计算平台每秒能完成的内存交换量上限$b_s$（单位：Byte/s）。公式为$内存数据速率*内存接口位宽/8=内存速度*内存接口位宽$。

（6）计算强度上限：$I_{max}=\frac{P_{peak}}{b_s}$。

（7）模型计算性能瓶颈：$P=min[P_{peak},I*b_s]$。
# Roofline模型

