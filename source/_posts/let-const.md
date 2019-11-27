---
title: ES6 之 let & const
date: 2019-02-06 17:48:58
tags:
  - JavaScript
  - ES6
---

# 浅析 let 与 const

今天学习一下 ES6 的两种声明方式，得先从说`var`说起,

## 先说`var`

```js
if (a === true) {
  var value = 1
}
// 可以变形成
var value
if (a === true) {
  value = 1
}
```

只要不是新手，都应该明白这两个代码块是相等的。为什么呢,因为`var`声明存在变量提升，此时如果`a !== true`成立时,则`value`应该等于`undefined`,同理，`var`声明的`function`也是存在变量提升行为的。下面是一个 for 循环

```js
for(var i = 0; i < 10; i++) {
  ...
}
console.log(i) // 10
```

<!--more-->

我们知道,即使循环结束了我们仍然可以访问到`i`,此时控制台打印的是 10，为了解决这个问题，ES6 引入了块级作用域。

## let & const

### 1.无变量提升行为

```js
if (false) {
  let value = 1
  const value2 = 2
}
console.log(value) // Uncaught ReferenceError: value is not defined
```

### 2.重复声明报错

```js
var value = 1
let value = 1 // Uncaught SyntaxError: Identifier 'value' has already been declared
const ...
```

### 3.暂时死区

`let` 和 `const`声明不会有变量提升存在，在声明之前访问这个变量会导致报错，称之为`暂时死区`

```js
console.log(value) // test.html:63 Uncaught ReferenceError: value is not defined
let value = 1
// const 与之同理
```

### 4.块级作用域

```js
{
  let value = 10
  console.log(value) // 10
}
{
  let value = 20
  console.log(value) // 20
}
// const 与之同理
```

### 5.const 只能在声明的时候赋值

```js
const value
value = 10 // Uncaught SyntaxError: Missing initializer in const declaration
```

## 实践

个人觉得在实际开发中更应使用`const`保持变量的不变，在变量需要改变时才使用`let`,这样数据初始化后不会改变，避免了很多 bug 产生。

晚安 🌛
