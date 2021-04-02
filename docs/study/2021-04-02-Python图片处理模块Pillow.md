---
title: Python装饰器
parent: 好好学习天天向上
nav_order: 2
has_children: false
---

# Python图片处理模块Pillow

### 1. 介绍

PIL：Python Imaging Library，是Python平台事实上的图像处理标准库，但PIL年久失修，仅支持到Python 2.7。于是一群志愿者在PIL的基础上创建了兼容的版本，名字叫[Pillow](https://github.com/python-pillow/Pillow)，支持最新Python 3.x，作为PIL的替代版本。

在命令行下通过pip安装：

```
pip install pillow
```



### 2. 基本用法

以这张懒羊羊的图片为例：

![](https://cdn.jsdelivr.net/gh/lei-wei/pic_bed/img/lanyangyang.jpg)

#### 图片属性

```python
# 导入模块
from PIL import Image

ImageFilePath = 'C:/Users/lei/Desktop/lanyangyang.jpg'
# 以只读模式打开图片
image = Image.open(ImageFilePath, 'r')
# 获取图片属性
print(image)
# 获取图片大小
print(image.size)
# 获取图片源格式，JPEG，PNG等
print(image.format)
# 获取图片模式,常见有：L（灰度图像），RGB和CMYK（真彩图像）
print(image.mode)
# 转化为灰度图像
im2 = image.convert('L')
im2.show()

# 输出结果如下：
# <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=1920x1080 at 0x230C953EFA0>
# (1920, 1080)
# JPEG
# RGB
# L
```

转化为灰度后：

![lyy-L](https://cdn.jsdelivr.net/gh/lei-wei/pic_bed/img/lyy-L.jpg)



#### 格式转换

```python
# 导入模块
from PIL import Image
import os

ImageFilePath = 'C:/Users/lei/Desktop/lanyangyang.jpg'
# 分割路径名、拓展名
FilePath, FileExt = os.path.splitext(ImageFilePath)
# 格式转换
Image.open(ImageFilePath).save("{}.png".format(FilePath))
```



#### 制作缩略图、图标

```python
# 导入模块
from PIL import Image

image = Image.open("lanyangyang.jpg", 'r')
size = 128, 128
# 缩略图，改变图片大小
image.thumbnail(size)
print(image.size)  # (128, 72)
image.save("lyy.ico", "JPEG")

```

缩略图效果：

![](https://cdn.jsdelivr.net/gh/lei-wei/pic_bed/img/lyy.ico)

### 3 案例

#### 生成验证码

```python
from PIL import Image, ImageDraw, ImageFont, ImageFilter
import random


# 随机生成字符
def rndChar():
    txt_list = list()
    txt_list.extend([i for i in range(65, 91)])  # 大写字母
    txt_list.extend([i for i in range(97, 123)])  # 小写字母
    txt_list.extend([i for i in range(48, 58)])  # 数字
    select_index = random.choice(txt_list)  # 随机选择
    return chr(select_index)  # chr返回整数对应的 ASCII 字符


# 随机背景颜色
def rndBgColor():
    return tuple(random.randint(64, 255) for i in range(3))


# 随机字符颜色
def rndTxtColor():
    return (1, random.randint(32, 127), random.randint(32, 127), random.randint(32, 127))


def go():
    # 设置图片的宽高：
    width, height = 60 * 4, 60
    # 创建新图片，参数分别为：mode, size, color.
    image = Image.new('RGB', (width, height), color=(255, 255, 255))
    # 创建Font对象:
    font = ImageFont.truetype('C:/Windows/Fonts/Arial.ttf', 36)
    # 创建Draw对象:
    draw = ImageDraw.Draw(image)
    # 填充背景，每个像素随机颜色:
    for x in range(width):
        for y in range(height):
            draw.point((x, y), fill=rndBgColor())
    # 输出随机验证码
    for t in range(4):
        draw.text((60 * t + 10, 10), rndChar(), fill=rndTxtColor(), font=font)
    image.show()
    image.save("Verification-Code.jpg")
    # 模糊处理
    image = image.filter(ImageFilter.BLUR)
    image.show()
    image.save("Verification-Code-blur.jpg")


go()

```



生成的验证码图片：

![Verification-Code](https://cdn.jsdelivr.net/gh/lei-wei/pic_bed/img/Verification-Code.jpg)

模糊处理后的：

![Verification-Code-blur](https://cdn.jsdelivr.net/gh/lei-wei/pic_bed/img/Verification-Code-blur.jpg)