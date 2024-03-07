---
title: Blendshape Retargeting 入门笔记
date: 2021-12-22 20:25:00
author: Vincent
toc: true
mathjax: true
summary: 表情重定向的一些笔记
categories: 技术
tags:
  - 未完工
  - 表情动画
---

## 前言
- 这里的blendshape指的是人脸模型表情的表示方式，所以这是一篇关于模型表情的笔记
- 本篇笔记主要关注算法上的实现，而不是CG或3D软件，如maya，ue等软件上的实现。

## 定义
Blendshape Retargeting,  被称为**表情重定向**问题，指的是将一个人脸模型的表情迁移到另一个模型上。包括真实人脸的表情迁移到3D人脸模型上，或者把A人脸模型的表情迁移到B人脸模型上。比如电影中，用真人来驱动阿凡达的脸部表情，就是一个表情重定向的过程。PS：在这里，默认为人脸模型的表情由Blendshape驱动。

**TODO**：放图

## 一些基础知识

### 1. 表情和Blendshape的关系 ###
- 现在业界在做表情动画时，都是基于blendshape或者骨骼来实现，大部分还是偏向用blendshape来做表情，因为操作起来会更方便一些。
- 每个模型会做一系列的基表情，比如闭眼、张嘴等，这样，从一个自然的表情到闭眼的基表情之间，做一个blendshape，就形成了眨眼的表情动画效果。

**TODO**：补基表情的图

- blendshape和表情动画的数学表示
    - 将表情模型看做 $N$ 个表情基模型：$B=[b_0,...,b_N]$, $b_0$ 可以看作无任何表情的中性表情。每一个基表情都会对应一个表情系数 $E=[e_0,...,e_N]$，组合起来就形成了各种各样的表情：
    $$
    F = b_0+\sum^{N}_{i}e_i(b_i-b_0), e_i\in[0,1]
    $$

### 2. 常用的blendshape标准 ###
- 通用的是51维blendshape
    - 包括eyeblink, eyeopen, jawopen, liptogether等维度
- 苹果标准
    - https://developer.apple.com/documentation/arkit/arfaceanchor/2928251-blendshapes

### 3. 拓扑结构是什么
- 拓扑结构一般指顶点数、三角面片数和三角面片连接关系

### 4. 稀疏表示和稀疏编码
- 有时会遇到稀疏表示的问题，可以参考下列文章
- https://www.pianshen.com/article/74041446901/

## 一些有用的文章或博客
### 论文：
- [Practice and Theory of Blendshape Facial Models](http://www.scribblethink.org/Work/Pdfs/blendshapes_MAIN.pdf)（入门级文章，主要讲解blendshape的一些基本知识）
- [基于变形迁移的表情克隆及其应用](http://www.cs.unc.edu/~jdh/documents/thesis_master.pdf)（中科院硕士论文，可以当基础知识来看）

### 博客：
- [卡通角色表情驱动系列一
](https://blog.csdn.net/zb1165048017/article/details/115491531)  （讲解表情驱动的博客）
- [卡通角色表情驱动系列二
](https://blog.csdn.net/zb1165048017/article/details/118864029)  （讲解网格变形的博客）

