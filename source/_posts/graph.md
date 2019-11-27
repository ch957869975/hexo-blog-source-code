---
title: 如何使用css绘制心形
date: 2019-02-06 08:09:36
tags:
  - css
  - 图形
---

# 如何使用 css 绘制心形

常遇到心形图案，比如点赞和取消点赞的使用场景。之前的使用方式是图片接入，作为`img` 或 `backgroundImage` 插入到 dom 中去。现在自己动手用 css 绘制一个心形图案。

<!--more-->

## 心形

准备一个`dom`元素如下,为其`id`赋值为`heart`

```html
<div id="heart"></div>
```

添加宽高

```css
#heart {
  position: relative;
  width: 50px;
  height: 40px;
}
```

现在它应该是一个宽`50px`,高`40px`的矩形，没跑了。现在开始操作伪元素

```css
/*上一步骤的代码省略...*/

#heart:before,
#heart:after {
  position: absolute;
  left: 0;
  top: 0;
  content: "";
  width: 25px;
  height: 40px;
  background: red;
  border-radius: 20px 20px 0 0;
}
#heart:after {
  content: "";
  left: 25px;
  top: 0;
}
```

emmm... 形状无法描述，上图吧还是...到现在为止的形状应该是这个样子的。
![](https://user-gold-cdn.xitu.io/2019/2/6/168c0906096c8752?w=148&h=116&f=jpeg&s=2672)

接下来要做的是将`before`和`after`两块内容旋转。代码如下：

```css
#heart:before,
#heart:after {
  position: absolute;
  left: 25px;
  top: 0;
  content: "";
  width: 25px;
  height: 40px;
  background: red;
  border-radius: 40px 40px 0 0;
  transform: rotate(-45deg);
  transform-origin: 0 100%;
}
#heart:after {
  content: "";
  left: 0;
  top: 0;
  transform: rotate(45deg);
  transform-origin: 100% 100%;
}
```

上图上图...

![](https://user-gold-cdn.xitu.io/2019/2/6/168c09dcf6787c68?w=228&h=194&f=jpeg&s=3275)

效果已出，感谢阅读。

[源码在此](https://codepen.io/ch957869975/pen/RvLgEg)
