---
title: ES6 之箭头函数
date: 2019-02-06 17:47:51
tags:
  - JavaScript
  - ES6
categories:
  - JavaScript
---

为了梳理知识点，写一下 8102 年的箭头函数  
(ps:我写分号了，不过被我配置的 ide 的格式规则给抹平了)

## 基本用法

```js
const fun = value => value
// 等价于
const fun = function(value) {
  return value
}
// 多个参数并需要返回对象的情况下可以这样
// 两个参数的默认值为0，es6设置默认值的方法，解构赋值时也可以设置默认值
const fun = (a = 0, b = 0) => ({ total: a + b })
// 参数解构并设置默认值的写法
const fun = ({ a = 0, b = 0 }) => ({ total: a + b })
// 可以这样调用
const ret = fun({ a: 10, b: 20 })
//或使用默认值
const ret = fun()
```

## 差异

### 1.不能被 new

就是不能通过 new 关键字被调用, js 函数的内部属性中包含了[[Construct]],当 new 实例发生时，调用该方法。  
通俗点讲就是 new 操作符执行了一个内部的 function 从而创建了一个空对象，这个对象原型指向构造函数的 prototype，执行构造函数后返回这个对象（return this）。
but，箭头函数没有这个构造器，所以 new 时会抛出一个错误

### 2.没有原型

不能被 new，就没有构造原型的必要，不难理解，才导致了没有 prototype 的

### 3.不能被 super

既然原型都没得，肯定也不能用 super 访问原型属性。but，要值得说下的是，super 和 this 类似，取的是最近一层非箭头函数的值

### 4.没有 arguments

没有此内部属性，同 super 和 this 一样，取最近一层函数的 arguments

### 5. <b>没有 this</b>

这里引用冴羽大大的例子，[原文在这里](https://juejin.im/post/5b14d0b4f265da6e60393680)

箭头函数没有 this，所以需要通过查找作用域链来确定 this 的值。

这就意味着如果箭头函数被非箭头函数包含，this 绑定的就是最近一层非箭头函数的 this。

模拟一个实际开发中的例子：

我们的需求是点击一个按钮，改变该按钮的背景色。

为了方便开发，我们抽离一个 Button 组件，当需要使用的时候，直接：

```js
// 传入元素 id 值即可绑定该元素点击时改变背景色的事件
new Button('button')
```

```html
<button id="button">点击变色</button>
```

js 是下面这样的

```js
function Button(id) {
  this.element = document.querySelector('#' + id)
  this.bindEvent()
}

Button.prototype.bindEvent = function() {
  this.element.addEventListener('click', this.setBgColor, false)
}

Button.prototype.setBgColor = function() {
  this.element.style.backgroundColor = '#1abc9c'
}

var button = new Button('button')
```

看着好像没有问题，结果却是报错 <font color="red">Uncaught TypeError: Cannot read property 'style' of undefined</font>

这是因为当使用 addEventListener() 为一个元素注册事件的时候，事件函数里的 this 值是该元素的引用。

所以如果我们在 setBgColor 中 console.log(this)，this 指向的是按钮元素，那 this.element 就是 undefined，报错自然就理所当然了。

也许你会问，既然 this 都指向了按钮元素，那我们直接修改 setBgColor 函数为：

```js
Button.prototype.setBgColor = function() {
  this.style.backgroundColor = '#1abc9c'
}
```

不就可以解决这个问题了？

确实可以这样做，但是在实际的开发中，我们可能会在 setBgColor 中还调用其他的函数，比如写成这种：

```js
Button.prototype.setBgColor = function() {
  this.setElementColor()
  this.setOtherElementColor()
}
```

所以我们还是希望 setBgColor 中的 this 是指向实例对象的，这样就可以调用其他的函数。

利用 ES5，我们一般会这样做：

```js
Button.prototype.bindEvent = function() {
  this.element.addEventListener('click', this.setBgColor.bind(this), false)
}
```

为避免 addEventListener 的影响，使用 bind 强制绑定 setBgColor() 的 this 为实例对象

使用 ES6，我们可以更好的解决这个问题：

```js
Button.prototype.bindEvent = function() {
  this.element.addEventListener('click', event => this.setBgColor(event), false)
}
```

由于箭头函数没有 this，所以会向外层查找 this 的值，即 bindEvent 中的 this，此时 this 指向实例对象，所以可以正确的调用 this.setBgColor 方法， 而 this.setBgColor 中的 this 也会正确指向实例对象。

在这里再额外提一点，就是注意 bindEvent 和 setBgColor 在这里使用的是普通函数的形式，而非箭头函数，如果我们改成箭头函数，会导致函数里的 this 指向 window 对象 (非严格模式下)。

最后，因为箭头函数没有 this，所以也不能用 call()、apply()、bind() 这些方法改变 this 的指向，可以看一个例子：

```js
var value = 1
var result = (() => this.value).bind({ value: 2 })()
console.log(result) // 1
```
