---
title: CSS 实现三角形
date: 2019-02-06 17:30:29
tags:
  - css
  - 图形
categories:
  - css
---

## HTML

```html
<div class="box box1"></div>
<div class="box box2"></div>
<div class="box box3"></div>
<div class="box box4"></div>
```

## CSS

```css
.box {
  width: 0px;
  height: 0px;
  overflow: hidden;
  border-width: 10px;
  border-style: solid;
}
.box1 {
  border-color: transparent transparent red transparent;
}
.box2 {
  border-color: red transparent transparent transparent;
}
.box3 {
  border-color: transparent red transparent transparent;
}
.box4 {
  border-color: transparent transparent transparent red;
}
```

## 效果

![](https://ws2.sinaimg.cn/large/006tNc79gy1fzwu5erw0vj30740f40sj.jpg)
