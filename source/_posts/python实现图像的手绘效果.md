---
title: python实现图像的手绘效果
date: 2019-07-08 19:55:45
tags: python
---

python实现图像的手绘效果

``` python

import numpy as np
from PIL import Image

'''
利用像素间的梯度值和虚拟深度值对图像进行重构
根据灰度变化来模拟人类视觉的明暗程度
'''

imagearr = np.asarray(Image.open('path/to/image').convert('L')).astype('float')

depth = 10.0  # 0-100
grad_x, grad_y = np.gradient(imagearr)
grad_x = grad_x*depth/100
grad_y = grad_y*depth/100

vec_el = np.pi/2.2  # elevation rad
vec_az = np.pi/4.  # azimuth rad
dx = np.cos(vec_el) * np.cos(vec_az)
dy = np.cos(vec_el) * np.sin(vec_az)
dz =np.sin(vec_el)

A = np.sqrt(grad_x**2 + grad_y**2 + 1)
uni_x = grad_x/A
uni_y = grad_y/A
uni_z = 1./A
imagearr2 = 255*(dx*uni_x + dy*uni_y + dz*uni_z)
imagearr2 = imagearr2.clip(0, 255)
img =Image.fromarray(imagearr2.astype('uint8'))
img.save('image.jpg')

```

![原图](/images/python-image-source.jpg)
![手绘效果图](/images/python-image-gradient.jpg)
