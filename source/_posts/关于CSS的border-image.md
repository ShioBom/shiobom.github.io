---
title: 关于CSS的border-image
date: 2019-10-20 16:34:48
tags:
---

## border-image

border-image 通过指定一张图片来取替 border-style 定义的样式，但 border-image 生效的前提是： border-style 和 border-width 同时为有效值(即 border-style 不为 none，border-width 不为 0)。

- `border-image-source`

```css
border-image-source: none | <image>
```
- `border-image-slice`

边框图像切片。指定4个值(4条分割线：top, right, bottom, left)将 border-image-source 分割成9宫格，如下：
![enter image description here](https://misc.aotu.io/leeenx/border-image/2ia2uu.gif)

四条分割线的值
border-image-slice 四条线的值类型为：number | percentage。

number 不带单位的数值。1 代表 1个图片像素。
percentage 百分比。
```css
border-image-slice: 27 27 27 27;
```

**推荐阅读：** [border-image 的正确用法](https://aotu.io/notes/2016/11/02/border-image/index.html)
