---
title: 手把手实现图片懒加载
date: 2019-02-06 17:48:47
tags:
  - JavaScript
  - 懒加载
---

懒加载作为节约性能开支的优化项，已经必不可少。理所应当的成为前端必做工作之一。  
针对这个老生常谈的话题，自己还是想亲自动手自己撸一个。

## 懒加载的原理

其实也不是什么难的原理，页面内部打的`<img>`标签如果没有`src`属性，浏览器就不会去发出请求进而加载图片资源,此时显示的是默认的占位图（或元素）,没有请求，性能自然没话说。这个时候我们要做的就是让出现在可视区内的图片发出请求，加载图片，通过 js 动态设置`src`。说干就干

## 开始实现懒加载

一个问题，实际的 url 存在哪里？我们优先选择`data-*`自定义属性集，叫什么无所谓，你自己记住就好
随便写下 dom 结构，只是为了做简易 demo，实际开发中结构要和设计相关联。

<!--more-->

```html
<div id="box">
  <img
    class="lazy"
    data-src="http://img.pconline.com.cn/images/upload/upc/tx/wallpaper/1301/05/c0/17135331_1357355776882.jpg"
  />
  <img
    class="lazy"
    data-src="http://f.hiphotos.baidu.com/zhidao/pic/item/eac4b74543a982267a3d54978a82b9014b90eb86.jpg"
  />
  <img
    class="lazy"
    data-src="http://pic1.win4000.com/wallpaper/2/58b61f7dc6c1d.jpg"
  />
  <img
    class="lazy"
    data-src="http://file03.16sucai.com/2017/1100/16sucai_p20161106032_0c2.JPG"
  />
  <img
    class="lazy"
    data-src="http://imgsrc.baidu.com/image/c0%3Dpixel_huitu%2C0%2C0%2C294%2C40/sign=5a7938d38acb39dbd5cd6f16b96e6c48/aec379310a55b3196c79de4c48a98226cffc1702.jpg"
  />
  <img
    class="lazy"
    data-src="http://c.hiphotos.baidu.com/zhidao/pic/item/8d5494eef01f3a2987a8062f9f25bc315d607ceb.jpg"
  />
</div>
```

下面是对应这个结构的样式

```css
html,
body {
  height: 100%;
  width: 100%;
  margin: 0;
}

#box {
  color: red;
  width: 200px;
  height: 300px;
}

.lazy {
  /*占位背景图*/
  background: url("./img/loading.gif") no-repeat center;
}

img {
  margin-top: 100px;
  background-size: cover;
  background-position: center;
  width: 490px;
  height: 242px;
}
```

### 懒加载类

我们将`<img>`的`class`作为参数传进来，构建图片资源列表

```js
class Lazy() {
    constructor(selector) {
      // 懒记载图片列表，将伪数组转为数组，以便可以使用数组的api
      this.imageList = [...document.querySelectorAll(selector)]
      // 或使用下面方法,同样的效果
      // this.imageList = Array.prototype.slice.call(document.querySelectorAll(selector))
      this.init()
    }

    /*
     * 判断图片是否在可视区内
     */
    inViewShow() {
      const len = this.imageList.length
      if(!this.imageList.length) return
      this.imageList.map(item => {
        const rect = item.getBoundingClientRect()
        // 出现在可视区域内则加载图片
        if (rect.top > document.documentElement.clientHeight) return
        // 赋值src,加载实际资源
        item.src = item.dataset.src
        // 将当前的img移除加载列表（为什么没有使用map的第二个参数可以思考下）
        const index = this.imageList.findIndex(img => img.dataset.src === item.dataset.src)
        this.imageList.splice(index, 1)
        // 如果全部都加载完 则去掉滚动事件监听
        if(this.imageList.length) return
        document.removeEventListener('scroll', this.inViewShow)
      })
    }
    init() {
      this.inViewShow()
      document.addEventListener('scroll', this.inViewShow)
    }
}
```

至此，我们简单的实现了图片懒加载。but，少点什么。`scroll`作为一个高频事件，`inViewShow`就会随着滚动频率
无限的被触发，这样不好，要做些限制才行。
实际上，函数的防抖和节流一直是优化点而存在。可以使用`lodash`已经封装好的[节流函数](http://www.css88.com/doc/lodash/#_throttlefunc-wait0-options)。但因为现在要封装独立的`Lazy`类，依赖越少越好，所以还是我们自己写一个吧。

### 节流函数

一般来说，节流函数需要三个参数`fn`, `delay`, `must`, 分别是函数体，延时时间，必须运行时间。

```js
throttle(fn, delay = 15, must = 30) {
  let t_start = null // 开始时间
  let timer = null // 定时器
  const context = this
  return function() {
    let t_current = +(new Date())
    const args = [...arguments]
    clearTimeout(timer)
    if(!t_start) {
      t_start = t_current
    }
    // 如果超过must则执行一次，否则延迟delay执行
    if(t_current - t_start > must) {
      fn.apply(context, args)
      t_start = t_current
    } else {
      timer = setTimeout(() => {
        fn.apply(context, args)
      }, delay)
    }
  }
}
```

此时，我们应该对`scroll`事件绑定节流事件才对。

```js
init() {
  ...
  this._throttleFn = this.throttle(this.inViewShow)
  document.addEventListener('scroll', this._throttleFn)
  ...
}
// 卸载事件此时应该替换成
document.removeEventListener('scroll', this._throttleFn)
```

就这样，晚安！！:crescent_moon:
