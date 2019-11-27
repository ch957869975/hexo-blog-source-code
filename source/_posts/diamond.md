---
title: 如何使用css绘制钻石
date: 2019-02-08 11:45:49
tags:
  - css
  - 图形
---

听说你想要钻石 💎？买不起，还是用 css 来画一个吧，但你敢送给自己女朋友，不保证不被打。

下午两点要相亲，要不把这个送相亲对象？

# 效果

先看下效果吧，想一想怎么构图先。
![](https://user-gold-cdn.xitu.io/2019/2/8/168caeba3f9f72ad?w=494&h=424&f=jpeg&s=18726)

上图是已经完成的效果。钻石整体都是由三角形构成，上五下三。上边是五个等边三角形，其中有 2 个是倒扣过来填补三个之间的空缺。下边是一个等腰三角形和 2 个对称的钝角三角形，差不多就是这样。（钝角三角形不是太好理解，至少我没成功，这里的钝角三角形是用等腰三角形通过`transform: skew()`实现的）

# 知识点

这个 demo 中涉及到了 css3 的 `transform`, css 画三角形 以及 **如何给 css 画出的三角形加边框**，三角形的边框构成了钻石的棱角（白色的线条），预处理语言使用的是`less`。

<!--more-->

**三角形的边框**：我们知道，三角形本来就是用`border`画的，给三角形加边框相当于给`border`加`border`,这个做法肯定行不通。我是这样做的：画 2 个三角形，一个大的一个小的，小的比大的小`1px`,然后小的盖在大的上面，这样大三角形就只漏出`1px`,视觉效果就是成为了内部小三角形的边框线了。[参考博文](https://www.cnblogs.com/blosaa/p/3823695.html)

# 开始

## dom 准备

```html
<div id="diamond">
  <div class="t">
    <div class="common top"></div>
    <div class="common top"></div>
    <div class="common top"></div>
    <div class="common top"></div>
    <div class="common top"></div>
  </div>
  <div class="b">
    <div class="common bottom bottom1"></div>
    <div class="common bottom bottom2"></div>
    <div class="common bottom bottom3"></div>
  </div>
</div>
```

三角形的个数是上五下三。~~请忽略命名，please~~

## 样式

### 钻石上部分

先把`common`的样式定义出来

```less
#diamond {
  margin: 100px;
  .t {
    //直接定义了高度免去了清除浮动
    height: 30px;
  }
  .common {
    // 公共样式
    position: relative;
    float: left;
    width: 0;
    height: 0;
    border-style: solid; // 元素本身做大三角形，衬底成为小三角形的边框
    &:after {
      // 伪元素定义小三角形
      content: "";
      position: absolute;
      border-style: solid;
    }
  }
  div.top {
    // 钻石顶部的5个三角形特定样式
    border-width: 0 30px 30px; // 三角形大小
    border-color: transparent transparent #fff; // 三角形颜色
    &:after {
      // 小三角形
      top: 1px; // 移动三角形使之盖在底部的大的三角形
      left: -28px;
      border-width: 0 28px 28px; // 小三角形的大小定义
      border-color: transparent transparent red;
    }
    &:nth-child(2n) {
      // 第二个第四个三角形倒立。
      transform: rotate(180deg);
    }
    &:nth-child(n + 2) {
      // 从第二个开始都向左移动30px
      margin-left: -30px;
    }
  }
}
```

在样式中都做了注释，不再赘述 ， ~~我会说我赶时间去相亲？~~ 到这呢效果只有钻石上面的部分。如下图：

![](https://user-gold-cdn.xitu.io/2019/2/8/168cb15a3d59a634?w=494&h=148&f=jpeg&s=5877)

### 钻石下部分

```less
// 上部分的样式省略了
div.bottom {
  border-width: 90px 30px 0 30px; // 高度适当的高点，这里给了90px
  border-color: #fff transparent transparent; // 三角形向下，底色白色
  &:after {
    // 同上，做出内部红色的小三角形，尺寸稍小，漏出白色的“边框线”
    border-width: 88px 28px 0 28px;
    border-color: #f00 transparent transparent;
    top: -89px;
    left: -28px;
  }
  /*
     *   到这应该是三个等腰三角形
     *   第一个第三个三角形应该要是钝角三角形的。
     *   所以要进行一下倾斜操作
     */

  &.bottom1 {
    // 底部第一个三角形倾斜转换
    transform: skew(33.5deg);
    transform-origin: 100% 0;
  }
  &.bottom3 {
    // 底部第三个三角形倾斜转换，与第一个对称
    transform: skew(-33.5deg);
    transform-origin: 100% 0;
  }
}
// 数学不好，这个角度是我试了几次试出来的，数学好的可以算下呢，啊哈哈哈
```

注释里都写了。~~不赘述不赘述，别问为什么。~~

差不多就是这样了，上一下效果。

![](https://user-gold-cdn.xitu.io/2019/2/8/168caeba3f9f72ad?w=494&h=424&f=jpeg&s=18726)

~~我会说这就是一开始的效果图？~~ 总觉得差点什么，duangduang 加一下特效

![](https://user-gold-cdn.xitu.io/2019/2/8/168cb2d704d2f3b4?w=688&h=562&f=jpeg&s=30703)

buling buling 的效果，啊哈哈哈哈哈哈。

## ps

张鑫旭大神博客中有 不包含钻石棱角的实现，在第 26 个图形中。 [地址在此](https://www.zhangxinxu.com/wordpress/2019/01/pure-css-shapes/)

祝新年快乐，万事顺意。愿往后的生活没有相亲和 IE 浏览器。

效果已出，感谢阅读。
[源码在此](https://codepen.io/ch957869975/pen/VgrNyW) 或访问 [我的博客](https://ch957869975.github.io/hexo-blog/)

送个福利，[css 三角形产生器](http://apps.eky.hk/css-triangle-generator/zh-hant) 。
