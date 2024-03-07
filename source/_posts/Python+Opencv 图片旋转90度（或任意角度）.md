---
title: Python+Opencv 图片旋转90度（或任意角度）
date: 2022-01-12 19:00:00
author: Vincent
toc: true
summary: 图像处理之图像旋转实践
categories: 技术
tags:
  - 图像
  - python
---

## 功能
对图像进行旋转处理，最常用的比如逆时针旋转90度等。以前的处理代码可以对图像进行任意角度的旋转，以逆时针旋转90为例说明。
## 依赖库
1. opencv
2. numpy

## 效果展示
#### 旋转之前的原图
![paul.jpg](https://s3.bmp.ovh/imgs/2022/01/3fb2a62ee21fbaaf.jpg)
#### 逆时针旋转90度后的图
![img_rotated.jpg](https://s3.bmp.ovh/imgs/2022/01/aa58e341ee76e82d.jpg)
## 代码
```python
import cv2
import numpy as np

def img_rotate(src, angel):
    """逆时针旋转图像任意角度

    Args:
        src (np.array): [原始图像]
        angel (int): [逆时针旋转的角度]

    Returns:
        [array]: [旋转后的图像]
    """
    h,w = src.shape[:2]
    center = (w//2, h//2)
    M = cv2.getRotationMatrix2D(center, angel, 1.0)
    # 调整旋转后的图像长宽
    rotated_h = int((w * np.abs(M[0,1]) + (h * np.abs(M[0,0]))))
    rotated_w = int((h * np.abs(M[0,1]) + (w * np.abs(M[0,0]))))
    M[0,2] += (rotated_w - w) // 2
    M[1,2] += (rotated_h - h) // 2
    # 旋转图像
    rotated_img = cv2.warpAffine(src, M, (rotated_w,rotated_h))

    return rotated_img

if __name__ == "__main__":
    # 读取图像
    img_path = "./imgs/paul.jpg"
    img = cv2.imread(img_path)
    img_rotated = img_rotate(img, 90)
    cv2.imwrte("img_rotated.jpg", img_rotated)

```