---
title: css为图片添加一层蒙版
date: 2020-08-19 10:29:40
tags:
- css
- html
categories:
- css
---

昨天做门户网站时遇到一个需求，需要给图片添加蒙版，所以记录一下用css如何来实现<!--more-->



![](/images/css为图片添加一层蒙版.png)

如图所示，要为图片添加一层蒙版

1. 首先在```img```标签外面添加一层```div```

```html
<div class="dimback">
    <img src="#" class="dim"/>
</div>
```

2. 将```div```的背景颜色设置为黑色，并为```img```元素设置不透明度

```css
.dim{
  opacity:0.6;
  filter: alpha(opacity=60);
}
 
.dimback{
  background: #000;
}
```

这样就可以实现想要的效果了